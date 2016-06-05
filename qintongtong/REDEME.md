这里写笔记
CSS position 属性
实例
定位 h2 元素：
h2
  {
  position:absolute;
  left:100px;
  top:150px;
  }
所有主流浏览器都支持 position 属性。
注释：任何的版本的 Internet Explorer （包括 IE8）都不支持属性值 "inherit"。
定义和用法
position 属性规定元素的定位类型。
说明
这个属性定义建立元素布局所用的定位机制。任何元素都可以定位，不过绝对或固定元素会生成一个块级框，而不论该元素本身是什么类型。相对定位元素会相对于它在正常流中的默认位置偏移。
默认值：	static
继承性：	no
版本：	CSS2
JavaScript 语法：	object.style.position="absolute"
可能的值
值	描述
absolute	
生成绝对定位的元素，相对于 static 定位以外的第一个父元素进行定位。
元素的位置通过 "left", "top", "right" 以及 "bottom" 属性进行规定。
fixed	
生成绝对定位的元素，相对于浏览器窗口进行定位。
元素的位置通过 "left", "top", "right" 以及 "bottom" 属性进行规定。
relative	
生成相对定位的元素，相对于其正常位置进行定位。
因此，"left:20" 会向元素的 LEFT 位置添加 20 像素。
static	默认值。没有定位，元素出现在正常的流中（忽略 top, bottom, left, right 或者 z-index 声明）。
inherit	规定应该从父元素继承 position 属性的值。
TIY 实例
定位：相对定位
本例演示如何相对于一个元素的正常位置来对其定位。
定位：绝对定位
本例演示如何使用绝对值来对元素进行定位。
定位：固定定位
本例演示如何相对于浏览器窗口来对元素进行定位。
设置元素的形状
本例演示如何设置元素的形状。此元素被剪裁到这个形状内，并显示出来。
Z-index
Z-index可被用于将在一个元素放置于另一元素之后。
Z-index
上面的例子中的元素已经更改了Z-index。
相关页面
CSS 教程：CSS 定位
HTML DOM 参考手册：position 属性
http://www.w3school.com.cn/cssref/pr_class_position.asp
