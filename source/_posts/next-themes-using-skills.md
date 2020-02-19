---
title: Next 使用技巧
art: a
id: 16062510
comments: false
tags:
  - hexo
  - next
date: 2016-06-25 10:35:54
categories: 教程
---



「标签」(Tag Plugin) 是 Hexo 提供的一种快速生成特定内容的方式。 例如，在标准 Markdown 语法中，我们无法指定图片的大小。这种情景，我们即可使用标签来解决。 Hexo 内置来许多标签来帮助写作者可以更快的书写， 完整的标签列表 可以参考 Hexo 官网。 另外，Hexo 也开放来接口给主题，使主题有可能提供给写作者更简便的写作方法。 以下标签便是 NexT 主题当前提供的标签。

<!-- more -->


#### 文本居中的引用

> 此标签将生成一个带上下分割线的引用，同时引用内文本将自动居中。 文本居中时，多行文本若长度不等，视觉上会显得不对称，因此建议在引用单行文本的场景下使用。 例如作为文章开篇引用 或者 结束语之前的总结引用。


##### 使用方式

- HTML方式：使用这种方式时，给 img 添加属性 class="blockquote-center" 即可。
- 标签方式：使用 centerquote 或者 简写 cq。


```
<!-- HTML方式: 直接在 Markdown 文件中编写 HTML 来调用 -->
<!-- 其中 class="blockquote-center" 是必须的 -->
<blockquote class="blockquote-center">blah blah blah</blockquote>

<!-- 标签 方式，要求版本在0.4.5或以上 -->
{% centerquote %}blah blah blah{% endcenterquote %}

<!-- 标签别名 -->
{% cq %} blah blah blah {% endcq %}


```

##### 效果示例

{% cq %} 随时随地发现新鲜事!微博带你欣赏世界上每一个精彩瞬间,了解每一个幕后故事。分享你想表达的,让全世界都能听到你的心声! {% endcq %}


#### 突破容器宽度限制的图片

> 当使用此标签引用图片时，图片将自动扩大 26%，并突破文章容器的宽度。 此标签使用于需要突出显示的图片, 图片的扩大与容器的偏差从视觉上提升图片的吸引力。 此标签有两种调用方式（详细参看底下示例）：


##### 使用方式

- HTML方式：使用这种方式时，为 img 添加属性 class="full-image"即可。
- 标签方式：使用 fullimage 或者 简写 fi， 并传递图片地址、 alt 和 title 属性即可。 属性之间以逗号分隔。


```
<!-- HTML方式: 直接在 Markdown 文件中编写 HTML 来调用 -->
<!-- 其中 class="full-image" 是必须的 -->
<img src="/image-url" class="full-image" />

<!-- 标签 方式，要求版本在0.4.5或以上 -->
{% fullimage /image-url, alt, title %}

<!-- 别名 -->
{% fi /image-url, alt, title %}

```


##### 默认
![12345](http://wx3.sinaimg.cn/mw690/005MoifUly1fgvlkibgcrj31kw0zk191.jpg)


##### 效果示例
{% fi http://wx3.sinaimg.cn/mw690/005MoifUly1fgvlkibgcrj31kw0zk191.jpg, alt, 这是 %}


#### Bootstrap Callout

##### 使用方式

```
{% note class_name %} Content (md partial supported) {% endnote %}

```
> 其中，class_name 可以是以下列表中的一个值：

- default
- primary
- success
- info
- warning
- danger

##### 效果示例


{% note default %}  default Content{% endnote %}

{% note primary %}  primary Content{% endnote %}

{% note success %}  success Content{% endnote %}

{% note info %}  info Content{% endnote %}

{% note warning %}  warning Content{% endnote %}

{% note danger %}  danger Content{% endnote %}



#### 参考连接

- [Next 内置标签](http://theme-next.iissnan.com/tag-plugins.html)



