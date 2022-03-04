---
title: CSS书写规范、顺序
tag: 前端 css
date: 2021-10-25
---

# CSS书写规范、顺序

使用良好的CSS书写规范来写CSS代码，可以提升代码的阅读体验，这里设计达人网总结一个CSS书写规范、CSS书写顺序供大家参考：

转至:[CSS书写规范、顺序](https://www.cnblogs.com/xuepei/p/8961809.html)

## CSS书写顺序
1.位置属性(position, top, right, z-index, display, float等)
2.大小(width, height, padding, margin)
3.文字系列(font, line-height, letter-spacing, color- text-align等)
4.背景(background, border等)
5.其他(animation, transition等)

![](https://images2018.cnblogs.com/blog/612653/201804/612653-20180427115626958-2138003962.png)

按照上述1 2 3 4 5的顺序进行书写，目的：减少浏览器reflow(回流)，提升浏览器渲染dom的性能。

浏览器的渲染流程为：

（1）浏览器解析html构建dom树，解析css构建cssom树即css rule tree：将html和css都解析成树形的数据结构； dom树的构建过程是一个深度遍历过程：当前节点的所有子节点都构建好后才会去构建当前节点的下一个兄弟节点。

（2）构建render树：DOM树和cssom树合并之后形成render树。为了构建渲染树，浏览器大体完成了下列工作：从DOM树的根节点开始遍历每个可见节点。对于每个可见节点，为其找到适配的CSSOM规则并应用它们。发射可见节点，连同其内容和计算的样式。渲染树中包含了屏幕上所有可见内容及其样式信息。

（3）布局render树：有了render树，浏览器已经知道网页中有哪些节点，各个节点的css定义以及它们的从属关系，接着就开始布局，计算出每个节点在屏幕中的位置和大小。(html采用了一种流式布局的布局模型，从上到下，从左到右顺序布局，布局的起点是从render树的根节点开始的，对应dom树的document节点，其初始位置为(0,0)，详细的布局过程为：每个renderer的宽度由父节点的renderer确定。父节点遍历子节点，确定子节点的位置(x,y)，调用子节点的layout方法确定其高度，父节点根据子节点的height, margin, padding确定自身的高度)。

（4）渲染，绘制render树：浏览器已经知道啦哪些节点要显示，每个节点的css属性是什么，每个节点在屏幕中的位置是哪里。就进入啦最后一步，按照计算出来的规则，通过显卡把内容画在屏幕上。


![](https://img2018.cnblogs.com/blog/612653/201905/612653-20190528155836708-1330513625.png)


 浏览器并不是一获取到css样式就立马开始解析而是根据css样式的书写顺序按照dom树的结构分布render样式，完成第（2）步，然后开始遍历每个树节点的css样式进行解析，此时的css样式的遍历顺序完全是按照之前的的书写顺序，在解析过程中，一旦浏览器发现某个元素的定位变化影响布局，则需要倒回去重新渲染。

例如css样式：{width: 100px; height: 100px; background-color: red; position: absolute;}当浏览器解析到position的时候突然发现该元素是绝对定位元素需要脱离文档流，而之前却是按照普通元素进行解析的，所以不得不重新渲染，解除该元素在文档中所占位置，然而由于该元素的占位发生变化，其他元素也可能会受到回流的影响而重新排位，最终导致（3）步骤花费时间太久而影响到（4）步骤的显示，影响了用户体验。

注：render树的结构不等同于DOM树的结构，一些设置display:none的节点不会被放在render树中，但会在dom树中。

有些情况，比如修改了元素的样式，浏览器并不会立刻reflow或repaint，而是把这些操作积攒一批，然后做一次reflow，这也叫做异步reflow。但是在有些情况下，比如改变窗口大小，改变页面默认字体等浏览器会马上进行reflow。

为了更好的用户体验，渲染引擎将会尽可能早的将内容呈现在屏幕上，并不会等到所有html都解析完成之后再去构建和布局render树。它是解析完一部分内容就显示一部分内容，同时，可能还在网络上下载其余内容。

render tree:

```html
<html>
<head>
     <title>Greeting</title>
</head>
<body>
     Hello,<span>world! <img id="footer" /></span>
</body>
</html>
```

![](https://img2018.cnblogs.com/blog/612653/201905/612653-20190528161825790-1728684334.png)

## CSS书写规范
### 使用CSS缩写属性
CSS有些属性是可以缩写的，比如padding,margin,font等等，这样精简代码同时又能提高用户的阅读体验。

![](https://images2018.cnblogs.com/blog/612653/201804/612653-20180427115841020-1945612259.png)

### 去掉小数点前的“0”

![](https://images2018.cnblogs.com/blog/612653/201804/612653-20180427115917155-286646213.png)

### 简写命名
很多用户都喜欢简写类名，但前提是要让人看懂你的命名才能简写哦！

![](https://images2018.cnblogs.com/blog/612653/201804/612653-20180427120449504-5757773.png)


### 16进制颜色代码缩写
有些颜色代码是可以缩写的，我们就尽量缩写吧，提高用户体验为主。

![](https://images2018.cnblogs.com/blog/612653/201804/612653-20180427120425991-1280068865.png)


### 连字符CSS选择器命名规范

1.长名称或词组可以使用中横线来为选择器命名。

2.不建议使用“_”下划线来命名CSS选择器，为什么呢？

* 输入的时候少按一个shift键；
* 浏览器兼容问题 （比如使用_tips的选择器命名，在IE6是无效的）
* 能良好区分JavaScript变量命名（JS变量命名是用“_”）

这里有一篇破折号与下划线的详细讨论，英文：[点击查看](http://stackoverflow.com/questions/7560813/why-are-dashes-preferred-for-css-selectors-html-attributes) 中文篇：[点击查看](http://www.cnblogs.com/kaiye/archive/2011/06/13/3039046.html)

![](https://images2018.cnblogs.com/blog/612653/201804/612653-20180427120404743-505228924.png)


### 不要随意使用id
id在JS是唯一的，不能多次使用，而使用class类选择器却可以重复使用，另外id的优先级优先与class，所以id应该按需使用，而不能滥用。

![](https://images2018.cnblogs.com/blog/612653/201804/612653-20180427120330454-657368948.png)

### 为选择器添加状态前缀
有时候可以给选择器添加一个表示状态的前缀，让语义更明了，比如下图是添加了“.is-”前缀。

![](https://images2018.cnblogs.com/blog/612653/201804/612653-20180427120306380-1290279509.png)

## CSS命名规范（规则）

### 常用的CSS命名规则

 > 头：header
 > 内容：content/container
 > 尾：footer
 > 导航：nav
 > 侧栏：sidebar
 > 栏目：column
 > 页面外围控制整体佈局宽度：wrapper
 > 左右中：left right center
 > 登录条：loginbar
 > 标志：logo
 > 广告：banner
 > 页面主体：main
 > 热点：hot
 > 新闻：news
 > 下载：download
 > 子导航：subnav
 > 菜单：menu
 > 子菜单：submenu
 > 搜索：search
 > 友情链接：friendlink
 > 页脚：footer
 > 版权：copyright
 > 滚动：scroll
 > 内容：content
 > 标签：tags
 > 文章列表：list
 > 提示信息：msg
 > 小技巧：tips
 > 栏目标题：title
 > 加入：joinus
 > 指南：guide
 > 服务：service
 > 注册：regsiter
 > 状态：status
 > 投票：vote
 > 合作伙伴：partner

### 注释的写法:

> /* Header \*/
> 内容区
> /* End Header \*/

### id的命名:
#### (1)页面结构

> 容器: container
> 页头：header
> 内容：content/container
> 页面主体：main
> 页尾：footer
> 导航：nav
> 侧栏：sidebar
> 栏目：column
> 页面外围控制整体佈局宽度：wrapper
> 左右中：left right center

#### (2)导航

> 导航：nav
> 主导航：mainnav
> 子导航：subnav
> 顶导航：topnav
> 边导航：sidebar
> 左导航：leftsidebar
> 右导航：rightsidebar
> 菜单：menu
> 子菜单：submenu
> 标题: title
> 摘要: summary

#### (3)功能

> 标志：logo
> 广告：banner
> 登陆：login
> 登录条：loginbar
> 注册：register
> 搜索：search
> 功能区：shop
> 标题：title
> 加入：joinus
> 状态：status
> 按钮：btn
> 滚动：scroll
> 标籤页：tab
> 文章列表：list
> 提示信息：msg
> 当前的: current
> 小技巧：tips
> 图标: icon
> 注释：note
> 指南：guild
> 服务：service
> 热点：hot
> 新闻：news
> 下载：download
> 投票：vote
> 合作伙伴：partner
> 友情链接：link
> 版权：copyright

### 注意事项::
1.一律小写;
2.尽量用英文;
3.不加中槓和下划线;
4.尽量不缩写，除非一看就明白的单词。

## CSS样式表文件命名
>主要的 master.css
>模块 module.css
>基本共用 base.css
>布局、版面 layout.css
>主题 themes.css
>专栏 columns.css
>文字 font.css
>表单 forms.css
>补丁 mend.css
>打印 print.css