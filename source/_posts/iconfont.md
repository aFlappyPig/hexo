---
title: iconfont
date: 2018-03-01 10:02:36
tags: [相关技术, SVG]
---

### 前言

这是一篇关于 iconfont 的理解和总结。[iconfont](http://www.iconfont.cn) 是阿里巴巴提供的一个在线选取和制作字体图标的网站，该网站提供了很多官网网站的图标供免费下载和使用，为字体图标的使用和推广提供了良好的社区环境。该站提供了 web 端，Android 端和 IOS 端三种使用字体图标的情况和代码示例。当然，本文只探讨 web 端。

### iconfont 在 web 中的使用

在 web 中使用 iconfont 主要有三种方式：

#### 直接使用 Unicode 码点

unicode 是字体在网页端最原始的应用方式，特点如下：

* 兼容性最好，支持 ie6+，及所有现代浏览器。
* 支持按字体的方式去动态调整图标大小，颜色等等。
* 但是因为是字体，所以不支持多色。

使用步骤如下：
第一步：拷贝项目下面生成的 font-face
{% codeblock lang:css %}
@font-face {
    font-family: 'iconfont';
    src: url('iconfont.eot');
    src: url('iconfont.eot?#iefix') format('embedded-opentype'),
    url('iconfont.woff') format('woff'),
    url('iconfont.ttf') format('truetype'),
    url('iconfont.svg#iconfont') format('svg');
}
{% endcodeblock %}
第二步：定义使用 iconfont 的样式
{% codeblock lang:css %}
.iconfont{
    font-family:"iconfont" !important;
    font-size:16px;font-style:normal;
    -webkit-font-smoothing: antialiased;
    -webkit-text-stroke-width: 0.2px;
    -moz-osx-font-smoothing: grayscale;
}
{% endcodeblock %}
第三步：挑选相应图标并获取字体编码，应用于页面
{% codeblock lang:html %}
<i class="iconfont">&#x33;</i>
{% endcodeblock %}

#### font-class 引用

font-class 是 unicode 使用方式的一种变种，主要是解决 unicode 书写不直观，语意不明确的问题。与 unicode 使用方式相比，具有如下特点：

* 兼容性良好，支持 ie8+，及所有现代浏览器。
* 相比于 unicode 语意明确，书写更直观。
* 因为使用 class 来定义图标，所以当要替换图标时，只需要修改 class 里面的 unicode 引用。
* 不过因为本质上还是使用的字体，所以多色图标还是不支持的。

使用步骤如下：
第一步：拷贝项目下面生成的 fontclass 代码：
{% codeblock lang:html %}
//at.alicdn.com/t/font_8d5l8fzk5b87iudi.css
{% endcodeblock %}
第二步：挑选相应图标并获取类名，应用于页面：
{% codeblock lang:html %}
<i class="iconfont icon-xxx"></i>
{% endcodeblock %}

#### symbol 引用

这是一种全新的使用方式，应该说这才是未来的主流，也是平台目前推荐的用法。相关介绍可以参考这篇文章 这种用法其实是做了一个 svg 的集合，与上面两种相比具有如下特点：

* 支持多色图标了，不再受单色限制。
* 通过一些技巧，支持像字体那样，通过 font-size,color 来调整样式。
* 兼容性较差，支持 ie9+,及现代浏览器。
* 浏览器渲染 svg 的性能一般，还不如 png。

使用步骤如下：
第一步：拷贝项目下面生成的 symbol 代码：
{% codeblock lang:html %}
//at.alicdn.com/t/font_8d5l8fzk5b87iudi.js
{% endcodeblock %}
第二步：加入通用 css 代码（引入一次就行）：
{% codeblock lang:css %}
.icon {
    width: 1em; height: 1em;
    vertical-align: -0.15em;
    fill: currentColor;
    overflow: hidden;
}
{% endcodeblock %}
第三步：挑选相应图标并获取类名，应用于页面：
{% codeblock lang:html %}
<svg class="icon" aria-hidden="true">
    <use xlink:href="#icon-xxx"></use>
</svg>
{% endcodeblock %}

### 总结
以上三种方式各有优劣，在选取技术方案时需要结合项目的兼容性需求和开发体验（打包工具的协作）进行综合考虑。一般建议方案三。
常见的bootstrap使用的是方案二，这也是大多数css框架采用的方案（兼容性+使用方便），毕竟css框架都不提供彩色图标。
移动端如果有优化加载字体大小和速度的需求，可使用[字蛛](http://font-spider.org/)工具优化，但和打包工具的结合使用可能会有些麻烦。
对于方案三，其实用的是字体图标图片的svg格式，便于操作和调整，设计软件可直接导出svg格式，也可从网站上下载svg文件使用，如果必要，svg也可以采用懒加载的方式引入。
对于方案一和方案二，前提是先引入iconfont字体图标字体文件：比如'&#xe7bb'; 其中&#是开头用以标明这是字符实体，x表示这是十六进制，而CSS的伪元素的content接受的也是16进制的Unicode编码，所以可以直接写 content: "\e7bb"。