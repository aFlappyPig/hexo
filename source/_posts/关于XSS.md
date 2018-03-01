---
title: 关于XSS
date: 2017-10-15 20:05:32
tags: [WEB安全]
---

### XSS 简介

跨站脚本攻击(Cross Site Scripting)，为了不和层叠样式表(Cascading Style Sheets, CSS)的缩写混淆，故将跨站脚本攻击缩写为 XSS。恶意攻击者往 Web 页面里插入恶意 Script 代码，当用户浏览该页之时，嵌入其中 Web 里面的 Script 代码会被执行，从而达到恶意攻击用户的目的。跨站点脚本（XSS）攻击发生在以下情况下：

* 1.数据通过不受信任的来源进入 Web 应用程序，通常是 Web 请求。
* 2.数据包含在发送给 Web 用户而不经过恶意内容验证的动态内容中。

发送到 Web 浏览器的恶意内容通常采用 JavaScript 段的形式，但也可能包括 HTML，Flash 或浏览器可能执行的任何其他类型的代码。基于 XSS 的各种攻击几乎是无限的，但它们通常包括向攻击者发送隐私数据（如 cookies 或其他会话信息），将受害者重定向到由攻击者控制的网页内容，或在用户计算机上执行其他恶意操作在易受攻击的网站的幌子下。

### XSS 分类

* 1.反射型
又称为非持久性跨站点脚本攻击，它是最常见的类型的 XSS。漏洞产生的原因是攻击者注入的数据反映在响应中。一个典型的非持久性 XSS 包含一个带 XSS 攻击向量的链接(即每次攻击需要用户的点击)。
{% blockquote %}
简单例子
正常发送消息：
{% codeblock %}
http://www.test.com/message.php?send=Hello,World!
{% endcodeblock %}
接收者将会接收信息并显示 Hello,Word
非正常发送消息：
{% codeblock %}
http://www.test.com/message.php?send=<script>alert(‘foolish!’)</script>！
{% endcodeblock %}
接收者接收消息显示的时候将会弹出警告窗口
{% endblockquote %}

* 2.存储型
又称为持久型跨站点脚本，它一般发生在 XSS 攻击向量(一般指 XSS 攻击代码)存储在网站数据库，当一个页面被用户打开的时候执行。每当用户打开浏览器,脚本执行。持久的 XSS 相比非持久性 XSS 攻击危害性更大,因为每当用户打开页面，查看内容时脚本将自动执行。
{% blockquote %}
简单例子
存储型 XSS 攻击就是将攻击代码存入数据库中，然后客户端打开时就执行这些攻击代码。例如留言板：留言板表单中的表单域：
{% codeblock %}
<input type=“text” name=“content” value=“这里是用户填写的数据”>
{% endcodeblock %}
正常操作：用户是提交相应留言信息；将数据存储到数据库；其他用户访问留言板，应用去数据并显示。接收者将会接收信息并显示 Hello,Word
非正常操作：攻击者在 value 填写
{% codeblock %}
<script>alert(‘foolish!’)</script>
{% endcodeblock %}
【或者 html 其他标签（破坏样式。。。）、一段攻击型代码】；将数据存储到数据库中；其他用户取出数据显示的时候，将会执行这些攻击性代码
{% endblockquote %}

* 3.DOM Based
基于DOM的XSS有时也称为type0XSS。当用户能够通过交互修改浏览器页面中的DOM(DocumentObjectModel)并显示在浏览器上时，就有可能产生这种漏洞，从效果上来说它也是反射型XSS。
通过修改页面的DOM节点形成的XSS，称之为DOMBasedXSS。
前提是易受攻击的网站有一个HTML页面采用不安全的方式从document.location 或document.URL 或 document.referrer获取数据（或者任何其他攻击者可以修改的对象）。

### XSS防范
* 1.不要在页面中插入任何不可信数据
* 2.在将不可信数据插入到HTML标签之间时，对这些数据进行HTML Entity编码
* 3.在将不可信数据插入到HTML属性里时，对这些数据进行HTML属性编码
* 4.在将不可信数据插入到SCRIPT里时，对这些数据进行SCRIPT编码
* 5.在将不可信数据插入到Style属性里时，对这些数据进行CSS编码
* 6.在将不可信数据插入到HTML URL里时，对这些数据进行URL编码
* 7.使用富文本时，使用XSS规则引擎进行编码过滤

总结：对于XSS防范，前端主要是针对允许直接放入html的地方进行限制，例如AngularJs中的ng-html或者是其他template的地方，如果设置为允许接受html，需要先用ngSanitize模块的$sce服务进行html模板信任操作，避免存在恶意代码，Vue中v-html也存在类似的操作。目前主流框架中基本上默认都对可能存在XSS攻击的地方进行了防护，但是开发者仍然需要注意，提高防范意识，防范于未然。