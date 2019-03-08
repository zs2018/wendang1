## 1、怎样使用Canvas绘图？

要使用<canvas>元素，必须先设置其 width 和 height 属性，指定可以绘图的区域大小。

要在这块画布(canvas)上绘图，需要取得绘图上下文。而取得绘图上下文对象的引用，需要调用getContext()方法并传入上下文的名字。传入"2d"，就可以取得 2D 上下文对象。	

```js
var drawing = document.getElementById("drawing");
//确定浏览器支持<canvas>元素 
if (drawing.getContext){
	//取得图像的数据 URI
	var imgURI = drawing.toDataURL("image/png");
	//显示图像
	var image = document.createElement("img"); 
	image.src = imgURI; 			  			
	document.body.appendChild(image);
}
```

使用 toDataURL()方法，可以导出在<canvas>元素上绘制的图像。这个方法接受一个参数，即图像的 MIME 类型格式，而且适合用于创建图像的任何上下文。

## 2、绘图中需要注意的点

1、2D 上下文的两种**基本绘图**操作是填充和描边。填充，就是用指定的样式(颜色、渐变或图像)填充图形;描边，就是只在图形的边缘画线。大多数 2D 上下文操作都会细分为填充和描边两个操作，而操作的结果取决于两个属性:**fillStyle** 和 **strokeStyle**。

```js
var drawing = document.getElementById("drawing");
//确定浏览器支持<canvas>元素 
if (drawing.getContext){
    var context = drawing.getContext("2d");
    context.strokeStyle = "red";
    context.fillStyle = "#0000ff";
}
```

2、**矩形**是唯一一种可以直接在 2D 上下文中绘制的形状。与矩形有关的方法包括 fillRect()、strokeRect()和 clearRect()。这三个方法都能接收 4 个参数:矩形的 x 坐标、矩形的 y 坐标、矩形宽度和矩形高度。这些参数的单位都是像素。

```js
var drawing = document.getElementById("drawing");
//确定浏览器支持<canvas>元素 
if (drawing.getContext){
    var context = drawing.getContext("2d");

    //绘制红色矩形
    context.fillStyle = "#ff0000"; 
    context.fillRect(10, 10, 50, 50);
    
    //绘制半透明的蓝色矩形
    context.fillStyle = "rgba(0,0,255,0.5)"; 
    context.fillRect(30, 30, 50, 50);
    
    //绘制红色描边矩形
	context.strokeStyle = "#ff0000"; 
    context.strokeRect(10, 10, 50, 50);
    
    //绘制半透明的蓝色描边矩形
    context.strokeStyle = "rgba(0,0,255,0.5)"; 
    context.strokeRect(30, 30, 50, 50);
    
    //在两个矩形重叠的地方清除一个小矩形
    context.clearRect(40, 40, 10, 10);
}
```


描边线条的宽度由 lineWidth 属性控制，该属性的值可以是任意整数。另外，通过 lineCap 属性可以控制线条末端的形状是平头、圆头还是方头("butt"、"round"或"square")，通过 lineJoin 属性可以控制线条相交的方式是圆交、斜交还是斜接("round"、"bevel"或"miter")。


3、要**绘制路径**，首先必须调用 beginPath()方法，表示要开始绘制新路径。然后，再通过调用下列方法来实际地绘制路径。

- arc(x, y, radius, startAngle, endAngle, counterclockwise):以(x,y)为圆心绘制一条弧线，弧线半径为 radius，起始和结束角度(用弧度表示)分别为 startAngle 和endAngle。最后一个参数表示 startAngle 和 endAngle 是否按逆时针方向计算，值为 false表示按顺时针方向计算。

- arcTo(x1, y1, x2, y2, radius):从上一点开始绘制一条弧线，到(x2,y2)为止，并且以给定的半径 radius 穿过(x1,y1)。

- bezierCurveTo(c1x, c1y, c2x, c2y, x, y):从上一点开始绘制一条曲线，到(x,y)为止，并且以(c1x,c1y)和(c2x,c2y)为控制点。

- **lineTo**(x, y):从上一点开始绘制一条直线，到(x,y)为止。

- **moveTo**(x, y):将绘图游标移动到(x,y)，**不画线**。

- quadraticCurveTo(cx, cy, x, y):从上一点开始绘制一条二次曲线，到(x,y)为止，并且以(cx,cy)作为控制点。

	 rect(x, y, width, height):从点(x,y)开始绘**制一个矩形**，宽度和高度分别由 width 和height 指定。这个方法绘制的是矩形路径，而不是 strokeRect()和 fillRect()所绘制的独立的形状。			

  如果想绘制一条连接到路径起点的线条，可以调用**closePath()**。如果路径已经完成，你想用 fillStyle 填充它，可以调用 **fill()**方法。另外，还可以调用 **stroke()**方法对路径描边，描边使用的是 strokeStyle。最后还可以调用 **clip()**，这个方法可以在路径上创建一个剪切区域。		

  ```js
  var drawing = document.getElementById("drawing");
  //确定浏览器支持<canvas>元素 
  if (drawing.getContext){
      var context = drawing.getContext("2d");
      //开始路径 
      context.beginPath();
      //绘制外圆
  	context.arc(100, 100, 99, 0, 2 * Math.PI, false);
  	//绘制内圆
  	context.moveTo(194, 100);
  	context.arc(100, 100, 94, 0, 2 * Math.PI, false);
  	//绘制分针 
      context.moveTo(100, 100); 
      context.lineTo(100, 15);
  	//绘制时针 
      context.moveTo(100, 100); 
      context.lineTo(35, 100);
  	//描边路径
      context.stroke();
  }
  ```

4、绘制文本主要有两个方法:**fillText()**和 **strokeText()**。这两个方法都可以接收 4 个参数:要绘制的文本字符串、x 坐标、y 坐标和可选的最大像素宽度。

  - font:表示文本样式、大小及字体，用 CSS 中指定字体的格式来指定，例如"10px Arial"。	

  - textAlign:表示文本对齐方式。可能的值有"start"、"end"、"left"、"right"和"center"。建议使用"start"和"end"，不要使用"left"和"right"，因为前两者的意思更稳妥，能同时适合从左到右和从右到左显示(阅读)的语言	

  - textBaseline:表示文本的基线。可能的值有"top"、"hanging"、"middle"、"alphabetic"、"ideographic"和"bottom"


5、通过上下文的变换，可以把处理后的图像绘制到画布上。2D 绘制上下文支持各种基本的绘制变换。创建绘制上下文时，会以默认值初始化变换矩阵，在默认的变换矩阵下，所有处理都按描述直接绘制。为绘制上下文应用变换，会导致使用不同的变换矩阵应用处理，从而产生不同的结果。

- rotate(angle):围绕原点旋转图像 angle 弧度。
- scale(scaleX, scaleY):缩放图像，在 x 方向乘以 scaleX，在 y 方向乘以 scaleY。scaleX和 scaleY 的默认值都是 1.0。			


- translate(x,y):将坐标原点移动到(x,y)。执行这个变换之后，坐标(0,0)会变成之前由(x,y) 表示的点。		
- transform(m1_1, m1_2, m2_1, m2_2, dx, dy):直接修改变换矩阵，方式是乘以如下矩阵。

```
m1_1  m1_2  dx 
m2_1  m2_2  dy 
0     0     1
```

- setTransform(m1_1, m1_2, m2_1, m2_2, dx, dy):将变换矩阵重置为默认状态，然后再调用 transform()

```js
var drawing = document.getElementById("drawing");
//确定浏览器支持<canvas>元素 
if (drawing.getContext){
    var context = drawing.getContext("2d");
    //开始路径 
    context.beginPath();
    //绘制外圆
    context.arc(100, 100, 99, 0, 2 * Math.PI, false);
    //绘制内圆
    context.moveTo(194, 100);
    context.arc(100, 100, 94, 0, 2 * Math.PI, false);
    //变换原点 
    context.translate(100, 100);
    //绘制分针 
    context.moveTo(0,0); 
    context.lineTo(0, -85);
    //绘制时针 
    context.moveTo(0, 0); 
    context.lineTo(-65, 0);
    //描边路径
    context.stroke();
}	
```

6、如果你想把一幅图像绘制到画布上，可以使用 **drawImage()**方法。根据期望的最终结果不同，调用这个方法时，可以使用三种不同的参数组合。

最简单的调用方式是传入一个 HTML <img>元素，以及绘制该图像的起点的 x 和 y 坐标。例如:

```
var image = document.images[0];
context.drawImage(image, 10, 10);
```

如果你想改变绘制后图像的大小，可以再多传入两个参数，分别表示目标宽度和目标高度。通过这种方式来缩放图像并不影响上下文的变换矩阵。

```
context.drawImage(image, 50, 10, 20, 30);
```

还可以选择把图像中的某个区域绘制到上下文中。drawImage()方法的这种调用方式总共需要传入 9 个参数:要绘制的图像、源图像的 x 坐标、源图像的 y 坐标、源图像的宽度、源图像的高度、目标图像的 x 坐标、目标图像的 y 坐标、目标图像的宽度、目标图像的高度。

```
 context.drawImage(image, 0, 10, 50, 50, 0, 100, 40, 60);	
```

7、阴影

2D 上下文会根据以下几个属性的值，自动为形状或路径绘制出阴影。

- shadowColor:用 CSS 颜色格式表示的阴影颜色，默认为黑色。
- shadowOffsetX:形状或路径 x 轴方向的阴影偏移量，默认为 0。
- shadowOffsetY:形状或路径 y 轴方向的阴影偏移量，默认为 0。
- shadowBlur:模糊的像素数，默认 0，即不模糊。

```
var context = drawing.getContext("2d");
//设置阴影
context.shadowOffsetX = 5; 
context.shadowOffsetY = 5;
context.shadowBlur = 4; 
context.shadowColor = "rgba(0, 0, 0, 0.5)";
```

8、渐变

渐变由 CanvasGradient 实例表示，很容易通过 2D 上下文来创建和修改。要创建一个新的**线性渐变**，可以调用 **createLinearGradient()**方法。这个方法接收 4 个参数:起点的 x 坐标、起点的 y 坐标、终点的 x 坐标、终点的 y 坐标。

创建了渐变对象后，下一步就是使用 **addColorStop()**方法来指定色标这个方法接收两个参数:色标位置和 CSS 颜色值。色标位置是一个 0(开始的颜色)到 1(结束的颜色)之间的数字.	

```
var gradient = context.createLinearGradient(30, 30, 70, 70);
gradient.addColorStop(0, "white");//开始颜色
gradient.addColorStop(1, "black");//结束颜色
```

要创建**径向渐变(或放射渐变)**，可以使用 **createRadialGradient()**方法。这个方法接收 6 个参数，对应着两个圆的圆心和半径。前三个参数指定的是起点圆的原心(x 和 y)及半径，后三个参数指定的是终点圆的原心(x 和 y)及半径。

```
var gradient = context.createRadialGradient(55, 55, 10, 55, 55, 30);
gradient.addColorStop(0, "white");
gradient.addColorStop(1, "black");
```

9、模式

模式其实就是重复的图像，可以用来填充或描边图形。要创建一个新模式，可以调用**createPattern()**方法并传入两个参数:一个 HTML <img>元素和一个表示如何重复图像的字符串。其中，第二个参数的值与 CSS 的 background-repeat 属性值相同，包括"repeat"、"repeat-x"、"repeat-y"和"no-repeat"。

```
var image = document.images[0],
    pattern = context.createPattern(image, "repeat");

//绘制矩形
context.fillStyle = pattern; 
context.fillRect(10, 10, 150, 150);	
```

10、使用图像数据


2D 上下文的一个明显的长处就是，可以通过 getImageData()取得原始图像数据。这个方法接收4 个参数:要取得其数据的画面区域的 x 和 y 坐标以及该区域的像素宽度和高度。

```
var imageData = context.getImageData(10, 5, 50, 50);	
```

11、合成