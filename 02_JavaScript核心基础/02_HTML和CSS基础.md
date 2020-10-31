<h1 id="HTML和CSS基础" align="center">HTML 和 CSS 基础</h1>
<!-- @import "[TOC]" {cmd="toc"} -->

<!-- code_chunk_output -->

- [1. HTML （Hyper Text Markup Language 超文本标记语言）](#1-html-hyper-text-markup-language-超文本标记语言)
  - [1.1. 简介](#11-简介)
    - [1.1.1. Hello HTML](#111-hello-html)
    - [1.1.2. <!doctype> 声明](#112-doctype-声明)
    - [1.1.3. \<meta> 标签](#113-meta-标签)
  - [1.2. 常用标签](#12-常用标签)
  - [1.3. HTML 字符实体（转义字符）](#13-html-字符实体转义字符)
- [2. CSS （Cascading Style Sheets 层叠样式表）](#2-css-cascading-style-sheets-层叠样式表)
- [3. 网页布局](#3-网页布局)

<!-- /code_chunk_output -->


# 1. HTML （Hyper Text Markup Language 超文本标记语言）

## 1.1. 简介

- 1993年6月：HTML第一个版本发布
- 1995年11月：HTML2.0
- 1997年1月：HTML3.2（W3C推荐）
- 1999年12月：HTML4.01（W3C推荐）
- 2000年底：XHTML1.0（W3C推荐）
- 2014年10月：HTML5（W3C推荐

### 1.1.1. Hello HTML

纯文本：只能保存文本内容，无法保存图片、音视频、文字大小颜色等格式化的内容。

```html
<!doctype html>
<html>
    <head>
        <meta charset="utf-8" />
        <title>HTML</title>
    </head>
    <body>
        <h1>hello, html</h1>
        <!-- 这是一个注释-->
    </body>
</html>
```

在 HTML 中，使用一对尖括号包裹起来的内容叫做标签 `<标签名>`；标签分为两种：一种是由一对开始标签和结束标签组成的（`<xxx></xxx>`），另一种仅需单个标签（`<xxx />`）
- html 根标签：一个页面中有且仅有一个根标签，网页中所以内容都必须写在 html 标签中。
- head 标签：该标签中的内容用于帮助浏览器解析页面。
- title 标签：网页的标题，搜索引擎会检索title。有专门的SEO会去优化title标签中的内容。
- body 标签：网页中所有可见内容都写在此处。

在标签中，可以写上标签的各种属性 `<标签名 属性1="值1" 属性2='值2'>`

### 1.1.2. <!doctype> 声明
<!doctype>声明有助于浏览器中正确显示网页。
```html
<!-- HTML5 -->
<!doctype html>
<!-- HTML 4.01 -->
<!doctype HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN"
"http://www.w3.org/TR/html4/loose.dtd">
<!-- XHTML 1.0 -->
<!doctype html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN"
"http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
```

### 1.1.3. \<meta> 标签
`<meta>` 标签可提供有关页面的元信息，放在 `<head>` 标签中。
```html
<!-- 设置页面的字符集 -->
<meta charset="utf-8">
<!-- 设置网页的描述 -->
<meta name="description" content="我的第一个网页">
<!-- 设置网页的关键字 -->
<meta name="keywords" content="fist web page">
<!-- 请求的重定向。5秒后跳转 -->
<meta http-equiv="refresh" content="5;url=http://www.baidu.com"/>
```
搜索引擎会检索description和keywords信息，但是这两者不会影响网页的排名。

## 1.2. 常用标签

**标题标签**
标题标签是一种语义化标签。从 h1-h6 重要性越来越低，会影响到网页的搜索。
```html
<h1>一级标题</h1>
<h2>二级标题</h2>
<h3>三级标题</h3>
<h4>四级标题</h4>
<h5>五级标题</h5>
<h6>六级标题</h6>
```

**段落标签**
`<p>`表示一个段落，段落与段落之间会有一定的段间距。标签是一种语义化标签。
`<br />`标签表示换行，换行后行与行之间不会有段间距。
`<hr />`标签是水平线标签。
```html
<p>第一段</p>
<p>
    第二段，第一行<br />
    第二段，第二行
    <hr />
    第二段，第三行
</p>
```

**文本格式化标签**
`<b>`:定义粗体文本
`<em>`:定义着重文字
`<i>`:定义斜体字
`<small>`:定义小号字
`<strong>`:定义加重语气
`<sub>`:定义下标字
`<sup>`:定义上标字
`<ins>`:定义插入字
`<del>`:定义删除字

**"计算机输出" 标签**
`<code>`:定义计算机代码
`<kbd>`:定义键盘码
`<samp>`:定义计算机代码样本
`<var>`:定义变量
`<pre>`:定义预格式文本

**引文, 引用, 及标签定义**
`<abbr>`:定义缩写
`<address>`:定义地址
`<bdo>`:定义文字方向
`<blockquote>`:定义长的引用
`<q>`:定义短的引用语
`<cite>`:定义引用、引证
`<dfn>`:定义一个定义项目。

**图片标签**
`<img />` 标签是图片标签，用来引入一张外部的图片。
- src: 指向一个外部的图片的路径。
- alt: 图片的描述。在图片没有显示时出现。提供给搜索引擎抓取使用。
- width: 图片显示宽度。
- heigth: 图片显示高度。当宽高只设置一个时，等比例缩放。
```html
<img src="xxx" alt="xxx" />
```

路径：
./ 表示当前目录，可省略
../ 表示上一级路目录
/ 表示根目录

**内联框架**
`<iframe src="01html.html"></iframe>`
不建议使用内联框架，因为内联框架中的内容不会被搜索引擎所检索。
- name 规定 iframe 的名称。
- frameborder 1/0 是否显示框架周围的边框。

**超链接标签**
`<a href="#">xxx</a>`
- href 指向一个链接地址
- target 设置打开目标页面的位置，可选值：
  - _blank    在新窗口中打开被链接文档。
  - _self     默认。在相同的框架中打开被链接文档。
  - _parent   在父框架集中打开被链接文档。
  - _top      在整个窗口中打开被链接文档。
  - framename 在指定的框架中打开被链接文档。

href="#" 与 href="" 的区别：
两者表现出跳转到当前页面。
""会重新加载页面。
"#"只是浏览器链接到TOP锚点处，不会加载页面。页面会向上滚到页首，# 默认锚点为 #TOP。

## 1.3. HTML 字符实体（转义字符）
```html
&entity_name;
<!-- 或者 -->
&#entity_number;
```
空格    `&nbsp;`
小于号  `&lt;`
大于号  `&gt;`
版权符  `&copy;`

# 2. CSS （Cascading Style Sheets 层叠样式表）

1. 内联样式：写在标签的 style 属性中，只对当前标签起作用。
2. 内部样式：写在 `<style type="text/css"></style>` 标签中。
3. 外部样式：写在外部 css 文件中，使用 `<link href="xxx" rel="stylesheet" type="text/css" />` 引入。

浏览器可以缓存外部样式表，可以加快网页的访问速度。


CSS 注释：
```css
/* 这是 CSS 的注释 */
```

CSS 语法
```css
选择器 {  /* 声明块 */
  键1: 值1; /* 声明1 */
  键2: 值2; /* 声明2 */
}
```

选择器
1. 标签选择器 `标签名`
2. 类选择器   `.类名`
3. ID选择器   `#ID`

# 3. 网页布局