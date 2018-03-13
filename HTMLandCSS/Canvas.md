### 栅格

canvas元素默认被网格所覆盖。通常来说网格中的一个单元相当于canvas元素中的一像素。栅格的起点为左上角（坐标为（0,0））。所有元素的位置都相对于原点定位。

### 绘制方法

-   `moveTo(x,y)`:将笔触移动到(x,y)
-   `lineTo(x,y)`:绘制一条从当前位置到(x,y)的直线。
-   `arc(x, y, radius, startAngle, endAngle, anticlockwise)`:画一个以（x,y）为圆心的以radius为半径的圆弧（圆），从startAngle（单位弧度）开始到endAngle结束，按照anticlockwise给定的方向（默认false为顺时针）来生成。`arc(75,75,35,0,Math.PI,false)`;
-   `ctx.arcTo(x1, y1, x2, y2, radius)`当前描点与给定的控制点1连接的直线为切线1，控制点1与控制点2连接的直线为切线2，作为使用指定半径的圆的**切线**，画出两条切线之间的弧线路径。
-   `quadraticCurveTo(cp1x, cp1y, x, y)`绘制二次贝塞尔曲线，`cp1x,cp1y`为一个控制点，`x,y为`结束点。
-   `bezierCurveTo(cp1x, cp1y, cp2x, cp2y, x, y)`绘制三次贝塞尔曲线，`cp1x,cp1y`为控制点一，`cp2x,cp2y`为控制点二，`x,y`为结束点。
-   `ctx.rect(x,y,width,height)` 等同于`strokeRect ` 绘制矩形。
-   `boolean ctx.isPointInPath(x, y)`判断当前路径是否包含检测点(x,y)。
-   `boolean ctx.isPointInStroke(x, y)` 判断检测点是否在秒便是

# CanvasRenderingContext2D——API





###绘制矩形

-   `fillRect(x, y, width, height)`   绘制一个填充的矩形
-   `strokeRect(x, y, width, height) `  绘制一个矩形的边框
-   `clearRect(x, y, width, height) `   清除指定矩形区域，让清除部分完全透明。



### 绘制文本

-   `ctx.fillText(text, x, y [, maxWidth]);`是 Canvas 2D API 在 *(x, y)*位置填充文本的方法。如果选项的第四个参数提供了最大宽度，文本会进行缩放以适应最大宽度。


-   `CanvasRenderingContext2D.strokeText()`在(x,y)位置绘制（描边）文本。


-   `ctx.measureText(text)` 方法返回一个 [`TextMetrics`](https://developer.mozilla.org/zh-CN/docs/Web/API/TextMetrics) 对象，包含关于文本尺寸的信息（例如文本的宽度）。



### 线型

-   `ctx.lineWidth = value`是 Canvas 2D API 设置线段厚度的**属性**（即线段的宽度）。`value`

	述线段宽度的数字。 0、 负数、 [`Infinity`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Infinity) 和 [`NaN`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/NaN) 会被忽略。	

-   `ctx.lineCap`是 Canvas 2D API 指定如何绘制每一条线段末端的属性。`butt`线段末端以方形结束。

`round`线段末端以圆形结束。`square`线段末端以方形结束，但是增加了一个宽度和线段相同，高度是线段厚度一半的矩形区域。

*   `ctx.lineJoin`是 Canvas 2D API 指定用来设置2个长度不为0的相连部分（线段，圆弧，曲线）如何连接在一起的属性。类似于拐角处的样式。
*   `ctx.miterLimit = value` 是用来设定外延交点与连接点的最大距离，如果交点距离大于此值，连接效果会变成了 bevel。


*   `ctx.getLineDash()`是 Canvas 2D API 获取当前线段样式的方法。返回值:当前线段样式的数组，数组包含一组数量为偶数的非负数数字。
	   ` ctx.setLineDash(segments)`是 Canvas 2D API 设置虚线样式的方法。`segments`:一个Array数组。一组描述交替绘制线段和间距（坐标空间单位）长度的数字。 如果数组元素的数量是奇数， 数组的元素会被复制并重复。例如， [5, 15, 25] 会变成 [5, 15, 25, 5, 15, 25]。	
*   `ctx.lineDashOffset = value` 是设置虚线偏移量的属性，一般通过修改该属性达到虚线滚动动画效果。



### 文本样式

+   `ctx.font = value`  设置当前字体样式的属性。 使用和 [CSS font](https://developer.mozilla.org/en-US/docs/Web/CSS/font) 规范相同的字符串值。
+   `ctx.direction = "ltr" || "rtl" || "inherit"` 描述绘制的文本方向的属性。
+   `ctx.textAlign = "left" || "right" || "center" || "start" || "end"`;是文本的对齐方式的属性。注意，该对齐是基于CanvasRenderingContext2D.fillText方法的x的值。所以如果textAlign="center"，那么该文本将画在 x-50%*width。也就是说文本一半在**x**的左边，一半在**x**的右边



### 填充和描边样式

 想要给图形上色，有两个重要的属性可以做到：`fillStyle` 和 `strokeStyle。`

+   `fillStyle`描述图形内部颜色和样式的属性。(填充)

```js
ctx.fillStyle = color;
ctx.fillStyle = gradient;//CanvasGradient 对象 （线性渐变或者放射性渐变）.
ctx.fillStyle = pattern; //CanvasPattern 对象 （可重复图像）
```

+   `strokeStyle` 类比border，属性同上。



### 渐变和图案

*   `CanvasGradient ctx.createLinearGradient(x0, y0, x1, y1)`创建一个沿参数坐标指定的直线方向的渐变。该方法返回一个线性 `CanvasGradient`对象。 创建成功后， 你就可以使用 `CanvasGradient.addColorStop()` 方法，根据指定的偏移和颜色定义一个新的终止。

*   `CanvasGradient ctx.createRadialGradient(x0, y0, r0, x1, y1, r1)`类似上述，不过是朝四面八方渐变，为圆。

*   `CanvasPattern ctx.createPattern(image, repetition)` 

    *   repetition : "repeat" (both directions)||"repeat-x" (oneside only)||"no-repeat" (neither).

    指定如何重复图像，如果为空字符串 ('') 或 null (但不是 undefined)，repetition将被当作"repeat"。

    *   image:图像来源

### 阴影 

`ctx.shadowBlur = level`设置模糊阴影,只有设置shadowColor属性值为不透明，阴影才会被绘制。

`ctx.shadowColor = color` 阴影颜色

`ctx.shadowOffsetX(or Y) = offset `阴影的水平或者垂直偏移。

### 绘制路径

路径是通过不同颜色和宽度的线段或曲线相连形成的不同形状的点的集合。一个路径，甚至一个子路径，都是闭合的。

1.  首先，你需要创建路径起始点。

2.  然后你使用[画图命令](https://developer.mozilla.org/en-US/docs/Web/API/CanvasRenderingContext2D#Paths)去画出路径。

3.  之后你把路径封闭。

4.  一旦路径生成，你就能通过描边或填充路径区域来渲染图形。

    所要用到的函数:

-   `beginPath()`清空子路径列表，新建一条路径，生成之后，图形绘制命令被指向到路径上生成路径。
-   `closePath()`闭合路径之后图形绘制命令又重新指向到上下文中。
-   `stroke()`通过线条来绘制图形轮廓。
-   `fill()`通过填充路径的内容区域生成实心的图形。

当前路径为空，即调用beginPath()之后，或者canvas刚建的时候，第一条路径构造命令通常被视为是moveTo（），无论实际上是什么。出于这个原因，你几乎总是要在设置路径之后专门指定你的起始位置。

当你调用fill()函数时，所有没有闭合的形状都会自动闭合，所以你不需要调用closePath()函数。但是调用stroke()时不会自动闭合。

```javascript
   ctx.beginPath();//开始构建路径
   ctx.moveTo(75,50);//将笔触移动到（x,y）即设置起始点
   ctx.lineTo(100,75);//从起始点绘制直线到下一个点
   ctx.lineTo(100,25);
   ctx.lineTo(75,25);//当前点
   ctx.closePath();//闭合路径，若调用fill可不写
   ctx.fill();//链接起始点与当前点，并填充该封闭图形
```



### 变换

*   `rotate() `旋转中心点一直是 canvas 的**起始点**(0,0)。如果想改变中心点，我们可以通过 translate() 方法移动 canvas 。

*   `scale()在 canvas 中一个单位实际上就是一个像素。` 例如，如果我们将0.5作为缩放因子，最终的单位会变成0.5像素，并且形状的尺寸会变成原来的一半

*   `ctx.translate(x, y) `x:水平方向的移动距离。y:垂直方向.

	   `ctx.transform(a, b, c, d, e, f)` 变换矩阵的描述： [ a	c	e	

    ​												b	d	f	

    ​												0	0	1 ]

    *    a (m11)水平缩放。b (m12)水平倾斜。c (m21)垂直倾斜。d (m22)垂直缩放。e (dx)水平移动。f (dy)垂直移动。

`ctx.setTransform(a, b, c, d, e, f)` 修改transform

### 状态

`save() `是 Canvas 2D API 通过将当前状态放入栈中，保存 canvas 全部状态的方法。状态是例如fillStyle等属性设置的值。

保存到栈中的绘制状态有下面部分组成：

当前的变换矩阵。
当前的剪切区域。
当前的虚线列表.
以下属性当前的值： strokeStyle, fillStyle, globalAlpha, lineWidth, lineCap, lineJoin, miterLimit, lineDashOffset, shadowOffsetX, shadowOffsetY, shadowBlur, shadowColor, globalCompositeOperation, font, textAlign, textBaseline, direction, imageSmoothingEnabled.

`restore()` 是 Canvas 2D API 通过在绘图状态栈中弹出顶端的状态，将 canvas 恢复到最近的保存状态的方法。 如果没有保存状态，此方法不做任何改变。

`ctx.canvas = HTMLCanvasElement || null  ` 属性是只读的，是 HTMLCanvasElement的反向引用。

### 绘制图像

```js
void ctx.drawImage(image, dx, dy);
void ctx.drawImage(image, dx, dy, dWidth, dHeight);
void ctx.drawImage(image, sx, sy, sWidth, sHeight, dx, dy, dWidth, dHeight);
```

image 绘制到上下文的元素。
(dx,dy)是目标图像相对于画布的定位
(dWidth，dHeight)是目标图像在画布上的尺寸
(sx,sy)是相对源图像的(sx，sy)为目标图像的左上角开始提取图片
sy 将要被提取的图像数据矩形区域的左上角 y 坐标。
(sWidth，sHeight)表示在源图像中截取的图像尺寸。

### 像素控制

```js
ImageData ctx.createImageData(width, height);//生成指定宽高的空白对象
ImageData ctx.createImageData(imagedata);//从现有的 ImageData 对象中，复制一个和其宽度和高度相同的对象。图像自身不允许被复制。
```

`ImageData ctx.getImageData(sx, sy, sw, sh)`  

*   `sx`将要被提取的图像数据矩形区域的左上角 x 坐标。
*   `sy`将要被提取的图像数据矩形区域的左上角 y 坐标。
*   `sw`将要被提取的图像数据矩形区域的宽度。
*   `sh`将要被提取的图像数据矩形区域的高度。

`CanvasRenderingContext2D.putImageData() ` 是 Canvas 2D API 将数据从已有的 [`ImageData`](https://developer.mozilla.org/zh-CN/docs/Web/API/ImageData) 对象绘制到位图的方法。 如果提供了一个绘制过的矩形，则只绘制该矩形的像素。此方法不受画布转换矩阵的影响。

```
void ctx.putImageData(imagedata, dx, dy);
void ctx.putImageData(imagedata, dx, dy, dirtyX, dirtyY, dirtyWidth, dirtyHeight);
```

### 合成

`ctx.globalAlpha = value` 设置图形和图片透明度的属性,数值的范围从 0.0 （完全透明）到1.0 （完全不透明）。

`ctx.globalCompositeOperation = type`设置要在绘制新形状时应用的合成操作的类型，其中type是用于标识要使用的合成或混合模式操作的字符串。

**type:**

*   source-over :  在目标图形的顶部绘制源图形，两者都显示。
*   source-out：只显示源图形中不与目标图形重叠的部分，目标图形不显示。
*   source-in：只显示源图形中与目标图形重叠的部分，目标图形不显示
*   source-atop：显示源图形中与目标图形重叠的部分，目标图形显示
*   destination-over：源图形在目标图形下方。
*   destination-out：只显示目标图形中不与源图形重叠的部分，源图形不显示。
*   destination-in： 与source-in相反
*   destination-atop：与source-atop相反
*   lighter：源图形与目标图形皆显示，重叠部分颜色会混合。
*   darker：与source-over相同
*   copy：只显示源图形，不显示目标图形。
*   xor：重叠部分不显示。

![img](http://dl.iteye.com/upload/picture/pic/71058/9292f201-bb95-30ec-8920-b9836bd20baa.jpg)