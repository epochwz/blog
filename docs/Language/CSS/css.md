CSS Cascading Style Sheets 层叠样式表

CSS 优先级
1. !important 标识的属性优先级最高
2. 内联样式
3. 选择器权值大的优先
	通用选择器	*
	标签选择器	p		1
	类选择器		.		10
	属性选择器	a[href]
	伪类选择器	a:hover
	ID选择器		#		100
4. 权值相等的采用就近原则(内部样式表>外部样式表)
5. 继承(元素的继承属性没有指定值时，则该属性继承父元素的值)
6. 浏览器默认

CSS Demo
p{  /* 选择器 */
	key:value;/* 属性列表:键值对 */
}

/* 分组选择器 */
p,span{}

/* 第一代子元素选择器 */
p > span{}
/* 所有后代选择器 */
p span{}
/* 第一个兄弟选择器 */
p+span{}
/* 所有相邻兄弟选择器 */
p~span{}

字体排版
body{
	font-family:"Microsoft Yahei";
	font-size:12px;
	color:#666;
	font-weight:bold;
	font-style:italic;
	text-decoration:underline;
	text-decoration:line-through;
	text-indent:2em;
	line-height:1.5em;
	letter-spacing:50px;
	word-spacing:50px;
	text-align:center;
	font:italic bold 12px/1.6em "Microsoft Yahei",sans-serif;
}

块元素(独占一行)：<div>、<p>、<h1>...<h6>、<ol>、<ul>、<dl>、<table>、<address>、<blockquote> 、<form>
行内元素(宽度、高度、底/顶部边距 不可设置)：<a>、<span>、<br>、<i>、<em>、<strong>、<label>、<q>、<var>、<cite>、<code>
行内块元素(宽度、高度、底/顶部边距 可设置)：<img>、<input>
	
盒子模型：元素宽度=内容+(外边距+边框+内边距)*2

网页基本布局模型
1. 流动模型flow（默认网页布局）
	块元素在所处的包含元素内从上到下垂直分布显示
	内联元素在所处的包含元素内从左到右水平分布显示
2. 浮动模型float
	浮动框可以向左/右移动，直到它的外边缘碰到包含框或另一个浮动框的边框为止。
	浮动框脱离了文档流，所以文档流中的其他框表现的像浮动框不存在似的

	清除浮动：clear:left/right/both	规定元素的左/右/两侧不允许其他浮动元素出现
	.clear{
	    clear:both;
	    height:0;
	    overflow:hidden;
	}
	<p class="clear"></p>
3. 层模型layer
	绝对定位(position: absolute):脱离文档流，使用left/right/top/bottom相对于最接近的一个具有定位属性的父包含块进行绝对定位(上溯至body);
	相对定位(position: relative):生成新的元素，保留旧元素，新元素相对旧元素的位置移动;
	固定定位(position: fixed):类似绝对定位，但其坐标参考系是浏览器视图;
相对其他元素定位：参照元素是定位元素的父元素，参照元素relative,定位元素absolute

水平居中
	行内元素：父元素设置	text-align:center
	块状元素
		定宽块状元素(width为固定值):	margin:0 auto;
		不定宽块状元素
			1. table(宽度由文本宽度决定，视为定宽块状元素。):margin:0 auto;
			2. display:inline（将块状元素变成行内元素,将其父元素设置 text-align:center）
			3. 设置浮动和相对定位：给父元素设置float和position:relative和left:50%;子元素设置position:relative和left:-50%

垂直居中
	父元素高度确定的单行文本：设置父元素的height和line-height一致(line-height: 行高，与文本大小的计算值之差分为两半，加入文本行的顶部和底部)
	父元素高度确定的多行文本
		表格包裹，利用td标签默认vertical-align:middle 对inline-block类型元素起作用
		display:table-cell;vertical-align:middle;

无论什么类型元素，一旦添加position:absolute或float:left/right，则该元素成为 inline-block 元素，可以设置宽高