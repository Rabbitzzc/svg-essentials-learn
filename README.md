### 入门指南

#### 图形系统
计算机中描述图形信息的两大图形系统：栅格图形和矢量图形：
1. 栅格图形。栅格图形就是将图像分成像素的长方形数组，每个像素用颜色值表示
2. 矢量图形。矢量图形是将图像描述为一系列几何形状，矢量图形阅读器接收指定坐标集上的绘制命令，绘制图像，而不是计算好的像素。
3. 
两者的区别我理解的是给一张都是格子的图纸，第一种是根据图形每个格子应该填充什么颜色，而矢量图形则是根据绘制命令，绘制点到点之间的直连或者曲线。因此看起来矢量图形就是对象而不是像素，矢量对象可以改变某个对象的形状和颜色，而像素则不可以，这也是canvas修改只能重绘，而svg则可以渲染图形中的部分区域。

栅格模式是使用一系列像素构成图片，而图片基本上很少有明显的线条和曲线，扫描图片也是扫描像素点，在使用照片也不需要关心很小的细节，只需要关注整个图像就可以。而矢量图形的利用就是一些精细的图，比如测绘等，比如很高的分辨率的一些图片，还有动画之类的都需要矢量图形的存在。

#### SVG
SVG是一种XML应用，意为可缩放矢量图形（Scalable Vector Graphics）。所有的图形有关信息被存储为纯文本，具有XML的开放性、可移植性和可交互性。当前稳定的XML和SVG版本都为1.1。

SVG文档结构是标准的XML文档，根元素svg定义图形的大小，根元素中包含各种的形状元素。SVG允许使用单独的属性指定元素的样式。

按照菜鸟教程给出的定义发散：
- SVG 指可伸缩矢量图形 (Scalable Vector Graphics)
- SVG 用来定义用于网络的基于矢量的图形
- SVG 使用 XML 格式定义图形
- SVG 图像在放大或改变尺寸的情况下其图形质量不会有所损失
- SVG 是万维网联盟的标准
- SVG 与诸如 DOM 和 XSL 之类的 W3C 标准是一个整体

SVG 存在的通用图形有矩形、圆形、椭圆形、直线、多边形、曲线、文本。

### 网页中使用 SVG
1. `<img>`包含SVG
将SVG图片包含在img里面，意味着将svg转化为了栅格模式，svg图片成了像素组成。此时svg不再是元素标签，而是资源，与主页面的渲染独立。同时在css中设置的svg样式意味着也会无效。

2. CSS 中包含SVG
在CSS中包含SVG，最常用的是background-image属性，应该避免SVG元素文件太大。同时在CSS中SVG还能被用作list-image或者border-image。

3. 将SVG作为应用程序
使用`<object>`嵌入SVG，可以让那些不能直接显示SVG但是有SVG插件的老版浏览器也能查看图像。

### 坐标系统
SVG的世界就是一张无限大的画布，坐标系统可以确定画布中每个区域应该显示什么内容。

1. 视口
文档打算使用的画布区域称为视口。使用`width`和`height`确定视口带下。
```html
<svg width="200" height="150"></svg>
```
2. 默认用户坐标
阅读器设置一个坐标系统，其中水平坐标向右递增，垂直坐标向下递增，即原点为（0，0）。这个坐标系系统是一个纯粹的集合系统，点没有宽度和高度，其网格线也被人为是无限细的，这也是矢量与栅格的区别。

3. 为视口指定用户坐标
阅读器的默认情况下，用户坐标系系统与视图坐标系系统重合。使用viewbox属性可以抛弃默认的坐标系系统，创建新的当前用户坐标系统。

比如现在使用svg创建一个圆
```html
<circle cx="200" cy="200" r="200" fill="#fdd" stroke="none"></circle>
```
放在一个`400*400`的画布，肯定是正好撑满的，现在如果需要放在一个`150*150`的svg里面。说明以默认坐标系标准是不合适，这时候使用viewbox
```html
<svg style="width:150px; height:300px" viewBox="0 0 400 400">
    <circle cx="200" cy="200" r="200" fill="#fdd" stroke="none">
    </circle>
</svg>
```
则正好可以覆盖，
`viewBox="<min-x>, <min-y>, <width>, <height>"`
- <min-x>与<min-y>：规定viewBox的左上角的坐标
- <width>与<height>：规定viewBox的尺寸

4. 嵌套坐标系统
我们可以在任何时候将另一个svg元素插入到文档中来建立新的视口和坐标系统，其效果就是创建一个稍后可以绘制图形的“迷你画布”。也就是说svg中可以嵌套另一个svg，每个svg都有自己独立的视口和坐标系统。

### 基本形状
一旦使用svg标签创建一个坐标系统，就可以开始画图。svg的主要和基本形状是：线段、矩形、多边形、圆、椭圆。

#### 矩形
width height 表示宽高

x y 表示 svg 容器之间的距离

#### 圆形
cx和cy属性定义圆点的x和y坐标。如果省略了设置，默认为设置为0,0坐标中心为圆心
r 表示半径

#### 椭圆形
cx cy 表示椭圆的圆心
rx ry  分别表示水平半径和垂直半径

#### 直线
x1 属性在 x 轴定义线条的开始
y1 属性在 y 轴定义线条的开始
x2 属性在 x 轴定义线条的结束
y2 属性在 y 轴定义线条的结束

#### 多边形
points 属性定义多边形每个角的 x 和 y 坐标
比如五角星
```html
<polygon points="100,10 40,180 190,60 10,60 160,180"
            style="fill:lime;stroke:purple;stroke-width:5;fill-rule:nonzero;" />
```
注意每个空格之间的点的坐标是有顺序的，线是根据相邻的点相互连接而成
fill-rule  nonzero | evenodd | inherit

属性用于指定使用哪一种算法去判断画布上的某区域是否属于该图形“内部” （内部区域将被填充）。对一个简单的无交叉的路径，哪块区域是“内部” 是很直观清除的。但是，对一个复杂的路径，比如自相交或者一个子路径包围另一个子路径，“内部”的理解就不那么明确了。

#### 路径
<path> 元素用于定义一个路径
M = moveto
L = lineto
H = horizontal lineto
V = vertical lineto
C = curveto
S = smooth curveto
Q = quadratic Bézier curve
T = smooth quadratic Bézier curveto
A = elliptical Arc
Z = closepath

#### 文本
x y 分别表示起点
fill="red" transform="rotate(30 20,40)"
fill 表示填充的颜色
transform 表示css的transform

#### 笔画和填充特性

| 属性              | 值                                        |
| ----------------- | ----------------------------------------- |
| Stroke            | 笔画颜色                                  |
| Stroke-width      | 笔画宽度                                  |
| Straoke-opacity   | 笔画的透明度                              |
| Stroke-dasharray  | 数字指定虚线空隙长度                      |
| Stroke-linecap    | 线头尾的形状                              |
| Stroke-linejoin   | 图形棱角                                  |
| stroke-miterlimit | 相交处显示宽度和线宽的最大比例，默认值为4 |

| 属性         | 值                                               |
| ------------ | ------------------------------------------------ |
| fill         | 填充颜色，默认为黑色                             |
| fill-opacity | 颜色的透明度                                     |
| fill-rule    | 属性为nonzero或者evenodd。判断某个点是否在图形内 |

### 文档结构
XML 的目标之一是提供以各种能将结构从视觉表示中独立出来的方法。

SVG 允许我们使用四种方式指定图形表现方面的信息：
- 内联样式
- 内部样式表
- 外部样式表
- 表现属性

#### 内联样式
```html
<circle style="stroke:#000"></circle>
```
#### 内部样式表
可以通过一个内部样式表来罗列常见的样式，使用`style`来定义，定义在defs内。
```html
<svg>
    <defs>
        circle {
            xxx
        }
    </defs>
</svg>
```

#### 外部样式表
与 html 类似，定义在css文件内部。

#### 表现属性
SVG允许以属性的形式指定表现样式，但是表现属性的优先级最低，如果以其他三种形式指定了相同的样式属性，则将覆盖通过表现属性指定的样式。

#### 分组和引用对象

使用`<g>`会员会将其所有的子元素作为一个组合，通常组合还会有一个唯一的id作为名称，，诶个组合还可以拥有自己的`title`和`desc`来共基于文本的XML应用程序识别，为用户提供更好的可访问性。

使用`<use>`元素为定义在`g`元素内的组合或者任意独立图形元素提供类似复制粘贴的能力。

使用`<defs>`元素用来定义需要复用的元素，定义在`defs`内的元素并不会被显示，而是作为模板供到其他地方使用。类似于定义组件。

`<symbol>`元素提供了另一种组合元素的方式，和g元素不同，symbol元素永远不会显示，因此我们无需把它放在`defs`规范内。然而习惯于将它放到`defs`中，因为symbol也是我们定义的供后续元素使用的元素，symbol还可以指定biewBox和preserveAspectRatio属性，通过给use元素添加with和height属性就可以让symbol适配视口大小。

### 系统坐标变换
1. 使用translate移动坐标系统
```html
<use xlink:href="#square" tranform="translate(50,50)"></use>
```
这不是css中移动某个图形到网格的另一个地方，而是translate会获取整个网格，然后将画布移动到新位置，而不是移动某个元素

2. scale变换
通过缩放坐标系统的方式让对象显示得比定义的尺寸更大或者更小。

仅仅使用scale(n)变换时，网格系统的原点位置并没有变化，只是每个用户坐标都变成了原来的n倍，也就是网格变大了，因此线也会变粗(用户单位并没有变)。

缩放变换永远不会改变图形对象的网格坐标或者笔画宽度，仅仅改变对应画布上的坐标系统网格的大小。

3. rotate变换
根据指定的角度旋转坐标系统，默认的坐标系统中，角度的测量顺时针增加，0度为3点钟方向。

### 路径

#### path 命令
所有的基本形状都是`<path>元素的简写样式。

所有描述轮廓的数据都放在`path`元素的d属性（data）。路径数据包括单个字符的命令，比如M表示moveto， L表示lineto，接着是该命令的坐标信息

| 命令 | 参数                                      | 说明                                                                                                                                                     |
| ---- | ----------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------- |
| M m  | x y                                       | 移动画笔到制定坐标                                                                                                                                       |
| L l  | x y                                       | 绘制一条到给定坐标的线                                                                                                                                   |
| H h  | x                                         | 绘制一条到给定x坐标的横线                                                                                                                                |
| V v  | y                                         | 绘制一条到给定y坐标的垂线                                                                                                                                |
| A a  | rx ry x-axis-rotation large-arc sweep x y | 圆弧曲线命令有7个参数，依次表示x方向半径、y方向半径、旋转角度、大圆标识、顺逆时针标识、目标点x、目标点y。大圆标识和顺逆时针以0和1表示。0表示小圆、逆时针 |
| Q q  | x1 y1 x y                                 | 绘制一条从当前点到x,y控制点为x1,y1的二次贝塞尔曲线                                                                                                       |
| T t  | x y                                       | 绘制一条从当前点到x,y的光滑二次贝塞尔曲线，控制点为前一个Q命令的控制点的中心对称点，如果没有前一条则已当前点为控制点。                                   |
| C c  | x1 y1 x2 y2 x y                           | 绘制一条从当前点到x,y控制点为x1,y1 x2,y2的三次贝塞尔曲线                                                                                                 |
| S s  | x2 y2 x y                                 | 绘制一条从当前点到x,y的光滑三次贝塞尔曲线。第一个控制点为前一个C命令的第二个控制点的中心对称点，如果没有前一条曲线，则第一个控制点为当前的点。           |




### 图案和渐变
填充图形或笔画除了使用fill,stroke纯色之外，还可以使用图案和渐变填充。

#### 图案
要使用图案，首先要定义一个书评或者垂直方向重复的图形对象，然后用他条虫另一个对象或者笔画使用。这个图形对象被称作瓷砖（tile）。

要创建一个图案，必须使用pattern元素包裹描述图案的path元素。
```html
<defs>
	<pattern id="tile" x="0" y="0" width="20%" height="20%" patternUnits="objectBoundingBox">
    	    <path d="M 0 0 Q 5 20 10 10 T 20 20" style="stroke:black;fill:none"></path>
	    <path d="M 0 0 h 20 v 20 h -20 z" style="stroke:black;fill:none"></path>
	</pattern>
</defs>
<path d="M 0 0 Q 5 20 10 10 T 20 20" style="stroke:black;fill:none"></path>
<path d="M 0 0 h 20 v 20 h -20 z" style="stroke:black;fill:none"></path>
<rect x="40" y="0" width="100" height="100" style="fill:url(#tile);stroke:black"></rect>	
<rect x="155" y="0" width="70" height="80" style="fill:url(#tile);stroke:black"></rect>
<rect x="250" y="0" width="150" height="130" style="fill:url(#tile);stroke:black"></rect>
```

#### 渐变
我呢可以使用渐变填充对象，也就是从一个颜色平滑过渡到另一个，而不是使用纯色填充对象，渐变可以使线性的，即颜色沿着直线过度，也可以是径向的，即颜色从中心店向外发散过渡。

线性渐变中，方向的起点（拥有0%渐变点）由属性x1和y1定义，重点（拥有00%渐变点）由属性x2和y2定义

对于径向渐变，焦点（拥有0%渐变点）由属性fx和fy定义，拥有00%渐变点的圆由中心点坐标cx、cy和半径r定义

### 文本
text元素以指定的x和y值作为元素内容第一个字符的基线位置，默认样式黑色填充、没有轮廓。

text元素无法对文本进行换行操作，如果需要分行显示文本，则需要使用tspan元素。tspan元素与html的span元素类似，可以嵌套在文本内容中，并可以单独改变其内部文本内容的样式。

设置writing-mode属性值为tb(top to bottom)，可以将文本上下排列。

如果要使得文本沿着某条路径排列，则需要使用textPath元素。需要将文本放在textPath元素内部，然后使用textPath元素的xlink:href属性引用一个定义好的path元素。

### 裁剪和蒙版

#### 裁剪
在创建SVG文档时，可以通过指定感兴趣的区域的宽度和高度建立视口，这个视口就会变成默认的裁剪区域，裁剪区域外的任何部分都不会被显示。裁剪区域可以通过clipPath元素建立自己的裁剪区域。

使用clipPath可以创建一个裁剪区域。

#### 蒙版
SVG的蒙版会变换对象的透明度，如果蒙版是不透明的，则被蒙版覆盖的对象的像素就是不透明的，如果蒙版是半透明的，则对象就是半透明的。

使用mask元素创建蒙版

### 滤镜
。。

### SVG 动画

#### 动画基础
SVG的东阿虎特性基于万维网联盟的“同步多媒体集成语言”（SMIL）规范。在这个动画系统中，我们可以指定想要进行动画的属性（比如颜色、动画、或者变形属性）的起始点和结束值，以及动画的开始时间和持续时间。

| 属性          | 说明                                                                                                                                                |
| ------------- | --------------------------------------------------------------------------------------------------------------------------------------------------- |
| attributeName | 动画中改变的值                                                                                                                                      |
| attributeType | width是一个XML属性，另一个常用的值是CSS，如果忽略这个值，则默认是auto，会先搜索CSS属性，再搜索XML属性                                               |
| from to       | 起始值和结束值                                                                                                                                      |
| values        | from和to只能指定两个值，使用values时可以以列表形式同时指定多个中间值，变换会依次使用列表内的值。如果要交替动画，则指定为start,end,start三个即可实现 |
| dur           | 持续时间                                                                                                                                            |
| fill          | 结束时该怎么做，freeze表示冻结，如果不指定会使用默认值：remove，表示动画完成后会返回原始值。                                                        |
| repeatCount   | 动画重复次数                                                                                                                                        |
| repeatDur     | 重复应该进行多长时间                                                                                                                                |
| calcMode      | 过渡类型，可以为线性(linear)，直接跳转(不支持过渡时直接跳转到结束值discrete)，spline，paced等。                                                     |


#### 同步动画
我们可以绑定动画的开始时间为另一个动画的开始或者结束，而不是在文档加载时定义每个动画的开始时间。
```html

<circle cx="60" cy="60" r="30" style="fille:gray;stroke:blueviolet;">
    <animate id="s1" attributeType="XML" attributeName="r" from="30" to="10" begin="0s" dur="4s" repeatCount="" fill="freeze" />
</circle>

<circle cx="120" cy="60" r="10" style="fille:gray;stroke:blueviolet;">
    <animate attributeType="XML" attributeName="r" from="10" to="30" begin="s1.end" dur="4s" repeatCount="4" fill="freeze" />
</circle>
```

#### set 元素
所有的动画归根到底都是修改值。有时候，特别是对于非数字属性或者不能过度的属性，可以使用set元素来描述动画。
```html
<text x="60" y="60" style="visibility: hidden;">
    HHHH
    <set attributeName="visibility" attributeType="CSS" to="visible" begin="3s" dur="1s" fill="freeze"></set>
</text>
```

#### 使用CSS处理svg动画
现代浏览器都支持CSS梳理SVG动画，使用CSS处理SVG动画需要两个步骤：一是选中要运动的元素，然后设置将动画属性作为一个整体进行设置。二是告诉浏览器改变选中元素的哪个属性以及在动画的什么阶段。这些都定义在@keyframes中。

| 动画属性                  | 说明                                                         |
| ------------------------- | ------------------------------------------------------------ |
| animation-name            | @keyframes说明符的名称                                       |
| animation-duration        | 动画持续时长                                                 |
| animation-timing-function | 如何计算中间值，也就是动画类型：线性，缓入缓出等等           |
| animation-iteration-count | 重复次数                                                     |
| animation-direction       | 动画是反向还是正向或者交替执行                               |
| animation-play-state      | 可以设置为running或paused                                    |
| animation-delya           | 延迟时间                                                     |
| animation-fill-mode       | 动画不再执行时使用什么属性，可以为forwards(结束时属性)、backwards(开始时属性值)、both |

