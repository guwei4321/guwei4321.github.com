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

## getComputedStyle

`window.getComputedStyle()` 会触发典型的样式重新计算
`window.getComputedStyle()` 会触发重绘:

1. 任何`Shadow DOM`
2. 使用了 media queries （viewport-related中的其中一些）,以下属性
    * `min-width`, `min-height`, `max-width`, `max-height`, `width`, `height`
    * `aspect-ratio`, `min-aspect-ratio`, `max-aspect-ratio`
    * `device-pixel-ratio`, `resolution`, `orientation` , `min-device-pixel-ratio`, `max-device-pixel-ratio`
2. 获取以下任一属性
    * 

