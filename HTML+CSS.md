# HTML
```

------------------------------------------文本-----------------------------------
标题 <h1>~<h6>	段落<p>				粗体<b>
	
斜体<i>			上标<sup>			下标<sub>

换行<br />		水平线<hr />		删除线<del>

下划线<ins>		居中标签<center>	预定义格式标签<pre>

字体标签 <font size="(大小1~7)" face="字体" color="颜色">txt</font>
--------------------------------------------------------------------------------


------------------------------------------列表-----------------------------------
有序列表:<ol> <li>		ol>li

无序列表:<ul> <li>		ul>li

自定义列表:<dl> <dt> <dd>	dl>dt+dd
--------------------------------------------------------------------------------


------------------------------------------链接-----------------------------------
网页链接:<a  href="http://	">

email链接:<a  href="mailto:	">

新窗口打开:<a  href="http://	"  target="_blank">
--------------------------------------------------------------------------------


------------------------------------------图像-----------------------------------
<img  src="图片位置"  alt="图像说明"  title="鼠标悬停时显示内容"
	width="宽"  height="高"  />

旧代码:

水平对 0 0
齐:<img  src=""  align="left/right" />  /* 图片居左/居右 */

垂直对齐:<img  src=""  align="top/middle/bottom">
				/* 文本第一行居上/居中/居下* /

HTNL5:图形与图形说明
<figure>  <figcaption>
figure>img+br+figcaption
--------------------------------------------------------------------------------


------------------------------------------表格-----------------------------------
创建表格<table>	创建行<tr>	创建居中标题<caption>	创建行内容<th> <td>
table>caption+tr>th+td

跨表格
横跨:<td  colspan="表格个数">
竖跨:<td  rowspan="表格个数">

长表格
标题<thead>	内容<tbody>	页脚<tfoot>
table>thead>th^tbody>th>td^^tfoot>tr>tdk

常用属性: bgcolor: 背景颜色;  background: 背景图片; border: 边框厚度;

--------------------------------------------------------------------------------


------------------------------------------表单-----------------------------------
<form  action="http://	"   method="get/post"    enctype="multipart/form-data" >

<input  type="表单类型"  name="发送地址"  />
type类型:
	text	单行文本框
	password	密码框
		maxlength="字符最大数量"	size="长度"
		required="required"	  /*表单验证*/
	
	radio	单选发送位置
		name="发送位置"
	checkbox	复选
		checked="checked"   /*默认选中的值*/
		value="发送的值"

	file	文件上传
	submit	提交
		value="按钮上显示的文本"

	date	日期
	image	图像按钮		src=""	width=""		height=""
	hidden	隐藏控件
	search	搜索控件		placeholder="未单击时文本框显示的内容"
				/*可应用于所有文本框*/

	email	邮箱
	url		url地址

多行文本框:<textarea  name=""  cols="宽"  rows="高">未单击时内容</texttarea>
			css 	resize 是否可由用户调整元素的尺寸。(none)


列表框:<select  name=""  size="一次显示的数量" >
		<optgroup label="">  分组
		<option  value="上传的值">
	select>optgroup>option

组合框:<fieldset><legend>
	fieldset>legend

自定义按钮:<button>

使文字和radio,checkbox关联
<input type="radio/checkbox" id="name">
<label for="name">txt<label>

去除选中文本框蓝色边框
input:focus{
	outline: none;
}

--------------------------------------------------------------------------------


------------------------------------------其他-----------------------------------
内联框架:<iframe  width=" "  height=" "  src=" ">
	旧代码:是否显示滚动条	scrolling="yes/no"
	旧代码:是否显示边框	frameborder="0/1"
	HTML5:不显示滚动条	seamless="seamless"

页面信息:<meta  name="		"  content="	">

		description					描述信息

		keywords					关键词,关键字

		robots						noindex/nofollow
									不能搜索到/能搜索到

	  <meta  http-equiv="	"  content="	">
			 author				网站设计者
			 pragma				no-cache /*防止浏览器缓存*/
			 expires			fir,月 apr 年 时间  gmt
					/*指定页面过期时间*/
	  <meta  charset="UTF-8">		/*编码方式*/
--------------------------------------------------------------------------------


------------------------------------视频/音频-----------------------------------

视频:
	<video  src=" "  poster="视频未播放时显示的图片"  width=" "  height=" ">
	controls	选择默认的播放控件	autoplay	自动播放
	loop	结束后重新播放	
	preload="none	/	auto/	metadata"
		未播放前不加载/未播放前加载/未播放前收集少量信息

多个视频:
	<source  src=" "  type="视频格式"  />
	video>source+source

音频:
	<audio  src=" "  >
	controls	是否显示播放控件
	autoplay	自动播放
	loop	结束后重新播放
	pleload="none	/	auto/	metadata"	
		未播放前不加载/未播放前加载/未播放前收集少量信息

多个音频:
	<source  src=" ">
	audio>source+source
--------------------------------------------------------------------------------





--------------------------特殊字符转义---------------------------
		<			&lt;
		>			&gt;
		©版权		&copy;
		®商标		&reg;
		空格 		&nbsp;
------------------------------------------------------------------

```


# CSS

```
-----------------------------------引用css-------------------------------------
外部css:<link  href="css文件路径"  type="text/css"  rel="stylesheet">

内部css:<style type="text/css">css编码</style>
--------------------------------------------------------------------------------


-------------------------------------颜色---------------------------------------
前景色:color:	;

背景色:background-color:	;

透明度:
	background-color:rgba(红,绿,蓝,透明度（0~1）);
	opacity:（0~1）;
--------------------------------------------------------------------------------


-------------------------------------文本---------------------------------------
字体选用:	font-family:"字体名称";

字体大小:	font-size:px/em/%;

粗体:		font-weight:bold;

斜体:		font-style:italic;

大小写		text-transform:	;
			uppercase		大写
			lowercase		小写
			capitalize		首字母大写
				
装饰线		text-decoration:	;
			none			删除文本的装饰线
			underline		文本底部增加装饰线
			overline		文本上部增加装饰线
			line-through		一条实线穿过文字
			blink			文本动态闪烁

行间距		line-height: em;

换行		word-break: ;
			normal		使用浏览器默认的换行规则
			break-all	允许在单词内换行
			keep-all	只能在半角空格或连字符处换行

文字间距		letter-spacing: em;

空格长度		word-spacing: em;

对齐方式		text-align:	;
				left		左对齐
				rigth		右对齐
				center		居中对齐
				justify		两端对齐
垂直对齐		vertical-align: em;

文本缩进		text-indent: px/em;

css3:投影		text-shadow:左右延伸距离,上下延伸距离,模糊程度,投影颜色;


		[伪元素]
首字母		:first-letter

首行文本	:first-line

链接样式	:link		/*未访问的链接样式*/
		:visited	/*访问过的链接样式*/

响应用户	:hover		/*光标悬停时的样式*/
		:active		/*单击是的样式*/
		:focus		/*获取焦点时的样式 点击后的样式 或 tab键获取焦点时的样式*/
当使用多个伪类时应按此顺序:	:link , :visit , :hover , :focus , :active
--------------------------------------------------------------------------------


-------------------------------------盒子---------------------------------------
盒子的大小	宽	width:px | em | %;
		高	height:px | em | %;
  
合并内边距和边框的宽度到盒子的总宽度    box-sizing: border-box;  

宽度限制		最大宽度	max-width:px | em | %;
				最小宽度	min-width:px | em | %;

高度限制		最大高度	max-height:px | em | %;
				最小高度	min-height:px | em | %;

内容溢出		overflow:hidden(将溢出内容隐藏) | scroll(为盒子添加滚动条)
						auto(自动选择);
					
边框宽度		border-width:px;

边框样式		border-style:	;
{
	实线solid			方形点dotted	
	虚线dashed		两条实现double
	雕入页面效果groove	凸起效果ridge	
	嵌入效果inset		凸出效果outset	
	不显示任何边框hidden/none
}

边框颜色		border-color:# ;

边框快捷方式	border:宽度 样式 颜色;
[
	四个值:上(top)  右(rigth)  下(bottom)  左(left);
	三个值:上 右左 下;
	两个值:上下 右左;
	一个值:全部;
]

内边距		padding:px | em | %;
外边距		margin:px | em | %;  (行标签不能使用top和bottom;top和bottom会重叠)

内连元素与块级元素的转换		display:	;
{
	inline		使块级元素像内连元素
	block		使内连元素像块级元素
	inline-block	使一个块级元素像内连元素,其他元素保持不变
	none		隐藏一个块级元素
}
盒子的剧中	margin:0px auto;

盒子的隐藏	visibility:hidden(隐藏)/visible(显示);隐藏/显示

css3边框图像		border-image:url("图像位置" )  像素  stretch(伸展)/repeat(重复)/round(自动);
				
css3盒子的投影	box-shadow:水平偏移 垂直偏移 模糊距离 阴影扩展  颜色;

css3圆角		border-radius: px ;

合并边框内边距       box-sizing: border-box;
--------------------------------------------------------------------------------


----------------------------------------列表.表格.表单--------------------------
列表项目符号样式:list-style-type:	;
[
	无序列表:	none			空白
				disc			•
				circle			
				square			■
	有序列表:	decimal 		1 2 3
				decimal-leading-zero	01 02 03
				lower-alpha		a b c
				upper-alpha		A B C
				lower-roman		i. ii. iii.
				upper-roman		I II III
]
列表项目图像:list-style-image:url("图片位置");
列表项目符号的位置:list-style-position:inside;/* 位于文本块的内部 */
列表快捷方式:list-style:标记的样式  标记的图像  标记的位置;

空单元格的边框:empty-cells:	;
[
	show	显示空单元格的边框
	hide	隐藏空单元格的边框
	inherit	用于嵌套,遵循外部的规则
]
单元格之间的空隙:	border-spacing:横向距离  纵向距离;	
					border-collapse:collapse/separate;	(用在table标签上)
							collapse 	将相邻的单元格得边框尽可能得合并为一个单独得边框
							separate	将相邻的边框分离 
光标样式:cursor
[
	auto	crosshair		default		pointer
	move	text		wait		help
	url("图片位置")
]
--------------------------------------------------------------------------------


-----------------------------------------布局------------------------------------
普通流:	position:static;
相对定位:	position:relative;
绝对定位:	position:absolute;
固定定位:	position:fixed;
[
	使用方向位移:top(上) bottom(下) left(左) right(右)
]

绝对定位 是 根据 有定位的 父级 来定位的,
如果父级没有定位,那就一层一层的往上找


重叠元素:	z-index:数字;	/* 数字越大越靠前 */
浮动元素:	float:right(右) | left(左);
清除浮动:	clear:left(左) | right(右) | both(两侧不能接触同一个元素)
				none(两侧能接触同一个元素) | inherit(从父元素继承的值) ;
	
--------------------------------------------------------------------------------


-----------------------------------------图像-------------------------------------
背景图像:	background-imag:url("图像位置");
重复图像:	background-repeat:	;
				[
					repeat			水平方向和垂直方向都进行重复
					repeat-x		水平方向进行重复
					repeat-y		垂直方向进行重复
					no-repeat		图像只显示一次
				]
			background-attachment:	;
				[
					fixed		固定在页面的一个位置
					scroll		随用户上下滚动
				]
图像定位:	background-position
				[
					left top		左上
					left center		左中
					left bottom		左下
					center top		中上
					center center	中间
					center bottom	中下
					right top		右上
					right center	右中
					right bottom	右下
				]
background简写:
		color--image--repeat--attachment--position
		css3渐变:background:linear-gradient( # , # );
--------------------------------------------------------------------------------


--------------------------------css3 2D 3D转换----------------------------------
transform:
		┏					2D					┓
			translate(Xpx,Ypx)				移动
			rotate(deg)						旋转
			cale(放大倍数)					放大
			skew(deg,deg)					翻转
			matrix(移动,旋转,放大,翻转)		组合
		┗ 										┛

		┏					3D					┓
			rotateX(deg)			绕轴X旋转
			rotateY(deg)			绕Y轴旋转
		┗										┛


--------------------------------css3 过渡--------------------------------------
transition: 属性 时间(s);
transition-timing-function:		; 		规定过渡效果的时间曲线
							linear			匀速
							ease			先慢后快(默认值)
							ease-in			慢速开始
							ease-in			慢速结束
							ease-in-out		慢 快 慢

transition-delay: 时间(s);				规定过渡效果何时开始
-------------------------------------------------------------------------------


--------------------------------css3 动画--------------------------------------
animation: 动画名称 时间(s);			捆绑

animation-timing-function:		;		规定动画的时间曲线
							linear			匀速
							ease			先慢后快(默认值)
							ease-in			慢速开始
							ease-in			慢速结束
							ease-in-out		慢 快 慢

animation-delay	:		;				规定动画何时开始

animation-iteration-count:		;		规定动画被播放的次数
							N 			N次
							infinite	无限次循环

animation-direction: alternate;			动画反向播放(只有动画被播放的次数在偶数的情况下最后一次才会发生.)

animation-play-state:		;			规定动画是否正在运行或暂停
							paused		暂停
							running		播放

animation-fill-mode:		;			规定动画在播放之前或之后,其动画效果是否可见
							none		不改变默认行为
							forwards	当动画完成后,保持最后一个属性值
							backwards	在 animation-delay 所指定的一段时间内,在动画显示之前,应用开始属性值
							both		向前和向后填充模式都被应用.



@keyframes	动画名称{
	from{}			/* 开始 */
	to{}			/* 结束 */
}

@keyframes	动画名称{			/* 使用百分数 */
	0%{}
	25%{}
	50%{}
	75%{}
	100%{}
}
-------------------------------------------------------------------------------


--------------------------------浏览器适配--------------------------------------
-ms-			IE 9
-webket-		Safari		Chrome
-o-				Opera
-moz-			Firefox
-------------------------------------------------------------------------------


```