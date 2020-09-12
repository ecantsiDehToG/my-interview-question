
web语义化
布局方式
水平垂直居中
多盒子水平垂直排布
bfc块级格式化上下文
高度塌陷 清除浮动
display/opcity/visibility
**1.谈谈html的语义化，h5新标签新特性**

  + 让代码结构更加清晰，利于阅读和维护

  + 对seo更加友好

  + h5新增了语义化标签：header、footer、section、nav、aside、article等（便于阅读，同时也有利于seo）

  + 

**3. img 的 alt 与 title 有何异同?strong 与 em 的异同?**

alt 属性是在你的图片因为某种原因不能加载时在页面显示的提示信息,它会直接输出在原本加载图片的地方

title 属性是在你鼠标悬停在该图片上时显示一个小提示,鼠标离开就没有了,有点类似 jQuery 的 hover.HTML 的绝大多数标签 都支持 title 属性,title 属性就是专门做提示信息的.

默认显示样式上:<em>默认样式是斜体,<strong>默认样式是粗体.

语义上:<em>用来局部强调,<strong>则是全局强调.从视觉上考虑,<em>的强调是有顺序的,阅读到某处时,才会注意 到.<strong>的强调则是一种随意无顺序的,看见某文时,立刻就凸显出来的关键词句

【注】:在实际开发中,我们看到一般大网站 都只是使用<strong>标签就够了,比较少使用<em>标签.此外,使用<em>和<strong>这两个标签可以为 SEO 关键词排名加分.

**4. 行内元素和块级元素的具体区别是什么**

一、块级元素：blockelement 块级元素默认占一整行,可嵌套块级元素或行内元素;一般作为容器出现,用来组织结构（特例:<form>只能包含块级元素）

二、行内元素：也叫内联元素、内嵌元素等;行内元素一般都是基于语义级(semantic)的基本元素,只能容纳文本或其他内联元素

三、block(块)元素的特点

①、总是在新行上开始;

②、高度,行高以及外边距和内边距都可控制;

③、宽度缺省是它的容器的 100%,除非设定一个宽度;

④、它可以容纳内联元素和其他块元素;

四、inline 元素的特点

①、和其他元素都在一行上;

②、高,行高及外边距和内边距不可改变;

③、宽度就是它的文字或图片的宽度,不可改变;

④、内联元素只能容纳文本或者其他内联元素 对行内元素,需要注意如下 设置宽度 width 无效. 设置高度 height 无效,可以通过 line-height 来设置. 设置 margin 只有左右 margin 有效,上下无效. 设置 padding 只有左右 padding 有效,上下则无效.注意元素范围是增大了,但是对元素周围的内容是没影响的.

五、常见的块状元素 Address、blockquote、center、dir、div、dl、fieldset、form 、h1 到 h6、hr、input、prompt、 menu、noframes、noscript、 ol、p、pre、table、ul

六、常见的内联元素 a、abbr、acronym、b、bdo、big、br、cite、code、dfn、em、font、i、img、input、kbd、label、q、s、samp、select、 small、span、strike、strong、sub、sup、textarea、u

七、空元素 没有内容的 HTML 内容被称为空元素.空元素是在开始标签中关闭的.<br/><hr><input><img>

**5. 如何垂直居中一个浮动元素?**

1.已知浮动元素的宽高

​ 设定父元素为相对定位,浮动元素为绝对定位 top 和 left 为 50%,再设置浮动元素的 margin-left/top 值为浮动元素宽高一半的负值.

2.不知道浮动元素的宽高

> 设定父元素为相对定位,浮动元素为绝对定位.且 left/right/top/bottom 设置为 0,再给浮动元素设置 margin:auto.


**6. 关于外边距重叠**

什么时候会发生外边距重叠 margin-collapse?

> 1. 标准流中相邻块元素垂直方向上的 margin 会折叠.

> 2. 浮动、inline-block、绝对定位的元素 margin 不会和垂直方向上其他元素的 margin 折叠.

> 3. 创建了块级格式化上下文 BFC 的元素,不会和它的子元素发生 margin 折叠. （position:relative 和 position:absolute）

防止外边距重叠解决方案:

> 外层元素 padding 代替

> 外层元素 overflow:hidden

> 内层元素加 float:left; 或 display:inline-block; 内层元素绝对定位 postion:absolute;

>

**7. 使用 css3 写一个幻灯片效果**

```css

<style> 

.ani{
  width:100px; height:100px;

  overflow:hidden;

  box-shadow:0 0 5px rgba(0,0,0,1);

  background-position:center;

  -webkit-animation:loops 10s infinite;
}

@-webkit-keyframes loops{
  0%{background:url(images/1.jpg)no-repeat;}

  33%{background:url(images/2.jpg)no-repeat;}

  66%{background:url(images/3.jpg)no-repeat;}

  /*说明:这里最后一帧如果和最初一帧不同,会出现跳跃*/
  100%{background:url(images/1.jpg)no-repeat;}
}

</style>

```

**8. css3 有哪些新特性？**

选择器、RGBA、阴影、盒模型、边框圆角、边框图片、渐变、背景、变换、过渡、动画、列布局、伸缩布局、媒体查询，calc()

**9. 移动端实现较小图片拥有较大响应区**

background-origin:padding-box/border-box/content-box; 设置定位原点

background-clip:padding-box/border-box/content-box; 设置显示区域

上面两个属性都设置 content-box，配合 padding 值，可以增大响应区域

**10. HTML5 新标签兼容性处理**

a. IE8/IE7/IE6 支持通过 document.createElement 方法产生的标签,但会把新标签解析成行内元素,还需要添加标签默认的样 式 display:block

b. 引入兼容性处理文件 html5shiv.min.js(bootstrap 框架中有使用)

> <!--[if lt IE9]>

> <script src="http://html5shim.googlecode.com/svn/trunk/html5.js"></script>

> <![end if]-->

>

**11. html5 常用的新标签**

​ header、nav、footer、aside、article、section

​

**12. css3 常用的新选择器**

> 属性选择器：E[attr]、E[attr=val]、E[attr*=val]、E[attr^=val]、E[attr$=val];

> 伪类选择器：E:only-child、E:first-child、E:last-child、E:nth-child(n)、E:nth-last-child(n)、E:nth-of-type(n)、E:empty、E:target、E:enabled、 E:disabled

> 伪元素选择器：E::before、E::after、E::selection

>

**13. html5 表单引入哪些新的标签、属性、输入类型、事件？**

​ 标签:<datalist></datalist> 配合 input 的 list 属性指定可能的值、<keygen />验证用户数据、<output></output> 展示内容不可修改 ;

​ 属性:placeholder 占位符、 autofocus 获取焦点、multiple 文件上传多选、 required 必填项、 pattern 正则表达式、autocomplete 自动完成、form 指定表单项属于哪个表单、novalidate 关闭验证 ;

​ 输入类型:email、tel、url、number、search、range、color、time、datatime ;

​ 事件:oninput 输入内容时触发,可用于移动端输入字数统计、onkeyup 按键弹起时触发、oninvalid 验证不通过时触发 ;

**32. h5 和 html4 中<a></a>标签的差异?**

​ 在 HTML 4.01 中，<a> 标签既可以是超链接，也可以是锚。在 HTML5 中，<a> 标签是超链接，但是假如没有 href 属性，它仅仅是超链接的一个占位符。

​ h5 中新属性：download, media, type

​ h5 去掉的属性：name, charset, coords, rev, shape

​

**33. 列举 h5 中的表单元素**

​ email(自动验证 email 格式), url(自动验证 url 格式), number(只能输入数字), range(滑动条), search(搜索域), datalist(自动验证内容是否在可选项中), color(颜色选择)

**34. 实现一个图片式上传图片的按钮**

**35. css3 中的单位**

​ px: 绝对单位，像素

​ em: 相对单位，以父节点的 font-size 大小为基准，如果自身定义了 font-size，则按自身的来计算

​ rem: 相对单位，以根节点 html 的 font-size 大小为基准

​ (另外需注意 chrome 强制最小字体为 12 号，即使设置成 10px 最终都会显示成 12px，当把 html 的 font-size 设置成 10px,子节点 rem 的计算还是以 12px 为基准，所以网上很多文章提到的将 html 的 font-size 设为 10 方便计算不是那么可取)

​ vh: 视窗高度， 1vh = 视窗高度的 1%

​ vw: 视窗宽度，1vw = 视窗宽度的 1%

​ vmin: vh 和 vw 中较小的那个

​ vmax: vh 和 vw 中较大的那个

​**56. css 中 position，display，overflow，float，margin 合并等几种特性叠加后的效果是怎么样的？**

​ 1. 如果元素拥有 position:absolute 或 position:fixed，那么 float 不起作用。

​ 2. 设置了 float，position:absolute，display:inline-block 属性的元素，margin 不会和垂直方向上相邻元素的 margin 合并。

​ 3. 创建了块级格式化上下文（BFC）的元素，如设置 overflow:hidden，不和它的子元素发生 margin 合并。

**57. 对于 BFC 的理解**

​ BFC（Block Formatting Context），块级格式化上下文，通俗将就是一个封闭的大箱子，箱子内部元素无论怎样都不会影响外部。BFC 规范决定了如何对其内容进行定位，以及它与其他元素的关系和相互作用。

​ BFC 规范：

​ 1. 元素的子元素会一个接一个的放置，垂直方向上它们的起点是一个 containing block 的顶部，相邻的块级元素垂直边距会折叠。

​ 2. 元素的子元素中，每一个子元素的左外边与 containing block 的左边相接触。

​ 3. BFC 区域不会与 float box 重叠。

​ 4. BFC 就是页面上一个隔离的独立容器，容器里面的子元素不会影响到外面的元素。

​ 5. 计算 BFC 高度时，浮动元素也参与计算。

​ 如何创建 BFC：

​ 1. 根元素 html 自动会形成一个 BFC

​ 2. float 属性不为 none 时

​ 3. overflow 属性不为 visible 时（hidden，scroll，auto）

​ 4. position 属性为 absolute 或 fixed

​ 5. display 属性为 inline-block，table-cell，table-caption

​ BFC 的作用：

​ 1. 清除内部浮动

​ 2. 解决垂直 margin 合并的问题

​ 3. 创建自适应两栏布局

​

**58. meta 标签了解多少？**

> meta 标签的作用是告诉浏览器该如何解析这个页面，还可以添加服务器发送到浏览器的 http 的头内容，如：

> <meta http-equiv="charset" content="iso-8859-1">

> 浏览器的头部就会包括：

> ​ charset: iso-8859-1

> http-equiv 属性：用来添加头部内容发送到浏览器，如在页面中加入：

> <meta http-equiv="Refresh" content="5; url=http://www.baidu.com" />

> ​ 5 秒钟后会跳转到指定页面。

> name 属性：

>

> 常用 meta 标签总结：

> 1. charset 声明文档使用的字符编码，两种写法：

> <meta charset="utf-8">

> <meta http-equiv="Content-Type" content="text/html; charset=utf-8">

> 2. 百度禁止转码

> <meta http-equiv="Cache-Control" content="no-siteapp" />

> 3. viewport 移动端布局

> <meta name="viewport" content="width=device-width, initial-scale=1.0" >

> 4. IE 优先使用最新的 IE 版本

> <meta http-equiv="x-ua-compatible" content="ie=edge">

> 5. Google 禁止自动翻译

> <meta name="google" value="notranslate">

> 6. 360 浏览器使用的解析内核

> <meta name="renderer" content="webkit | ie-comp | ie-stand">

> 7. SEO 优化部分

> ​ <title>your title</title>

> <meta name="keywords" content="your keywords">

> <meta name="description" content="your description">

> <meta name="author" content="author,email address">

> <meta name="robots" content="index,follow">

**60. H5 新的事件**

ondrag onresize

**68. 纯 css 创建一个三角形的原理是什么？**

> ​ 宽高为 0，设置三边边框


**72. 列举清除浮动的方法**

> ​ 1. 浮动结尾处加空 div 标签，设置属性 clear: both

> ​ 2. 父级设置固定高度

> ​ 3. 父级设置属性 overflow: hidden（同时不能设置 height），浏览器会自动检查浮动区域的高度

> ​ 4. 父级元素设置伪元素：after 和 zoom 属性，如：

> > > /_清除浮动代码_/ .clearfloat:after{display:block;clear:both;content:"";visibility:hidden;height:0} .clearfloat{zoom:1}

*127. 为什么 html5 可以开发移动应用，使用 HTML5 开发移动应用或混合应用的好处**

\1. 离线缓存，即本地存储 localStorage

\2. 音频视频标签<audio></audio><video></video>

\3. 地理定位，分享位置：navigator.geolocation API

\4. canvas 绘图，提升移动平台绘图能力

好处：

\1. 即时浏览，持续交付

\2. 开源生态系统发达：大量开源库，开发应用更轻松快捷，快速迭代，成本下降

\3. 兼容不同的系统和平台，一次开发，多处运行

\4. 互联网中分享和搜索方便，只需要一个 URL

**128. html5 的地理定位 API**

![img](http://note.youdao.com/yws/public/resource/493dfe1ab5c8c391fb978cf6701337e0/xmlnote/2618AF9B5544485BA597414E9F76C1F2/2415)

**129. 不定宽高的盒子水平垂直居中的方法**

1.  不使用弹性布局：
  父盒子要设置绝对定位，因为相对定位相对的是所有父级中上一个具有position属性的盒子的位置，如果都没有那相对的就是根级元素

```css
.xxx {
  ​position: absolute;
  ​top: 50%;
  ​left: 50%;
  ​-webkit-transform: translate(-50%，-50%) ;
}
```

2.  使用弹性布局：

```css
.xxx {
  display: -webkit-flex;
  justify-content: center;
  ​align-items: center;
}
```
