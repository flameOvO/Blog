## window对象

-   `window.window`指向自身；
-   `window.name`属性用于设置当前浏览器窗口的名字。
-   `window.location`返回一个`location`对象，用于获取窗口当前的URL信息。等同于`document.location`对象
-   `window.closed`属性返回一个布尔值，表示窗口是否关闭。一般用来检查，使用脚本打开的新窗口是否关闭

```
var popup = window.open();

if ((popup !== null) && !popup.closed) {
  // 窗口仍然打开着
}
```

-   `window.opener`属性返回打开当前窗口的父窗口。如果当前窗口没有父窗口，则返回`null`。通过`opener`属性，可以获得父窗口的的全局变量和方法，比如`window.opener.propertyName`和`window.opener.functionName()`。但这只限于两个窗口属于同源的情况且其中一个窗口由另一个打开。

-   `window.screenX`和`window.screenY`属性，返回浏览器窗口左上角相对于**当前屏幕**左上角（`(0, 0)`）的水平距离和垂直距离，单位为像素。

-   `window.innerHeight`和`window.innerWidth`属性，返回网页在当前窗口中可见部分的高度和宽度，即“视口”（viewport），单位为**像素**。**属性值包括滚动条的高度和宽度。**

    当用户放大网页的时候（比如将网页从100%的大小放大为200%），这两个属性会变小。因为这时网页的像素大小不变（比如宽度还是960像素），只是**每个像素占据的屏幕空间变大了，因为可见部分（视口）就变小了。**

-   `window.outerHeight`和`window.outerWidth`属性返回浏览器窗口的高度和宽度，包括浏览器菜单和边框，单位为像素。

-   `pageYOffset` 属性是 `scrollY` 属性的别名：**返回**文档在垂直方向已滚动的像素值。`window.pageXOffset`属性是`scrollX` 的别名，返回页面的水平滚动距离，单位都为**像素**， 只可读。

## navigator对象

*   `window`对象的`navigator`属性
*   `navigator.userAgent`属性返回浏览器的User-Agent字符串，标示浏览器的厂商和版本信息。通过`userAgent`可以大致准确地识别手机浏览器，方法就是测试是否包含`mobi` 之类的字符串。
*   `navigator.plugins`属性返回一个类似数组的对象，成员是浏览器安装的插件，比如Flash、ActiveX等。
*   `navigator.platform`属性返回是平台上的操作系统信息。
*   `navigator.onLine`属性返回一个布尔值，表示用户当前在线还是离线。
*   `navigator.geolocation`返回一个Geolocation对象，包含用户地理位置的信息。
*   `javaEnabled`方法返回一个布尔值，表示浏览器是否能运行Java Applet小程序。
*   `cookieEnabled`属性返回一个布尔值，表示浏览器是否能储存Cookie。

## window.screen对象

*   `window.screen`对象包含了**显示设备**的信息。

*   `screen.height`和`screen.width`两个属性，一般用来了解设备的分辨率。该值只与显示器分辨率有关。

*   `window.scrollTo(X,Y)`方法用于将网页的指定位置，滚动到浏览器左上角。

*   `window.scrollBy`方法用于将网页移动指定距离，单位为像素。它接受两个参数：向右滚动的像素，向下滚动的像素。

*   `window.open`方法用于新建另一个浏览器窗口，并且返回该窗口对象。

    *   `open`方法一共可以接受四个参数

    -   第一个参数：字符串，表示新窗口的网址。如果省略，默认网址就是`about:blank`。
    -   第二个参数：字符串，表示新窗口的名字。如果该名字的窗口已经存在，则跳到该窗口，不再新建窗口。如果省略，就默认使用`_blank`，表示新建一个没有名字的窗口。
    -   第三个参数：字符串，内容为逗号分隔的键值对，表示新窗口的参数，比如有没有提示栏、工具条等等。如果省略，则默认打开一个完整UI的新窗口。
    -   第四个参数：布尔值，表示第一个参数指定的网址，是否应该替换`history`对象之中的当前网址记录，默认值为`false`。显然，这个参数只有在第二个参数指向已经存在的窗口时，才有意义。

*   `window.close`方法用于关闭当前窗口，一般用来关闭`window.open`方法新建的窗口。

*   `focus`方法会激活指定当前窗口，使其获得焦点。

*   `window.getSelection`方法返回一个`Selection`对象，表示用户现在选中的文本。使用`Selction`对象的`toString`方法可以得到选中的文本。



## 多窗口操作

-   `top`：顶层窗口，即最上层的那个窗口
-   `parent`：父窗口
-   `self`：当前窗口，即自身
-   浏览器还提供一些特殊的窗口名，供`open`方法、`<a>`标签、`<form>`标签等引用。
    -   `_top`：顶层窗口
    -   `_parent`：父窗口
    -   `_blank`：新窗口