title: 哪些些JS属性/方法会导致重绘
date: 2018-05-15 13:53:51
tags:
- 技术细节
- reflow
- 前端性能
categories:
- Js
---

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
 {% img [Timeline trace of The Guardian. Outbrain is forcing layout repeatedly, probably in a loop.] http://om64pi295.bkt.clouddn.com/timeline-trace-of-the-guardian.png %}


