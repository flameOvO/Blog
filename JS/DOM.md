##Node接口

```js
//不同节点的nodeType属性值和对应的常量如下
nodeType = {
    'Node.ELEMENT_NODE' : 1, //一个 元素 节点，例如 <p> 和 <div>。
    'Node.TEXT_NODE': 3, //Element 或者 Attr 中实际的  文字
    'Node.PROCESSING_INSTRUCTION_NODE': 7,//一个用于XML文档的 ProcessingInstruction ，例如 <?xml-stylesheet ... ?> 声明。
    'Node.COMMENT_NODE': 8,//一个注释节点。
        'Node.DOCUMENT_NODE': 9,//文档节点（document）
    'Node.DOCUMENT_TYPE_NODE': 10,//描述文档类型的 DocumentType 节点。例如 <!DOCTYPE html>  
    'Node.DOCUMENT_FRAGMENT_NODE': 11//一个 DocumentFragment 节点
}
```

```js
//不同节点的nodeName属性值如下。
nodeName = {
    文档节点（document）：#document
    注释节点（Comment）：#comment
    文本节点（text）：#text
    文档片断节点（DocumentFragment）：#document-fragment
	元素节点（element）：大写的标签名
	属性节点（attr）：属性的名称
}
```

#### Node.nodeValue

只有文本节点（text）和注释节点（comment）有文本值，因此这两类节点的`nodeValue`可以返回结果，其他类型的节点一律返回`null`。

```js
// HTML 代码如下
// <div id="d1">hello world</div>
var div = document.getElementById('d1');
div.nodeValue // null
div.firstChild.nodeValue // "hello world"
```

#### Node.textContent

`Node.textContent` 属性表示一个节点及其后代的文本内容。

- 如果 element 是 Document，DocumentType 或者 Notation 类型节点，则 `textContent` 返回 `null`。如果你要获取整个文档的文本以及CDATA数据，可以使用`document.documentElement.textContent`。
- 如果节点是个CDATA片段，注释，ProcessingInstruction节点或一个文本节点，`textContent` 返回节点内部的文本内容。
- 对于其他节点类型，`textContent` 将所有子节点的 `textContent` 合并后返回，除了注释、ProcessingInstruction节点。如果该节点没有子节点的话，返回一个空字符串。
- 在节点上设置 `textContent` 属性的话，会==删除它原来的所有子节点==，并替换为一个具有给定值的文本节点。

```Js
// 给定如下HTML:
//   <div id="divA">This is <span>some</span> text</div>

// 获得文本内容:
var text = document.getElementById("divA").textContent;
// |text| is set to "This is some text".

// 设置文本内容:
document.getElementById("divA").textContent = "This is some text";
// divA的HTML现在是这样的:
//   <div id="divA">This is some text</div>
```

#### Node.baseURI

该属性的值一般由当前网址的 URL（即`window.location`属性）决定，但是可以使用 HTML 的`<base>`标签，改变该属性的值。

#### Node.ownerDocument,Node.getRootNode()

`Node.ownerDocument` 只读属性会返回当前节点的顶层的 document 对象。

当前节点为document则返回null

`getRootNode`方法返回当前节点所在文档的根节点，与`ownerDocument`属性的作用相同。

#### Node.isConnected

如果该节点与 DOM 树连接则返回 `true` , 否则返回 `false` .判断当前节点是否在document之中。

```js
let test = document.createElement('p');
console.log(test.isConnected); // false
document.body.appendChild(test);
console.log(test.isConnected); // true
```



#### Node.nextSibling/previousSibling

Node.nextSibling返回其父节点的 childNodes 列表中紧跟在其后面的兄弟节点，如果指定的节点为最后一个节点，则返回 null。

previousSibling则返回当前节点前面最近的兄弟节点，若无，则返回null。

#### Node.parentNode/Node.parentElement

`parentNode`属性返回当前节点的父节点。

当父节点为element时，parentNode与parentElement返回一致。

当父节点不为element时，parentElement返回空

对于一个节点来说，它的父节点只可能是三种类型：元素节点（element）、文档节点（document）和文档片段节点（documentfragment）。

#### Node.firstChild/Node.lastChild

`firstChild`属性返回当前节点的第一个子**节点**，如果当前节点没有子节点，则返回`null`。

`lastChild`属性返回当前节点的最后一个子**节点**，如果当前节点没有子节点，则返回`null`。用法与`firstChild`属性相同。

```js
// HTML 代码如下
// <p id="p1"> 
//   <span>First span</span>
//  </p>
var p1 = document.getElementById('p1');
p1.firstChild.nodeName // "#text"
 //p元素与span元素之间有空白字符，这导致firstChild返回的是文本节点。
```

#### Node.childNodes

`childNodes`属性返回一个类似数组的对象（`NodeList`集合），成员包括当前节点的所有子节点。

文档节点（document）就有两个子节点：文档类型节点（docType）和 HTML 根元素节点（<html>）。

注意，除了元素节点，`childNodes`属性的返回值还包括文本节点和注释节点。

如果当前节点不包括任何子节点，则返回一个空的`NodeList`集合。由于`NodeList`对象是一个==动态集合==，一旦子节点发生变化，立刻会反映在返回结果之中。



#### Node.appendChild()

该方法的返回值就是插入文档的子节点。

`appendChild`方法接受一个节点对象作为参数，将其作为最后一个**子节点**，插入当前节点。

#### Node.insertBefore()

返回值是插入的新节点`newNode`。

`insertBefore`方法接受两个参数，第一个参数是所要插入的节点`newNode`，第二个参数是父节点`parentNode`内部的一个子节点`referenceNode`。`newNode`将插在`referenceNode`这个子节点的前面。

如果`insertBefore`方法的第二个参数为`null`，则新节点将插在当前节点内部的最后位置，即变成最后一个子节点。等同于`appendChild`

如果newNode已经在DOM树中，newNode首先会从原来的位置移除 

```js
/*<div id="parentElement">
  <p id="123">asd</p>
  <span id="childElement">foo bar</span>
  <p id="456">sdf</p>
</div>*/

var sp1 = document.getElementById("456");
var sp2 = document.getElementById("childElement");
var parentDiv = sp2.parentNode;
parentDiv.insertBefore(sp1, sp2);
//
asd
sdf
foo bar
```

#### Node.removeChild()

返回值是移除的子节点。

`removeChild`方法接受一个子节点作为参数，用于从当前节点移除该子节点。

#### Node.replaceChild()

返回值是替换走的那个节点`oldChild`。

`replaceChild`方法用于将一个新的节点，替换当前节点的某一个子节点。

上面代码中，`replaceChild`方法接受两个参数，第一个参数`newChild`是用来替换的新节点，第二个参数`oldChild`是将要替换走的子节点。

#### Node.hasChildNodes()

`hasChildNodes`方法返回一个布尔值，表示当前节点是否有**子节点**

#### Node.contains()

`contains`方法返回一个布尔值，表示参数节点是否满足以下三个条件之一。

- 参数节点为当前节点。
- 参数节点为当前节点的子节点。
- 参数节点为当前节点的后代节点。

#### Node.cloneNode()

`cloneNode`方法用于克隆一个节点。它接受一个布尔值作为参数，表示是否同时克隆子节点。它的返回值是一个克隆出来的新节点。

#### Node.compareDocumentPosition()

`compareDocumentPosition`方法的用法，与`contains`方法完全一致，返回一个七个比特位的二进制值，表示参数节点与当前节点的关系。

#### Node.isEqualNode()，Node.isSameNode()

`isEqualNode`方法返回一个布尔值，用于检查两个节点是否相等。所谓相等的节点，指的是两个节点的类型相同、属性相同、子节点相同。

#### Node.normalize()

`normailize`方法用于清理当前节点内部的所有文本节点（text）。它会去除空的文本节点，并且将毗邻的文本节点合并成一个，也就是说不存在空的文本节点，以及毗邻的文本节点

```js
var wrapper = document.createElement("div");

wrapper.appendChild(document.createTextNode("Part 1 "));
wrapper.appendChild(document.createTextNode("Part 2 "));

// 这时(规范化之前),wrapper.childNodes.length === 2
// wrapper.childNodes[0].textContent === "Part 1 "
// wrapper.childNodes[1].textContent === "Part 2 "

wrapper.normalize();
// 现在(规范化之后), wrapper.childNodes.length === 1
// wrapper.childNodes[0].textContent === "Part 1 Part 2"
```

## NodeList接口

#### NodeList.keys()，NodeList.values()，NodeList.entries()

`keys()`返回键名的遍历器，`values()`返回键值的遍历器，`entries()`返回的遍历器同时包含键名和键值的信息。

#### HTMLCollection.namedItem()

`namedItem`方法的参数是一个字符串，表示`id`属性或`name`属性的值，返回对应的元素节点。如果没有对应的节点，则返回`null`。

## ParentNode,childNode

#### ParentNode.children

ParentNode.children 是一个只读属性，当前节点的所有==元素==子节点。 一个Node的子elements ，是一个==动态==更新的 HTMLCollection。

#### ParentNode.firstElementChild||ParentNode.lastElementChild

返回当前节点的第一个元素子节点。/最后一个元素子节点

#### ParentNode.childElementCount

`childElementCount`属性返回一个整数，表示当前节点的所有元素子节点的数目。如果不包含任何元素子节点，则返回`0`。

#### ParentNode.append()，ParentNode.prepend()

`append`方法为当前节点追加一个==或多个子节点==，位置是最后一个元素子节点的后面。

该方法不仅可以添加元素子节点，还可以添加文本子节点。

`prepend`方法为当前节点追加一个或多个子节点，位置是第一个元素子节点的前面

#### ChildNode.remove()

`remove`方法用于从父节点移除当前节点。

#### ChildNode.before()，ChildNode.after()

`before`方法用于在当前节点的前面，插入一个或多个同级节点。两者拥有相同的父节点。

注意，该方法不仅可以插入元素节点，还可以插入文本节点。

#### ChildNode.replaceWith()

`replaceWith`方法使用参数节点，替换当前节点。参数可以是元素节点，也可以是文本节点。

##Document节点

`document`对象继承了`EventTarget`接口、`Node`接口、`ParentNode`接口。这意味着，这些接口的方法都可以在`document`对象上调用。除此之外，`document`对象还有很多自己的属性和方法。

#### document.defaultView

`document.defaultView`属性返回`document`对象所属的`window`对象。

#### document.doctype

`document`对象一般有两个子节点。

第一个子节点是`document.doctype`，指向`<DOCTYPE>`节点

`document.firstChild`通常就返回这个节点(如果这个节点存在）。

#### document.documentElement

`document.documentElement`属性返回当前文档的根节点（root）。一般是`<html>`节点。

#### document.body，document.head

`document.body`属性指向`<body>`节点，`document.head`属性指向`<head>`节点。这两个属性总是存在的，如果网页源码里面省略了`<head>`或`<body>`，浏览器会自动创建。



#### document.scrollingElement

`document.scrollingElement`属性返回文档的滚动元素。也就是说，当文档整体滚动时，到底是哪个元素在滚动。

#### document.activeElement

`document.activeElement`属性返回获得当前焦点（focus）的 DOM 元素。

通常，这个属性返回的是`<input>`、`<textarea>`、`<select>`等表单元素

如果当前没有焦点元素，返回`<body>`元素或`null`。

#### document.fullscreenElement

`document.fullscreenElement`属性返回当前以全屏状态展示的 DOM 元素。如果不是全屏状态，该属性返回`null`。(用来判断<video>有没有处在全屏)



#### document.links

`document.links`属性返回当前文档所有设定了`href`属性的`<a>`及`<area>`==**节点**==。

#### document.forms

`document.forms`属性返回所有`<form>`表单节点。

 除了使用位置序号，`id`属性和`name`属性也可以用来引用表单。

```js
/* HTML 代码如下
  <form name="foo" id="bar"></form>
*/
document.forms[0] === document.forms.foo // true
document.forms.bar === document.forms.foo // true
```

#### document.images

`document.images`属性返回页面所有`<img>`图片节点。

#### document.embeds，document.plugins

`document.embeds`属性和`document.plugins`属性，都返回所有`<embed>`节点。

#### document.scripts

`document.scripts`属性返回所有`<script>`节点。

#### document.styleSheets

`document.styleSheets`属性返回文档内嵌或引入的样式表集合

#### document.documentURI，document.URL

`document.documentURI`属性和`document.URL`属性都返回一个字符串，表示当前文档的网址。

不同之处是它们继承自不同的接口，`documentURI`继承自`Document`接口，可用于所有文档；

`URL`继承自`HTMLDocument`接口，只能用于 HTML 文档。

#### document.domain

`document.domain`属性返回当前文档的域名，不包含协议和接口。

#### document.location

`Location`对象是浏览器提供的原生对象，提供 URL 相关的信息和操作方法。通过`window.location`和`document.location`属性，可以拿到这个对象。

#### document.lastModified

表示当前文档最后修改的时间。不同浏览器的返回值，日期格式是不一样的。

#### document.referrer

`document.referrer`属性返回一个字符串，表示当前文档的访问者来自哪里。

如果无法获取来源，或者用户直接键入网址而不是从其他网页点击进入，`document.referrer`返回一个空字符串

`document.referrer`的值，总是与 HTTP 头信息的`Referer`字段保持一致。

#### document.hidden

`document.hidden`属性返回一个布尔值，表示当前页面是否可见。如果窗口最小化、浏览器切换了 Tab，都会导致导致页面不可见，使得`document.hidden`返回`true`。

#### document.visibilityState

`document.visibilityState`返回文档的可见状态。

#### document.readyState

`document.readyState`属性返回当前文档的状态，共有三种可能的值。

- `loading`：加载 HTML 代码阶段（尚未完成解析）
- `interactive`：加载外部资源阶段
- `complete`：加载完成

1. 浏览器开始解析 HTML 文档，`document.readyState`属性等于`loading`。
2. 浏览器遇到 HTML 文档中的`<script>`元素，并且没有`async`或`defer`属性，就暂停解析，开始执行脚本，这时`document.readyState`属性还是等于`loading`。
3. HTML 文档解析完成，`document.readyState`属性变成`interactive`。
4. 浏览器等待图片、样式表、字体文件等外部资源加载完成，一旦全部加载完成，`document.readyState`属性变成`complete`。

每次状态变化都会触发一个`readystatechange`事件。



#### document.implementation

`document.implementation`属性返回一个`DOMImplementation`对象。该对象有三个方法，主要用于创建独立于当前文档的新的 Document 对象。

- `DOMImplementation.createDocument()`：创建一个 XML 文档。
- `DOMImplementation.createHTMLDocument()`：创建一个 HTML 文档。
- `DOMImplementation.createDocumentType()`：创建一个 DocumentType 对象。

#### document.querySelector()，document.querySelectorAll()

`document.querySelector`方法接受一个 CSS 选择器作为参数，返回匹配该选择器的元素节点。如果有多个节点满足匹配条件，则返回第一个匹配的节点。如果没有发现匹配的节点，则返回`null`。

`document.querySelectorAll`方法与`querySelector`用法类似，区别是返回一个`NodeList`对象，包含所有匹配给定选择器的节点。

这两个方法的参数，可以是逗号分隔的多个 CSS 选择器，返回匹配其中一个选择器的元素节点，这与 CSS 选择器的规则是一致的。

它们不支持 CSS 伪元素的选择器和伪类的选择器

```js
var matches = document.querySelectorAll('div.note, div.alert');
```

#### document.getElementsByTagName()

#### document.getElementsByClassName()

参数可以是多个`class`，它们之间使用空格分隔。

<!--上面的三种方法可以在元素节点中使用-->

<!--下面的两种方法只能在document节点中使用-->

#### document.getElementsByName()

`document.getElementsByName`方法用于选择拥有`name`属性的 HTML 元素（比如`<form>` `<input>`

#### document.getElementById()

`document.getElementById()`比`document.querySelector()`效率高得多。



#### document.createElement()

`document.createElement`方法用来生成元素节点，并返回该节点。

#### document.createTextNode()

`document.createTextNode`方法用来生成文本节点（`Text`实例），并返回该节点。它的**参数**是文本节点的内容。

#### document.createAttribute()

`document.createAttribute`方法生成一个新的属性节点（`Attr`实例），并返回它。

####  document.createComment()

`document.createComment`方法生成一个新的注释节点，并返回该节点。

#### document.createDocumentFragment()

`document.createDocumentFragment`方法生成一个空的文档片段对象（`DocumentFragment`实例）。

`DocumentFragment`不属于当前文档，对它的任何改动，都不会引发网页的重新渲染，比直接修改当前文档的 DOM 有更好的性能表现



#### document.hasFocus()

 `document.hasFocus`方法返回一个布尔值，表示当前文档之中是否有元素被激活或获得焦点

#### document.adoptNode()，document.importNode()

`document.adoptNode`方法从其他的document文档中获取一个节点。 该节点以及它的子树上的所有节点都会从原文档==**删除**== (如果有这个节点的话), 并且它的`ownerDocument` 属性会变成当前的document文档,而`parentNode`属性是`null`。。 之后你可以把这个节点插入到当前文档中。

`document.importNode`方法则是从原来所在的文档或`DocumentFragment`里面，拷贝某个节点及其子节点，让它们归属当前`document`对象。

==注意==，`document.importNode/document.adoptNode`方法只是改变了节点的归属，并没有将这个节点插入新的文档树。所以，还要再用`appendChild`方法或`insertBefore`方法，将新节点插入当前文档树。

#### document.createNodeIterator()

`document.createNodeIterator`方法返回一个子节点遍历器。

`document.createNodeIterator`方法第一个参数为所要遍历的根节点，第二个参数为所要遍历的节点类型，这里指定为元素节点（`NodeFilter.SHOW_ELEMENT`）。几种主要的节点类型写法如下。

注意，遍历器返回的第一个节点，总是根节点。

该实例的`nextNode()`方法和`previousNode()`方法，可以用来遍历所有子节点。





```Js
var nodeIterator = document.createNodeIterator(document.body);
var pars = [];
var currentNode;

while (currentNode = nodeIterator.nextNode()) {
  pars.push(currentNode);
}
```

#### document.createTreeWalker()

`document.createTreeWalker`方法返回一个 DOM 的子树遍历器。它与`document.createNodeIterator`方法基本是类似的，区别在于它返回的是`TreeWalker`实例，后者返回的是`NodeIterator`实例。



### 元素节点Element

#### 节点属性

```js
Element = {
    id:'元素的id属性',
    title： '元素的title属性',
    tagName: '元素的大写标签名',
 	accessKey: '分配给当前元素的快捷键',
    draggable: '表示当前元素是否可拖动',
    lang: '当前元素的语言设置',
    tabIndex: '表示当前元素在 Tab 键遍历时的顺序'//通常为-1，负值不遍历
    //这个属性并不能用来判断当前元素的实际可见性，css优于这个
    hidden: '表示当前元素的hidden属性，用来控制当前元素是否可见。',
    
    //元素本身的高度/宽度和padding，滚动条不含在内（可见部分）
    clientHeight: '元素节点的 CSS 高度',
    clientWidth: '元素节点的 CSS 宽度',
	//如果是小数，会四舍五入
    clientLeft: '元素节点的左border',
    clientRight: '元素节点的右border',
    //包括溢出容器、当前不可见的部分,padding，但是不包括border、margin以及水平滚动条
    scrollHeight: '当前元素的总高度',
    scrollWidth: '当前元素的总宽度',
    
    scrollLeft: '当前元素的水平滚动条向右侧滚动的像素数量',
    scrollTop: '当前元素的垂直滚动条向下滚动的像素数量',
 //包括元素本身的高/宽度、padding 和 border，以及水平滚动条的高度,不含margin
    offsetHeight：'元素的 CSS 垂直高度',
    offsetWidth: '元素的 CSS 水平宽度',
    
}
```

#### Element.attributes

`Element.attributes`属性返回一个类似数组的对象，成员是当前元素节点的所有属性节点

#### Element.className，Element.classList

`className`属性用来读写当前元素节点的`class`属性。它的值是一个字符串，每个`class`之间用空格分割。

`classList`属性返回一个类似数组的对象，当前元素节点的每个`class`就是这个对象的一个成员

#### Element.dataset

`Element.dataset`属性返回一个对象，可以从这个对象读写`data-`属性。

- 开头的`data-`会省略。
- 如果连词线后面跟了一个英文字母，那么连词线会取消，该字母变成大写。
- 其他字符不变。

```js
// <article
//   id="foo"
//   data-columns="3"
//   data-index-number="12314"
//   data-parent="cars">
//   ...
// </article>
var article = document.getElementById('foo');
foo.dataset.columns // "3"
foo.dataset.indexNumber // "12314"
foo.dataset.parent // "cars"

```

删除一个`data-*`属性，可以直接使用`delete`命令。

```js
delete document.getElementById('myDiv').dataset.foo;
```

#### Element.innerHTML/outerHTML

`Element.outerHTML`属性返回一个字符串，表示当前元素节点的所有 HTML 代码，包括该元素本身和所有子元素。

注意，如果一个节点没有父节点，设置`outerHTML`属性会报错。

`Element.innerHTML`只包含所有子元素

#### Element.offsetParent

`Element.offsetParent`属性返回最靠近当前元素的、并且 CSS 的`position`属性不等于`static`的上层元素。

如果当前元素是不可见的（`display`属性为`none`），或者位置是固定的（`position`属性为`fixed`），则`offsetParent`属性返回`null`。

 如果某个元素的所有上层节点的`position`属性都是`static`，则`Element.offsetParent`属性指向`<body>`元素。

#### Element.offsetLeft，Element.offsetTop

`Element.offsetLeft`返回当前元素左上角相对于`Element.offsetParent`节点的水平位移，`Element.offsetTop`返回垂直位移，单位为像素。

通常，这两个值是指相对于父节点的位移。



#### Element.firstElementChild，Element.lastElementChild

`Element.firstElementChild`属性返回当前元素的第一个元素子节点，`Element.lastElementChild`返回最后一个元素子节点。

#### Element.nextElementSibling，Element.previousElementSibling

`Element.nextElementSibling`属性返回当前元素节点的后一个同级元素节点，如果没有则返回`null`。

#### Element.closest()

 `Element.closest`方法接受一个 CSS 选择器作为参数，返回匹配该选择器的、最接近当前节点的一个祖先节点（**包括当前节点本身**）。如果没有任何节点匹配 CSS 选择器，则返回`null`。

```js
// HTML 代码如下
// <article>
//   <div id="div-01">Here is div-01
//     <div id="div-02">Here is div-02
//       <div id="div-03">Here is div-03</div>
//     </div>
//   </div>
// </article>

var div03 = document.getElementById('div-03');

// div-03 最近的祖先节点
div03.closest("#div-02") // div-02
div03.closest("div div") // div-03
div03.closest("article > div") //div-01
div03.closest(":not(div)") // article
```

#### Element.matches()

`Element.matches`方法返回一个布尔值，表示当前元素是否匹配给定的 CSS 选择器。参数是css选择器。

#### Element.scrollIntoView() 

`Element.scrollIntoView`方法滚动当前元素，进入浏览器的可见区域，类似于设置`window.location.hash`的效果。

该方法可以接受一个布尔值作为参数。如果为`true`，表示元素的顶部与==当前区域的可见部分==的顶部对齐（前提是当前区域可滚动）；如果为`false`，表示元素的底部与==当前区域的可见部分==的尾部对齐（前提是当前区域可滚动）。如果没有提供该参数，默认为`true`。

#### Element.getBoundingClientRect()

`Element.getBoundingClientRect`方法返回一个对象，提供当前元素节点的大小、位置等信息，基本上就是 CSS 盒状模型的所有信息。

```Js
DOMRect = {
    x：'元素左上角相对于视口的横坐标',
	y：'元素左上角相对于视口的纵坐标',
	height：'元素高度',
	width：'元素宽度',
	left：'元素左上角相对于视口的横坐标，与x属性相等',
	right：'元素右边界相对于视口的横坐标（等于x + width）',
	top：'元素顶部相对于视口的纵坐标，与y属性相等',
	bottom：'元素底部相对于视口的纵坐标（等于y + height）'
}
```



#### Element.getClientRects()

它和`Element.getBoundingClientRect()`方法的主要区别，

对于行内元素（比如`<span>`、`<a>`、`<em>`），该方法返回的对象有多少个成员，取决于该元素在页面上占据多少行。

它对于行内元素总是返回一个矩形。



#### Element.insertAdjacentElement()/insertAdjacentHTML()/insertAdjacentText()

方法接受两个参数，第一个是一个表示指定位置的字符串，表示插入的位置

- `beforebegin`：当前元素之前
- `afterbegin`：当前元素内部的第一个子节点前面
- `beforeend`：当前元素内部的最后一个子节点后面
- `afterend`：当前元素之后

`beforebegin`和`afterend`这两个值，只在当前节点有父节点时才会生效。

第二个参数是将要插入的节点/HTML/文本.

如果插入的节点是一个文档里现有的节点/HTML/文本，它会从原有位置删除，放置到新的位置



#### Element.focus()，Element.blur()，Element.click()

`Element.focus`方法用于将当前页面的焦点，转移到指定元素上。

`Element.blur`方法用于将焦点从当前元素移除。

`Element.click`方法用于在当前元素上模拟一次鼠标点击，相当于触发了`click`事件。



### 属性操作

#### Element.getAttribute()

`Element.getAttribute`方法返回当前元素节点的指定属性。如果指定属性不存在，则返回`null`。

```Js
// HTML 代码为
// <div id="div1" align="left">
var div = document.getElementById('div1');
div.getAttribute('align') // "left"		
```

#### Element.getAttributeNames()

`Element.getAttributeNames()`返回一个数组，成员是当前元素的所有属性的名字。

```js
  /*<div id="d" id="123" href="#" style="border-left:20px solid">12
    <p>Content</p>
    <p>Further Elaborated</p>
</div>*/
const d = document.getElementById("d");
console.log(d.getAttributeNames())
//["id", "href", "style"]
```

#### Element.setAttribute()

`Element.setAttribute`方法用于为当前元素节点新增属性。如果同名属性已存在，则相当于编辑已存在的属性。

该方法没有返回值。

```js
// HTML 代码为
// <button>Hello World</button>
var b = document.querySelector('button');
b.setAttribute('name', 'myButton');
//属性值总是字符串，其他类型的值会自动转成字符串
```

#### Element.hasAttribute()

`Element.hasAttribute`方法返回一个布尔值，表示当前元素节点是否包含指定属性。

#### Element.hasAttributes()

`Element.hasAttributes`方法返回一个布尔值，表示当前元素是否有属性，如果没有任何属性，就返回`false`，否则返回`true`。

#### Element.removeAttribute()

`Element.removeAttribute`方法移除指定属性。该方法没有返回值。

### 文本节点Text

#### wholeText

`wholeText`属性将当前文本节点与毗邻的文本节点，作为一个整体返回。大多数情况下，`wholeText`属性的返回值，与`data`属性和`textContent`属性相同。

#### nextElementSibling，previousElementSibling

文本节点的这两个属性，返回紧跟在当前文本节点前后的那个同级元素节点。如果取不到元素节点，则返回`null`。



#### appendData()，deleteData()，insertData()，replaceData()，subStringData(),remove(),splitText()

以下5个方法都是编辑`Text`节点文本内容的方法。

- `appendData()`：在`Text`节点尾部追加字符串。
- `deleteData()`：删除`Text`节点内部的子字符串，第一个参数为子字符串开始位置，第二个参数为子字符串长度。
- `insertData()`：在`Text`节点插入字符串，第一个参数为插入位置，第二个参数为插入的子字符串。
- `replaceData()`：用于替换文本，第一个参数为替换开始位置，第二个参数为需要被替换掉的长度，第三个参数为新加入的字符串。
- `subStringData()`：用于获取子字符串，第一个参数为子字符串在`Text`节点中的开始位置，第二个参数为子字符串长度。
- `remove`方法用于移除当前`Text`节点。
- `splitText()`方法将`Text`节点一分为二，变成两个毗邻的`Text`节点。它的参数就是分割位置（从零开始），该方法返回==分割位置后方==的字符串m,而原`Text`节点变成只包含分割位置前方的字符串。

```js
// HTML 代码为
// <p>Hello World</p>
var pElementText = document.querySelector('p').firstChild;

pElementText.appendData('!');
// 页面显示 Hello World!
pElementText.deleteData(7, 5);
// 页面显示 Hello W
pElementText.insertData(7, 'Hello ');
// 页面显示 Hello WHello
pElementText.replaceData(7, 5, 'World');
// 页面显示 Hello WWorld
pElementText.substringData(7, 10); 
// 页面显示不变，返回"World "
pElementText.remove();
// 现在 HTML 代码为
// <p></p>
// html 代码为 <p id="p">foobar</p>
var newText = pElementText.splitText(3);
newText // "bar"
textnode // "foo"
//父元素节点的normalize方法可以将毗邻的两个Text节点合并。
```

### DocumentFragment 节点

`DocumentFragment`节点对象没有自己的属性和方法，全部继承自`Node`节点和`ParentNode`接口。也就是说，`DocumentFragment`节点比`Node`节点多出以下四个属性。

- `children`：返回一个动态的`HTMLCollection`集合对象，包括当前`DocumentFragment`对象的所有子元素节点。
- `firstElementChild`：返回当前`DocumentFragment`对象的第一个子元素节点，如果没有则返回`null`。
- `lastElementChild`：返回当前`DocumentFragment`对象的最后一个子元素节点，如果没有则返回`null`。
- `childElementCount`：返回当前`DocumentFragment`对象的所有子元素数量。



## CSS操作

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

### CSSStyleDeclaration接口

每一条 CSS 规则的样式声明部分（大括号内部的部分），都是CSSStyleDeclaration对象的属性。不过，连词号需要变成骆驼拼写法。

- `cssText`：当前规则的所有样式声明文本。该属性可读写，即可用来设置当前规则。
- `length`：当前规则包含多少条声明。
- `parentRule`：包含当前规则的那条规则，同 CSSRule 接口的`parentRule`属性。

#### style对象

style对象是一个CSSStyleDeclaration的实例。

**(是DOM上的style属性，为行内样式,并不是该元素的全部样式）**

通过样式表设置的样式，或者从父元素继承的样式，无法通过这个属性得到。

操作 CSS 样式最简单的方法，就是使用网页元素节点的`getAttribute`方法、`setAttribute`方法和`removeAttribute`方法，直接读写或删除网页元素的`style`属性。

每个DOM节点对象都有个style属性，其值是个对象，该属性能直接操作来读写CSS样式。

这个对象所包含的属性与CSS规则一一对应，但是名字需要改写，比如`background-color`写成`backgroundColor`。

如果CSS属性名是JavaScript保留字，则规则名之前需要加上字符串`css`，比如`float`写成`cssFloat`。

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

#### 实例属性

##### cssText属性

`cssText`可以读写或删除**整个**样式。`cssText`的属性值可以**不用**改写CSS属性名。

删除一个元素的所有==行内样式==，最简便的方法就是设置`cssText`为空字符串。

```js
var divStyle = document.querySelector('div').style;

divStyle.cssText = 'background-color: red;'
  + 'border: 1px solid black;'
  + 'height: 100px;'
  + 'width: 100px;';
```

##### setProperty()，getPropertyValue()，removeProperty()

- `setProperty(propertyName,value)`：在CSS中添加某个CSS属性。
- `getPropertyValue(propertyName)`：读取某个CSS属性。
- `removeProperty(propertyName)`：删除某个CSS属性。

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

### window.matchMedia()

`window.matchMedia`方法接受一个`mediaQuery`语句的字符串作为参数，返回一个[`MediaQueryList`](https://developer.mozilla.org/en-US/docs/DOM/MediaQueryList)对象。该对象有以下两个属性。

- `media`：返回所查询的`mediaQuery`语句字符串。
- `matches`：返回一个布尔值，表示当前环境是否匹配查询语句。

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

### CSS事件

CSS的过渡效果（transition）结束后，触发`transitionEnd`事件。

实际使用`transitionend`事件时，可能需要添加浏览器前缀。

### animationstart事件，animationend事件，animationiteration事件

- animationstart事件：动画开始时触发。
- animationend事件：动画结束时触发。
- animationiteration事件：开始新一轮动画循环时触发。如果animation-iteration-count属性等于1，该事件不触发，即只播放一轮的CSS动画，不会触发animationiteration事件。

## 动画暂停/播放

animation-play-state属性可以控制动画的状态（暂停/播放），该属性需求加上浏览器前缀。

```
element.style.webkitAnimationPlayState = "paused";
element.style.webkitAnimationPlayState = "running";
```



