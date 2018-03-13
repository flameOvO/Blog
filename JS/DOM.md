###CSS操作

#### style对象

**(是DOM上的style属性，为行内样式）**

操作 CSS 样式最简单的方法，就是使用网页元素节点的`getAttribute`方法、`setAttribute`方法和`removeAttribute`方法，直接读写或删除网页元素的`style`属性。

每个DOM节点对象都有个style属性，其值是个对象，该属性能直接操作来读写CSS样式。

这个对象所包含的属性与CSS规则一一对应，但是名字需要改写，比如`background-color`写成`backgroundColor`。如果CSS属性名是JavaScript保留字，则规则名之前需要加上字符串`css`，比如`float`写成`cssFloat`。

```js
var divStyle = document.querySelector('div').style;

divStyle.backgroundColor = 'red';
divStyle.border = '1px solid black';
divStyle.width = '100px';
divStyle.height = '100px';
divStyle.fontSize = '10em';

divStyle.backgroundColor // red
divStyle.border // 1px solid black
divStyle.height // 100px
divStyle.width // 100px
```

##### **style的cssText属性**

`cssText`可以读写或删除**整个**样式。`cssText`的属性值可以**不用**改写CSS属性名。

```js
var divStyle = document.querySelector('div').style;

divStyle.cssText = 'background-color: red;'
  + 'border: 1px solid black;'
  + 'height: 100px;'
  + 'width: 100px;';
```

##### setProperty()，getPropertyValue()，removeProperty()

-   `setProperty(propertyName,value)`：在CSS中添加某个CSS属性。
-   `getPropertyValue(propertyName)`：读取某个CSS属性。
-   `removeProperty(propertyName)`：删除某个CSS属性。

```js
var divStyle = document.querySelector('div').style;

divStyle.setProperty('background-color','red');
divStyle.getPropertyValue('background-color');
divStyle.removeProperty('background-color');
```

这三个方法的第一个参数，都是CSS属性名，且不用改写连词线。



#### window.getComputedStyle()

`window.getComputedStyle`方法接受一个DOM节点对象作为参数，返回一个包含该节点最终样式信息的对象(只读)。所谓“最终样式信息”，指的是各种CSS规则叠加后的结果。

```js
var div = document.querySelector('div');
window.getComputedStyle(div).backgroundColor
```

`getComputedStyle`方法还可以接受第二个参数，表示指定节点的**伪元素**（比如`:before`、`:after`、`:first-line`、`:first-letter`等）。

```js
var result = window.getComputedStyle(div, ':before');
```

想·

### StyleSheet对象

`StyleSheet`对象代表**网页**的一张样式表，它包括`<link>`节点加载的样式表和`<style>`节点内嵌的样式表。

`document`对象的`styleSheets`属性，可以返回当前页面的所有`StyleSheet`对象（即所有样式表）。它是一个类似数组的对象。

```
var sheets = document.styleSheets;
var sheet = document.styleSheets[0];
```

**media属性**

media属性表示这个样式表是用于屏幕（screen），还是用于打印（print），或两者都适用（all）。该属性只读，默认值是screen

**media属性**

media属性表示这个样式表是用于屏幕（screen），还是用于打印（print），或两者都适用（all）。该属性只读，默认值是screen

**type属性**

`type`属性返回`StyleSheet`对象的`type`值，通常是`text/css`。

**ownerNode属性**

`ownerNode`属性返回`StyleSheet`对象所在的DOM节点，通常是`<link>`或`<style>`。对于那些由其他样式表引用的样式表，该属性为`null`。

```js
// HTML代码为
// <link rel="StyleSheet" href="example.css" type="text/css" />

document.styleSheets[0].ownerNode // [object HTMLLinkElement]
```

### insertRule()，deleteRule()

`insertRule`方法用于在当前样式表的`cssRules`对象插入CSS规则，`deleteRule`方法用于删除`cssRules`对象的CSS规则。

```js
var sheet = document.querySelector('#styleElement').sheet;
sheet.insertRule('#block { color:white }', 0);
sheet.insertRule('p { color:red }',1);
sheet.deleteRule(1);
```

`insertRule`方法的第一个参数是表示CSS规则的字符串，第二个参数是该规则在`cssRules`对象的插入位置。`deleteRule`方法的参数是该条规则在`cssRules`对象中的位置。

### CSSRule接口

`cssRules`属性指向一个类似数组的对象，里面每一个成员就是当前样式表的一条CSS规则。使用该规则的`cssText`属性，可以得到CSS规则对应的字符串

```js
var sheet = document.querySelector('#styleElement').sheet;

sheet.cssRules[0].cssText
// "body { background-color: red; margin: 20px; }"

```

**（1）cssText**

cssText属性返回当前规则的文本。

**（2）parentStyleSheet**

parentStyleSheet属性返回定义当前规则的样式表对象。

**（3）parentRule**

parentRule返回包含当前规则的那条CSS规则。最典型的情况，就是当前规则包含在一个@media代码块之中。如果当前规则是顶层规则，则该属性返回null。

**（4）type**

type属性返回有一个整数值，表示当前规则的类型。



### CSSStyleRule接口

**（1）selectorText属性**

selectorText属性返回当前规则的选择器。

```js
var stylesheet = document.styleSheets[0];
stylesheet.cssRules[0].selectorText // ".myClass"

```

**（2）style属性**

style属性返回一个对象，代表当前规则的样式声明，也就是选择器后面的大括号里面的部分。该对象部署了CSSStyleDeclaration接口，使用它的cssText属性，可以返回所有样式声明，格式为字符串。

```js
document.styleSheets[0].cssRules[0].style.cssText
// "background-color: gray;font-size: 120%;"
```

### CSSMediaRule接口

如果一条CSS规则是@media代码块，那么它除了CSSRule接口，还部署了CSSMediaRule接口。

该接口主要提供一个media属性，可以返回@media代码块的media规则。

### CSSStyleDeclaration对象

每一条 CSS 规则的样式声明部分（大括号内部的部分），都是CSSStyleDeclaration对象的属性。不过，连词号需要变成骆驼拼写法。

-   `cssText`：当前规则的所有样式声明文本。该属性可读写，即可用来设置当前规则。
-   `length`：当前规则包含多少条声明。
-   `parentRule`：包含当前规则的那条规则，同 CSSRule 接口的`parentRule`属性。

### window.matchMedia()

`window.matchMedia`方法接受一个`mediaQuery`语句的字符串作为参数，返回一个[`MediaQueryList`](https://developer.mozilla.org/en-US/docs/DOM/MediaQueryList)对象。该对象有以下两个属性。

-   `media`：返回所查询的`mediaQuery`语句字符串。
-   `matches`：返回一个布尔值，表示当前环境是否匹配查询语句。

window.matchMedia方法返回的**MediaQueryList**对象有两个方法，用来监听事件：addListener方法和removeListener方法。如果mediaQuery查询结果发生变化，就调用指定的回调函数。

```
var mql = window.matchMedia("(max-width: 700px)");

// 指定回调函数
mql.addListener(mqCallback);

// 撤销回调函数
mql.removeListener(mqCallback);

function mqCallback(mql) {
  if (mql.matches) {
    // 宽度小于等于700像素
  } else {
    // 宽度大于700像素
  }
}
```

## CSS事件

CSS的过渡效果（transition）结束后，触发`transitionEnd`事件。

实际使用`transitionend`事件时，可能需要添加浏览器前缀。

### animationstart事件，animationend事件，animationiteration事件

-   animationstart事件：动画开始时触发。
-   animationend事件：动画结束时触发。
-   animationiteration事件：开始新一轮动画循环时触发。如果animation-iteration-count属性等于1，该事件不触发，即只播放一轮的CSS动画，不会触发animationiteration事件。

##动画暂停/播放

animation-play-state属性可以控制动画的状态（暂停/播放），该属性需求加上浏览器前缀。

```
element.style.webkitAnimationPlayState = "paused";
element.style.webkitAnimationPlayState = "running";
```