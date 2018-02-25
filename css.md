## 样式

####box-shadow

```css
/* offset-x | offset-y | blur-radius | spread-radius | color */
box-shadow: 2px 2px 2px 1px rgba(0, 0, 0, 0.2);
/* Any number of shadows, separated by commas */
box-shadow: 3px 3px red, -1em 0 0.4em olive;/*同时应用两种阴影*/
```

-   `inset`默认阴影在边框外。`使用inset后，`阴影在边框内（即使是透明边框），背景之上内容之下。
-   `<offset-x>` `<offset-y>`这是头两个值，用来设置阴影偏移量。`<offset-x>` 设置水平偏移量，如果是负值则阴影位于元素左边。 `<offset-y>` 设置垂直偏移量，如果是负值则阴影位于元素上面。如果两者都是0，那么阴影位于元素后面。这时如果设置了`<blur-radius>` `或<spread-radius>` 则有模糊效果。
-   `<blur-radius>`这是第三个值。值越大，模糊面积越大，阴影就越大越淡。 不能为负值。默认为0，此时阴影边缘锐利。
-   `<spread-radius>11`这是第四个值。取正值时，阴影扩大；取负值时，阴影.收缩。默认为0，此时阴影与元素同样大。

#### border-radius

border-radius: none　｜　length{1,4} / length{1,4} 
其中每一个值可以为 数值或百分比的形式。 
length/length　第一个lenght表示**水平方向**的直径，而第二个表示**竖直方向**的直径。

```css
border-radius: top-left top-right bottom-right bottom-left /top-left top-right bottom-right bottom-left
```



####background-size

`background-size`属性通过以下方式之一进行指定：

-   使用关键字值`contain`或`cover`。
-   仅使用宽度值，在这种情况下，高度默认为`auto`
-   使用宽度和高度值，在这种情况下，第一个设置宽度，第二个设置高度。值可以使`length`, ` 百分比(%)` ,` auto`


-   `contain`: 缩放图像尽可能大，而不裁剪或拉伸图像。
-   `cover`: 尽可能大的缩放图像而**不拉伸**图像。如果图像的比例与元素的比例不同，则会垂直或水平**裁剪**，因此不会留下空白空间。
-   `auto`: 按比例在相应的方向上缩放背景图像。
-   `length`: 将图像拉伸到指定尺寸。
-   `百分比`: 图像的尺寸=背景尺寸*百分比且是等比例缩放。背景定位区域由[`background-origin`](https://developer.mozilla.org/en-US/docs/Web/CSS/background-origin)（默认为填充框）的值确定。但是，如果背景的[`background-attachment`](https://developer.mozilla.org/en-US/docs/Web/CSS/background-attachment)值是`fixed`，则定位区域是整个[视口](https://developer.mozilla.org/en-US/docs/Glossary/viewport)。负值是不允许的。

#### outline

当该属性存在，点击会显示轮廓。

#### white-space

 white-space属性是用来设置如何处理元素中的空白。

`nowrap`和 normal 一样，连续的空白符会被合并。但文本内的换行无效。

`pre`连续的空白符会被保留。在遇到换行符或者[``](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/br)元素时才会换行。 

`pre-wrap`连续的空白符会被保留。在遇到换行符或者[``](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/br)元素，或者需要为了填充line盒子时才会换行。

`pre-line`连续的空白符会被合并。在遇到换行符或者[``](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/br)元素，或者需要为了填充line盒子时会换行。

#### ::after和::before

默认是行内元素的虚拟元素。作为已选中元素的最后一个子元素。通常会配合[`content`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/content)属性来为该元素添加装饰内容。

##响应式布局

想要实现自适应：

1.  首先，在网页代码的头部，加入一行[viewport元标签](https://developer.mozilla.org/en/mobile/viewport_meta_tag)。`viewport`是网页默认的宽度和高度，上面这行代码的意思是，网页宽度默认等于屏幕宽度（width=device-width），原始缩放比例（initial-scale=1）为1.0，即网页初始大小占屏幕面积的100%。
2.  不适用绝对宽度。具体说，CSS代码不能将宽度定死（px），只能设定百分比或者auto
3.  字体大小也不能用绝对大小（px），应该用em或者rem之类的；
4.  使用@meida
5.  使用max-width/height，min-width/height



#### @media

```css
　　@media screen and (max-device-width: 400px) {
　　　　.column {
　　　　　　float: none;
　　　　　　width:auto;
　　　　}
　　　　#sidebar {
　　　　　　display:none;
　　　　}
　　}/*如果屏幕宽度小于400像素，则column块取消浮动（float:none）、宽度自动调节（width:auto），sidebar块不显示（display:none）。*/
```



#### rem

浏览器对于根元素即`html`的字体大小最小值有限制，一般是12px，所以要设置font-size一般要大于12px，否则无效





------

## 定位

#### position

-   static
-   relative：相对定位的元素是在文档中的正常位置偏移给定的值，但是不影响其他元素的偏移。（未脱离文档流）
-   absolute：绝对定位元素相对于*最近的非 static 祖先元素*定位。当这样的祖先元素不存在时，则相对于根级容器定位。（脱离文档流）
-   fix：元素的相对于viewport视口，即网页可视的窗口定位。
-   sticky：粘性定位是相对定位和固定定位的混合。元素在跨越特定阈值前为相对定位，之后为固定定位。例如：侧边栏。该元素并不脱离文档流，仍然保留元素原本在文档流中的位置。须指定 `top`, `right`, `bottom` 或 `left` 四个阈值其中之一，才可使粘性定位生效。

####  top right bottom left

当元素定位非static时有效。

#### text-align

`text-align` CSS属性定义行内内容（例如文字）如何相对它的块父元素对齐。并不会控制设置这个属性的块怎么对齐，只控制这个块内的元素怎么对齐。









##选择器

#### 伪类:nth-child和:nth-of-type

参数从1开始，可以使是形如3n的变量。

p:nth-child(2)：意思是，父元素的第二个子元素，如果它的标签名是`p`则应用样式。

p:nth-of-type(2)：父元素的第二个`p`标签应用该样式。

#### 伪类:last-child

同一个父元素 (非body)下的一组相同选择器中的最后一个元素。



## 文字

#### letter-spacing

CSS 的`letter-spacing` 属性明确了文字的间距行为。 

#### text-indent

`text-indent` 属性 规定了 一个元素 首行 文本内容之前应该有多少水平空格。通常用于段落首行缩进。在input中使用类似于padding。

