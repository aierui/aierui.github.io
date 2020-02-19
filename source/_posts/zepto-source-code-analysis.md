---
title: Zepto 源码分析
art: a
id: 17070716
comments: false
tags:
  - zepto
date: 2017-07-08 00:12:36
categories: 源码分析
---



{% cq %} 
想 游刃有余 如鱼得水
得 一清二楚 机制原理
{% endcq %}

```js
var Zepto = (function() {
	// core
})()

// If `$` is not yet defined, point it to `Zepto`
window.Zepto = Zepto
window.$ === undefined && (window.$ = Zepto)
```

<!-- more -->
Zepto是一个轻量级的针对现代高级浏览器的JavaScript库， 它与jquery有着类似的api， 但主要针对移动端。


这两天看了看 Zepto 源码本想着自己分析下 Zepto 的代码结构、内部工具函数、文档操作等，但是发现下面这位前辈分析的更加透彻，在这里分享下。但是前辈目前只是分享了 Zepto.js 未分享其他模块、如 Ajax 、 Event 、 Detect 、 Touch 、 Effects 、 Form 等


详情见 [读 Zepto 源码系列](https://github.com/yeyuqiudeng/reading-zepto) 👍