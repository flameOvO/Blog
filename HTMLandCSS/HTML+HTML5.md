#### 浏览器渲染页面DOM文档加载的步骤：

1.解析HTML结构。

2.加载外部脚本和css文件。

3.解析并执行脚本代码。

4.DOM树构建完成。(此时会触发DOMContentLoaded事件)

5.加载外部图片等文件。

6.页面加载完毕。(此时会触发load事件)

## Web Storage

-   `sessionStorage` 为每一个给定的源（given origin）维持一个独立的存储区域，该存储区域在页面会话期间可用（即只要浏览器处于打开状态，包括页面重新加载和恢复）。**新开一个标签页无效**
-   `localStorage` 同样的功能，但是在浏览器关闭，然后重新打开后数据仍然存在。

####localStorage



#### sessionStorage

## 行内元素与块级元素

#### 行内元素

-   b, big, i, small, tt
-   abbr, acronym, cite, code, dfn, em, kbd, strong, samp, var
-   `a`, bdo, br, `img,` map, object, q, script, `span`, sub, sup
-   `button`, `input`, `label`,` select`, textarea

###块级元素

`<address>`联系方式信息。 `<blockquote>`  块引用。`<pre>`预格式化文本。
`<dd>` 定义列表中定义条目描述。`<div>` 文档分区。`<dl>` 定义列表。
`<h1>, <h2>, <h3>, <h4>, <h5>, <h6>`标题级别 1-6. `<form>`表单。

`<hr>`水平分割线。`<noscript>`不支持脚本或禁用脚本时显示的内容。。

`<ul>`有序列表。`<ol>`有序列表 `<table>`	表格。 `<p>`行。

##### HTML5:

`<header> `,区段头或页头。`<fieldset>` 表单元素分组。`<figcaption>`  ,**figure**的标题
`<figure> `图文信息组 。`<aside>` ,伴随内容。`<audio>`  音频播放。
`<footer> `,区段尾或页尾。`<hgroup> `,标题组。`<article> `文章内容。  

`<canvas>` ,绘制图形。`<section>` ,一个页面区段。`<video>`视频。

`<output>`  表单输出。



#### requestAnimationFrame

#### DOMContentLoaded 

`DOMContentLoaded`事件会在DOM解析完成后再开始执行回调函数。

```js
<script>
  document.addEventListener("DOMContentLoaded", function(event) {
      console.log("DOM fully loaded and parsed");
  });
</script>
```



#### [自定义数据属性](https://developer.mozilla.org/en/HTML/Global_attributes#attr-data-*)(data-*)集

自定义data-*可以通过dataset.\*来访问。

自定义的data 属性名称转化成 [`DOMStringMap`](https://developer.mozilla.org/zh-CN/docs/Web/API/DOMStringMap) 的键值时会遵循下面的规则：

-   前缀  `data-` 被去除(包括减号)；
-   对于每个在ASCII小写字母 `a到` `z前面`的减号 (`U+002D`)，减号会被去除，并且字母会转变成对应的大写字母。
-   其他字符（包含其他减号）都不发生变化

```html
<div id="user" data-id="1234567890" data-user="johndoe" data-date-of-birth>John Doe
</div>

var el = document.querySelector('#user');
// el.id == 'user'
// el.dataset.id === '1234567890'
// el.dataset.user === 'johndoe'
// el.dataset.dateOfBirth === ''
```




#### atrribute

**autofocus** [HTML5](https://developer.mozilla.org/zh-CN/docs/HTML/HTML5)

这个布尔属性允许您指定的表单控件在页面加载时具有焦点（自动获得焦点），除非用户将其覆盖，例如通过键入不同的控件。文档中只有一个表单元素可以具有autofocus属性，它是一个布尔值。 如果type属性设置为隐藏则不能应用（即您不能自动获得焦点的属性设置为隐藏的控件）。

**autosave** [HTML5](https://developer.mozilla.org/zh-CN/docs/HTML/HTML5)

This attribute should be defined as a unique value. If the value of the type attribute is `search`, previous search term values will persist in the dropdown across page load.

**checked**

如果该元素的**type**属性的值为radio或者checkbox,则该布尔属性的存在与否表明了该控件是否是默认选择状态.

**disabled**

这个布尔属性表示此表单控件不可用。 特别是在禁用的控件中， `click` 事件 [将不会被分发](http://www.whatwg.org/specs/web-apps/current-work/multipage/association-of-controls-and-forms.html#enabling-and-disabling-form-controls) 。 并且，禁用的控件的值在提交表单时也不会被提交。如果 **type** 属性为  hidden，此属性将被忽略。

**form** [HTML5](https://developer.mozilla.org/zh-CN/docs/HTML/HTML5)

The form element that the input element is associated with (its *form owner*). The value of the attribute must be an **id** of a [``](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/form) element in the same document. If this attribute is not specified, this <input> element must be a descendant of a [``](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/form)element. This attribute enables you to place <input> elements anywhere within a document, not just as descendants of their form elements.

**formaction**[HTML5](https://developer.mozilla.org/zh-CN/docs/HTML/HTML5)

The URI of a program that processes the information submitted by the input element, if it is a submit button or image. If specified, it overrides the `action` attribute of the element's form owner.

**formenctype**[HTML5](https://developer.mozilla.org/zh-CN/docs/HTML/HTML5)

-   application/x-www-form-urlencoded: The default value if the attribute is not specified.
-   multipart/form-data: Use this value if you are using an [``](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/input) element with the `type` attribute set to file.
-   text/plain

If this attribute is specified, it overrides the `enctype` attribute of the element's form owner.

**formmethod**[HTML5](https://developer.mozilla.org/zh-CN/docs/HTML/HTML5)

-   post: The data from the form is included in the body of the form and is sent to the server.
-   get: The data from the form are appended to the **form** attribute URI, with a '?' as a separator, and the resulting URI is sent to the server. Use this method when the form has no side-effects and contains only ASCII characters.

If specified, this attribute overrides the `method` attribute of the element's form owner.

**formnovalidate**[HTML5](https://developer.mozilla.org/zh-CN/docs/HTML/HTML5)

If the input element is a submit button or image, this Boolean attribute specifies that the form is not to be validated when it is submitted. If this attribute is specified, it overrides the `novalidate` attribute of the element's form owner.

**formtarget**[HTML5](https://developer.mozilla.org/zh-CN/docs/HTML/HTML5)

If the input element is a submit button or image, this attribute is a name or keyword indicating where to display the response that is received after submitting the form. This is a name of, or keyword for, a browsing context (for example, tab, window, or inline frame). If this attribute is specified, it overrides the **target** attribute of the elements's form owner. The following keywords have special meanings:

-   _self: Load the response into the same browsing context as the current one. This value is the default if the attribute is not specified.
-   _blank: Load the response into a new unnamed browsing context.
-   _parent: Load the response into the parent browsing context of the current one. If there is no parent, this option behaves the same way as _self.
-   _top: Load the response into the top-level browsing context (that is, the browsing context that is an ancestor of the current one, and has no parent). If there is no parent, this option behaves the same way as _self.

**height** [HTML5](https://developer.mozilla.org/zh-CN/docs/HTML/HTML5)

如果**type**属性的值是image，这个属性定义了按钮图片的高度。

**list** [HTML5](https://developer.mozilla.org/zh-CN/docs/HTML/HTML5)

Identifies a list of pre-defined options to suggest to the user. The value must be the **id**of a [``](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/datalist) element in the same document. The browser displays only options that are valid values for this input element. This attribute is ignored when the **type**attribute's value is hidden, checkbox, radio, file, or a button type.

**max** [HTML5](https://developer.mozilla.org/zh-CN/docs/HTML/HTML5) 

此项目的最大（数字或日期时间）值，且不得小于其最小值（**min**属性）值。

**maxlength** [HTML5](https://developer.mozilla.org/zh-CN/docs/HTML/HTML5)

如果 **type** 的值是  text, email, search, password, tel, 或 url，那么这个属性指明了用户最多可以输入的字符个数（按照Unicode编码方式计数）；对于其他类型的输入框，该属性被忽略。它可以大于 **size** 属性的值。如果不指定这个属性，那么用户可以输入任意多的字符。如果指定为一个负值，那么元素表现出默认行为，即用户可以输入任意多的字符。本属性的约束规则，仅在元素的 value 属性发生变化时才会执行。译者注:ie10+

**min** [HTML5](https://developer.mozilla.org/zh-CN/docs/HTML/HTML5) 

此项目的最小（数字或日期时间）值，且不得大于其最大值（最大属性）值。

**multiple**[HTML5](https://developer.mozilla.org/zh-CN/docs/HTML/HTML5)

This Boolean attribute indicates whether the user can enter more than one value.这个属性指示用户能否输入多个值。这个属性仅在**type**属性为email或file的时候生效 ; 否则被忽视.

**name**

控件的名称，与表单数据一起提交。

**pattern**[HTML5](https://developer.mozilla.org/zh-CN/docs/HTML/HTML5)

检查控件值的正则表达式.。pattern必须匹配整个值，而不仅仅是某些子集.。使用title属性来描述帮助用户的模式.。当类型属性的值为text, search, tel, url 或 email时，此属性适用，否则将被忽略。译者注:ie10+

**placeholder** [HTML5](https://developer.mozilla.org/zh-CN/docs/HTML/HTML5)

提示用户输入框的作用。用于提示的占位符文本不能包含回车或换行。仅适用于当**type** 属性为text, search, tel, url or email时; 否则会被忽略。

Note: 请不要用`placeholder` 属性替换 [``](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/label) 元素。他们的作用不同:  [``](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/label) 属性描述表单元素的角色; 也就是说，它展示预期的信息，而`placeholder` 属性是提示用户内容的输入格式。某些情况下 `placeholder` 属性对用户不可见, 所以当没有它时也需要保证form能被理解。

**readonly**

[HTML5](https://developer.mozilla.org/zh-CN/docs/HTML/HTML5) 如果控件的 **type** 属性为hidden, range, color, checkbox, radio, file 或 type时，此属性会被忽略。

**required** [HTML5](https://developer.mozilla.org/zh-CN/docs/HTML/HTML5)

这个属性指定用户在提交表单之前必须为该元素填充值. 当type属性是hidden,image或者按钮类型(submit,reset,button)时不可使用. [`:optional`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:optional) 和 [`:required`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:required) CSS 伪元素的样式将可以被该字段应用作外观.

**selectionDirection** [HTML5](https://developer.mozilla.org/zh-CN/docs/HTML/HTML5)

The direction in which selection occurred. This is "forward" if the selection was made from left-to-right in an LTR locale or right-to-left in an RTL locale, or "backward" if the selection was made in the opposite direction. This can be "none" if the selection direction is unknown.

**size**

控件的初始大小。以像素为单位。但当**type**  属性为text 或 password时, 它表示输入的字符的长度。从HTML5开始, 此属性仅适用于当 **type** 属性为 text, search, tel, url, email,或 password；否则会被忽略。 此外，它的值必须大于0。 如果未指定大小，则使用默认值20。 HTML5 概述 "用户代理应该确保至少大部分字符是可见的", 但是不同的字符的用不同的字体表示可能会导致宽度不同。在某些浏览器中，一串带有x的字符即使定义了到x的大小也将显示不完整。 。

**spellcheck** [HTML5](https://developer.mozilla.org/zh-CN/docs/HTML/HTML5)

将此属性的值设置为`true`表示元素需要检查其拼写和语法。值`default` 表示该元素将根据默认行为进行操作，可能基于父元素自己的`spellcheck`值。值`false`表示不应该检查元素

**src**

如果**type**属性的值是image, 这个属性指定了按钮图片的路径; 否则它被忽视.

**step** [HTML5](https://developer.mozilla.org/zh-CN/docs/HTML/HTML5)

使用**min**和**max** 属性来限制可以设置数字或日期时间值的增量。它可以是任意字符串或是正浮点数。如果此属性未设置为任何，则控件仅接受大于最小步长值的倍数的值。

**tabindex** element-specific in [HTML 4](https://developer.mozilla.org/zh-CN/docs/HTML), global in [HTML5](https://developer.mozilla.org/zh-CN/docs/HTML/HTML5)

元素在当前文档的Tab导航顺序中的位置。

**usemap** [HTML 4](https://developer.mozilla.org/zh-CN/docs/HTML) only, 已废弃 [HTML5](https://developer.mozilla.org/zh-CN/docs/HTML/HTML5)

作为图像映射的[``](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/map)元素的名称.

**value**

控件的初始值. 此属性是可选的，除非**type** 属性是radio或checkbox.

请注意，当重新加载页面时，如果在重新加载之前更改了值，[Gecko和IE将忽略HTML源代码中指定的值](https://bugzilla.mozilla.org/show_bug.cgi?id=46845#c186)。

**width** [HTML5](https://developer.mozilla.org/zh-CN/docs/HTML/HTML5)

如果**type**属性的值是image，这个属性定义了按钮图片的宽度。