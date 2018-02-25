#### 柯里化



#### JSON

​	JSON格式：

1.  复合类型的值只能是数组或对象，不能是函数、正则表达式对象、日期对象。
2.  原始类型的值只有四种：字符串、数值（必须以十进制表示）、布尔值和`null`（不能使用`NaN`, `Infinity`, `-Infinity`和`undefined`）。
3.  字符串必须使用双引号表示，不能使用单引号。
4.  对象的键名必须放在双引号里面。
5.  数组或对象最后一个成员的后面，不能加逗号。
6.  `null`、空数组和空对象都是合法的 JSON 值。

```
["one", "two", "three"]

{ "one": 1, "two": 2, "three": 3 }

{"names": ["张三", "李四"] }

[ { "name": "张三"}, {"name": "李四"} ]
```



#### 浅拷贝

-   如果该元素是个对象引用 （不是实际的对象）， 会拷贝这个对象引用到新的数组里。两个对象引用都引用了同一个对象。如果被引用的对象发生改变，则新的和原来的数组中的这个元素也会发生改变。


-   对于字符串、数字及布尔值来说（不是 [`String`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String)、[`Number`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Number) 或者 [`Boolean`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Boolean) 对象），会拷贝这些值到新的数组里。在别的数组里修改这些字符串或数字或是布尔值，将不会影响另一个数组。

### IIFE

```js
(function(a) {
  console.log(a);  
})(123);

!function(a) {
  console.log(a); 
}(12345);

+function(a) {
  console.log(a);  
}(123456);

-function(a) {
  console.log(a);  
}(1234567);

var fn = function(a) {
  console.log(a); 
}(12345678);
```

#### 立即执行函数有什么用？

只有一个作用：创建一个独立的作用域。

这个作用域里面的变量，外面访问不到（即避免「变量污染」）。

### map/reduce

`map()` 方法返回一个**新数组**，其结果是该数组中的每个元素都调用一个提供的函数后返回的结果。

```
let numbers = [1, 5, 10, 15];
let doubles = numbers.map( x => x ** 2);

// doubles is now [1, 25, 100, 225]
// numbers is still [1, 5, 10, 15]
```



`reduce()` 方法对累加器和数组中的每个元素（从左到右）应用一个函数，将其减少为单个值。返回一个值，原数组不变。

```
var sum = [0, 1, 2, 3].reduce(function (a, b) {
  return a + b;
}, 0);
// sum is 6
```





### filter

filter也是一个常用的操作，它用于把`Array`的某些元素过滤掉，然后返回剩下的元素组成的**新数组**。

和`map()`类似，`Array`的`filter()`也接收一个函数。和`map()`不同的是，`filter()`把传入的函数依次作用于每个元素，然后根据返回值是`true`还是`false`决定保留还是丢弃该元素。

例如，在一个`Array`中，删掉偶数，只保留奇数，可以这么写：

```js
var arr = [1, 2, 4, 5, 6, 9, 10, 15];
var r = arr.filter(function (x) {
    return x % 2 !== 0;
});
console.log(r); // [1, 5, 9, 15]
```



### call/apply/bind





## 选择器

### querySelector

```javascript
const element = document.querySelector(selectors);
```

`selectors`是一个字符串，包含一个或是多个 [CSS 选择器](https://developer.mozilla.org/zh-CN/docs/Web/Guide/CSS/Getting_Started/Selectors) ，多个则以逗号分隔。

如果没有找到匹配元素，则返回 `null`，如果找到多个匹配元素，则返回**第一个**匹配到的元素。

如果要匹配的ID或选择器不符合 CSS 语法（比如不恰当地使用了冒号或者空格），你必须用反斜杠将这些字符转义，你必须将它**转义两次**（一次是为 JavaScript 字符串转义，另一次是为 `querySelector` 转义）：



### ParentNode.querySelectorAll()

返回一个 [`NodeList`](https://developer.mozilla.org/zh-CN/docs/Web/API/NodeList) 表示元素的列表，把**当前的元素**作为**根**与指定的选择器组相匹配。

### classList

**classList** 是一个只读属性,返回一个元素的类属性的实时 `DOMTokenList`集合。**element.classList** 本身是只读的，虽然你可以使用 **add() **和 **remove()** 方法修改它。

`add( String [, String] )`

添加指定的类值。如果这些类已经存在于元素的属性中，那么它们将被忽略。

`remove( String [,String] )`

删除指定的类值。

`item ( Number )`

按集合中的索引返回类值。

`toggle ( String [, force] )`

当只有一个参数时：切换 class value; 即如果类存在，则删除它并返回false，如果不存在，则添加它并返回true。

当存在第二个参数时：如果第二个参数的计算结果为true，则添加指定的类值，如果计算结果为false，则删除它

`contains( String )`

检查元素的类属性中是否存在指定的类值。

```js
// div是具有class =“foo bar”的<div>元素的对象引用
div.classList.remove("foo");
div.classList.add("anotherclass");

// 如果visible被设置则删除它，否则添加它
div.classList.toggle("visible");

// 添加/删除 visible，取决于测试条件，i小于10
div.classList.toggle("visible", i < 10);

alert(div.classList.contains("foo"));

//添加或删除多个类
div.classList.add("foo","bar");
div.classList.remove("foo", "bar");
```



### addEventListener

```js
el.addEventListener("click", modifyText, false);//调用无参函数，直接用函数名
el.addEventListener("click", function(){modifyText("four")}, false);//调用有参函数，则需要用匿名函数包起来，否则直接传带参函数名，则调用的是其函数的返回值。
```

### Element.matches()

如果元素被指定的选择器字符串选择，**Element.matches()**  方法返回true; 否则返回false。

```js
<ul id="birds">
  <li>Orange-winged parrot</li>
  <li class="endangered">Philippine eagle</li>
  <li>Great white pelican</li>
</ul>

<script type="text/javascript">
  var birds = document.getElementsByTagName('li');

  for (var i = 0; i < birds.length; i++) {
    if (birds[i].matches('.endangered')) {
      console.log('The ' + birds[i].textContent + ' is endangered!');
    }
  }
</script>
```

## Array（数组）

### Array.prototype.slice()

```
arr.slice(begin, end);//两个参数可选
```

`slice` 不修改原数组，只会返回一个浅复制了原数组中的元素的一个新数组。原数组的元素会按照下述规则拷贝：

-   如果该元素是个对象引用 （不是实际的对象），`slice` 会拷贝这个对象引用到新的数组里。两个对象引用都引用了同一个对象。如果被引用的对象发生改变，则新的和原来的数组中的这个元素也会发生改变。


-   对于字符串、数字及布尔值来说（不是 [`String`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String)、[`Number`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Number) 或者 [`Boolean`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Boolean) 对象），`slice` 会拷贝这些值到新的数组里。在别的数组里修改这些字符串或数字或是布尔值，将不会影响另一个数组。

### event.target 和 event.currentTarget

e.target指的是触发时间的元素，e.currentTarget是绑定事件的元素。





### Array.prototype.forEach()

forEach对于元素例如数值，字符等是按值传参，即浅复制，对于对象则是引用。所以通过forEach去替换数组中的数值是不可行的。