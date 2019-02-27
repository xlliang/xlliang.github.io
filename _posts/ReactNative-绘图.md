#ReactNative

###ART-绘图
####基础元素

* Surface - 长方形的容器，可渲染的区域，是其他元素的容器！

* Group - 可容纳形状、文本和其他的分组

* Shape - 一个向量路径定义的形状，可填充

* Text - 文本

说明：

即一个图形，必须是这样的：最外层是一个 Surface，去定义容器的大小，就像是 Canvas 元素一样，然后其子元素由 Group 进行组装，使用 Shape 和 Text 确定内容

####公共的 props

* x,y: 坐标

* originX, originY: 原始坐标，这个给 scale 和 rotate 使用的

* transform: {Array} 变形相关

* visible: {boolean} 决定了 opacity 的值是 0|1

* opacity: {number} opacity 的值

* scale, scaleX, scaleY {number} 缩放

* rotation: {string} 旋转 0deg

说明：

x, y 每个都可有，它指定了元素的位置。
originX, originY则是一个相对位置，它主要是给 scale 和 rotate 使用的，默认是0, 0，这样就意味着，默认是以起点为中心旋转，或者从起点开始缩放！

####Surface
容器，接受的 props：

* width: {number}

* height: {number}

####Group
定义分组，它是一个 Component，实际上使用了 NativeGroup，可接受的 props：

* onMouseDown

* onMouseUp

####Shape
形状，与 ART 或者 ReactART 不同的是，当前不支持 width 和 height，可接受的 props：

* fill:填充色

* stroke: 画笔颜色

* strokeDash: {Object} 画虚线

* strokeDash.count {number} array 的个数，必须有此值才能生效，必须=array.length
  
* strokeDash.array：{Array} 虚线是如何交替绘制
	1. 10,10 表示先绘制10个点，再跳过10个点，如此反复

   2. 10, 20, 10 先绘制10个点，跳过20个点，绘制10个点，跳过10个点，再绘制20个点，如此反复

* strokeCap: {string} 设置线条终点形状，可取值：butt(0) , * square(2) 有个默认值 round(1)

* strokeJoin: {string} 设置线条连接点的风格，该属性支持如下三个值 可取值：miter(0), bevel(2) 有个默认值 round(1)

* strokeWidth: {number} 线宽 默认1

####Text
文字

* path 文字的路径

* font 字体相关 
	1. {string} 例如 ‘normal|bold|italic 13px 字体名称’ 可选单位如 px、pt、em 等

	2. {Object}

     1. font.fontFamily {string} 字体，可以使用逗号分隔

     2. fontSize {number}

     3. fontWeight {string} bold|normal

	 4. fontStyle {string} italic|normal

其余几个跟 Shape 一致的

####Path
所有的起点都是当前位置，往往是所在 Group 的x, y
xxTo:表示绝对坐标，反之是相对于起始点的距离

方法：
* move

* moveTo

* line

* lineTo

* reset

* close

* toJSON

* curve 曲线，三阶贝塞尔曲线，参数为 c1x, c1y, c2x, c2y, ex, ey

	1. (c1x, c1y): 第一个控制点坐标

   2. (c2x, c2y): 第二个控制点坐标

   3. (ex, ey): 曲线终点，取值是相对于起点的距离

*  curveTo 曲线，三阶贝塞尔曲线，跟 curve 唯一的区别是 (ex, ey) 是坐标值

*  arc 弧，参数x, y, rx, ry, outer, counterClockwise, 
rotation

	1. (x, y) 终点坐标距离起点坐标的偏移量

	1. (rx, ry) x,y轴的半径

	1. outer 外侧圆弧

	1. counterClockwise 是否顺时针

	1. rotation 角度

     counterArc

     counterArcTo
     
     
 #示例
 
 
````

testRenderLine=()=>{

            let w = WIDTH-140;
            let path1 = ART.Path()
            .line(w-3,1)
            .curve(3,0, 3, 3)
            .lineTo(w,36)
            // .arc(0,20, 10, 10,true,true,0)
            .curve(-7,10, 0, 20)
            .lineTo(w,96)
            .curve(0,3, -3, 3)
            .lineTo(0,99);
            return( 
                <ART.Surface width={w} height={100} style={{position:'absolute'}}>
                    <ART.Shape d={path1} strokeWidth={0.5} stroke='#f29c61'/>
                </ART.Surface>
            )
        }
````

