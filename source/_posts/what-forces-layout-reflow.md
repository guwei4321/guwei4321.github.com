title: 哪些会导致重绘和回流
date: 2018-05-15 13:53:51
tags:
- 技术细节
- reflow
- 前端性能
categories:
- Js
---


{% blockquote paulirish/what-forces-layout, https://gist.github.com/paulirish/5d52fb081b3570c81e3a %}
翻译自 《What forces layout / reflow》
{% endblockquote %}

当调用以下所有属性/方法时，会触发浏览器重绘。重绘就是让让浏览器同步计算样式和布局，通常是浏览器性能瓶颈。
<!--more-->
## Element

### 盒子度量
* `elem.offsetLeft`, `elem.offsetTop`, `elem.offsetWidth`, `elem.offsetHeight`, `elem.offsetParent`
* `elem.clientLeft`, `elem.clientTop`, `elem.clientWidth`, `elem.clientHeight`
* `elem.getClientRects()`, `elem.getBoundingClientRect()`

### Scroll之类的
* `elem.scrollBy()`, `elem.scrollTo()`
* `elem.scrollIntoView()`, `elem.scrollIntoViewIfNeeded()`
* `elem.scrollWidth`, `elem.scrollHeight`
* `elem.scrollLeft`, `elem.scrollTop` 设置也会影响他们

### Focus
* `elem.focus()` 会触发两次重绘 [source](https://cs.chromium.org/chromium/src/third_party/WebKit/Source/core/dom/Element.cpp?q=updateLayoutIgnorePendingStylesheets+-f:out+-f:test&sq=package:chromium&dr=C)&l=2923

### 还有...
* `elem.computedRole`, `elem.computedName`
* `elem.innerText` [source](https://cs.chromium.org/chromium/src/third_party/WebKit/Source/core/dom/Element.cpp?q=updateLayoutIgnorePendingStylesheets+-f:out+-f:test&sq=package:chromium&dr=C)&l=3440

### getComputedStyle

`window.getComputedStyle()` 会触发典型的样式重新计算
`window.getComputedStyle()` 会触发重绘:

1. 任何`Shadow DOM`
2. 使用了 media queries （viewport-related中的其中一些）,以下属性
    * `min-width`, `min-height`, `max-width`, `max-height`, `width`, `height`
    * `aspect-ratio`, `min-aspect-ratio`, `max-aspect-ratio`
    * `device-pixel-ratio`, `resolution`, `orientation` , `min-device-pixel-ratio`, `max-device-pixel-ratio`
2. 获取以下任一属性
    * `height`, `width`
    * `top`, `right`, `bottom`, `left`
    * `margin` [`-top`, `-right`, `-bottom`, `-left`, 或者简写] 仅当`margin`是固定的.
    * `padding` [`-top`, `-right`, `-bottom`, `-left`, 或者简写] 仅当`padding`是固定的.
    * `transform`, `transform-origin`, `perspective-origin`
    * `translate`, `rotate`, `scale`
    * `grid`, `grid-template`, `grid-template-columns`, `grid-template-rows`
    * `perspective-origin`
    * 以下这些项目出现在列表中，但现在看来已经不存在了。 (截至2018年2月): `motion-path`, `motion-offset`, `motion-rotation`, `x`, `y`, `rx`, `ry`

### window
1. `window.scrollX`, `window.scrollY`
2. `window.innerHeight`, `window.innerWidth`
3. `window.getMatchedCSSRules()` 仅重新计算样式

### Forms
1. `inputElem.focus()`
2. `inputElem.select()`, `textareaElem.select()`

### 鼠标事件
1. `mouseEvt.layerX`, `mouseEvt.layerY`, `mouseEvt.offsetX`, `mouseEvt.offsetY`

### document
1. `doc.scrollingElement` 仅重新计算样式

### Range
1. `range.getClientRects()`, `range.getBoundingClientRect()`

### SVG
1. 很多，没有详尽的列表，但是(Tony Gentilcore's 2011 Layout Triggering List )[http://gent.ilcore.com/2011/03/how-not-to-trigger-layout-in-webkit.html]列出了一些。

### contenteditable
1. 很多很多。

## 附录
* 回流只在Dom结构有改变会有一定性能消耗，在设置无效的样式和布局的时候会触发。通常，部分原因是因为DOM节点变化（改变类名，增加/删除节点，甚至是增加伪元素，譬如:focus）
* 如果布局发生变化，样式肯定会重新计算。所以重绘会触发布局以及样式的重新计算。重绘的消耗非常依赖于内容/位置的变化，但是这两个的消耗又差不多。
* 改怎么避免回流跟重绘呢？
    1. 尽量避免在 for 循环中重绘和改变DOM
    2. 使用 DevTools Timeline，分析页面加载或用户交互后的每个时间。你可能会发现很多意想不到的事情。
    3. 批处理读/写DOM，可以使用(FastDom)[https://github.com/wilsonpage/fastdom]或者虚拟DOM

### 浏览器兼容性
    因为每个浏览器渲染页面原理都不一样，所以使用 Chrome 的 DevTools看到的数据不一定在每个浏览器都适用。

## CSS Triggers
(CSS Triggers)[https://csstriggers.com/] 列出了在各个引擎下，Js设置/改变CSS的值时候是否触发回滚的情况，使用三种色块来表示是否会触发Layout/Paint/Composite。

## 更多参考资料
* [Avoiding layout thrashing — Web Fundamentals](https://developers.google.com/web/fundamentals/performance/rendering/avoid-large-complex-layouts-and-layout-thrashing?hl=en)
* [Fixing Layout thrashing in the real world | Matt Andrews](https://mattandre.ws/2014/05/really-fixing-layout-thrashing/)
* [Timeline demo: Diagnosing forced synchronous layouts - Google Chrome](https://developer.chrome.com/devtools/docs/demos/too-much-layout)
* [Preventing &apos;layout thrashing&apos; | Wilson Page](http://wilsonpage.co.uk/preventing-layout-thrashing/)
* [wilsonpage/fastdom](https://github.com/wilsonpage/fastdom)
* [Rendering: repaint, reflow/relayout, restyle / Stoyan](http://www.phpied.com/rendering-repaint-reflowrelayout-restyle/)
* [We spent a week making Trello boards load extremely fast. Here’s how we did it. - Fog Creek Blog](http://blog.fogcreek.com/we-spent-a-week-making-trello-boards-load-extremely-fast-heres-how-we-did-it/)
* [Minimizing browser reflow  |  PageSpeed Insights  |  Google Developers](https://developers.google.com/speed/articles/reflow?hl=en)
* [Optimizing Web Content in UIWebViews and Websites on iOS](https://developer.apple.com/videos/wwdc/2012/?id=601)
* [Accelerated Rendering in Chrome](http://www.html5rocks.com/en/tutorials/speed/layers/)
* [web performance for the curious](https://www.igvita.com/slides/2012/web-performance-for-the-curious/)
* [Jank Free](http://jankfree.org/)


