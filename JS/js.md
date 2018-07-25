#### 实现对象的不可改变

四个方法，方法三，方法四是方法一与二的结合。

1. 结合 writable:false 和 configurable:false 就可以创建一个真正的常量属性(不可修改、 重定义或者删除): 

2. 禁止一个对象添加新属性并且保留已有属性，可以使用 Object.prevent Extensions(..): 

   ```js
   var myObject = { a:2
        };
        Object.preventExtensions( myObject );
        myObject.b = 3;
        myObject.b; // undefined
   ```

3. Object.seal(..) 会创建一个“密封”的对象，这个方法实际上会在一个现有对象上调用 Object.preventExtensions(..) 并把所有现有属性标记为 configurable:false。 

4. Object.freeze(..) 会创建一个冻结对象，这个方法实际上会在一个现有对象上调用 Object.seal(..) 并把所有“数据访问”属性标记为 writable:false，这样就无法修改它们的值。 

#### 属性描述符

对象里目前存在的属性描述符有两种主要形式：**数据描述符**和**存取描述符**。

**数据描述符**和**存取描述符**共有特性： `enumerable`(可枚举)和 `configurable`(可配置)。

* configurable决定属性是否可配置。只要属性是可配置的，就可以使用 defineProperty(..) /delete方法来==删改==属性描述符。且修改configurable是单向的，因为configurable为false时，无法修改属性描述符。

  <!--要注意有一个小小的例外:即便属性是 configurable:false，我们还是可以 把 writable 的状态由 true 改为 false，但是无法由 false 改为 true。--> 

* Enumerable  这个描述符控制的是属性是否会出现在对象的属性枚举中 。

**数据描述符**特有：

`writable` 决定是否可以修改属性的值。 

`value`该属性对应的值。**默认为 undefined**。

**存取描述符**特有：

`get`:一个给属性提供 getter 的方法，如果没有 getter 则为 `undefined`。当访问该属性时，该方法会被执行，方法执行时没有参数传入，但是会传入`this`对象

`set`:一个给属性提供 setter 的方法，如果没有 setter 则为 `undefined`。当属性值修改时，触发执行该方法。该方法将接受唯一参数，即该属性新的参数值。

#### this

this 是在运行时进行绑定的，并不是在编写时绑定，它的上下文取决于函数调用时的各种条件。

函数的不同使用场合，this有不同的值。总的来说，this就是函数运行时所在的环境对象。下面分四种情况。

##### 独立函数调用

把这条规则看作是无法应用其他规则时的默认规则。

```js
function foo() { 
	console.log( this.a );
}

var a = 2; 
foo(); // 2

```

##### 作为对象方法的调用(隐私绑定)

```js
//当函数引 用有上下文对象时，隐式绑定规则会把函数调用中的 this 绑定到这个上下文对象。
function foo() { 
    console.log( this.a );
}
var obj2 = { 
    a: 42,
	foo: foo 
};
var obj1 = { 
    a: 2,
	obj2: obj2 
};
obj1.obj2.foo(); // 42
//对象属性引用链中只有最后一层会影响调用位置
```

##### call/apply方法(显示绑定)

```js
function foo() { 
    console.log( this.a );
}
var obj = { 
    a:2
};
foo.call( obj ); // 2
```

##### new绑定

使用 new 来调用函数，或者说发生构造函数调用时，会自动执行下面的操作。 

1. 创建(或者说构造)一个全新的对象。
2. 这个新对象会被执行[[原型]]连接。
3. 这个新对象会绑定到函数调用的this。
4. 如果函数没有返回其他对象，那么new表达式中的函数调用会自动返回这个新对象。 

##### 优先级

new绑定 > call/apply方法(显示绑定) > 作为对象方法的调用(隐私绑定) > 函数调用（默认规则）

##### 判断this

如果要判断一个运行中函数的 this 绑定，就需要找到这个函数的直接调用位置。找到之后 

就可以顺序应用下面这四条规则来判断 this 的绑定对象。 

```js
//1. 函数是否在new中调用(new绑定)?如果是的话this绑定的是新创建的对象。
     var bar = new foo()
//2. 函数是否通过call、apply(显式绑定)或者硬绑定调用?如果是的话，this绑定的是指定的对象。
     var bar = foo.call(obj2)
//3. 函数是否在某个上下文对象中调用(隐式绑定)?如果是的话，this 绑定的是那个上下文对象。
     var bar = obj1.foo()
//4. 如果都不是的话，使用默认绑定。如果在严格模式下，就绑定到undefined，否则绑定到全局对象。
     var bar = foo()
     
//例外
//1. null 或者 undefined 作为 this 的绑定对象传入 call、apply 或者 bind，这些值在调用时会被忽略，实际应用的是默认绑定规则:
    function foo() { 
         console.log( this.a );
	}
	var a = 2;
	foo.call( null ); // 2
//2. 一个函数的“间接引用”，在这 种情况下，调用这个函数会应用默认绑定规则。
function foo() { 
    console.log( this.a );
}
var a = 2;
var o = { a: 3, foo: foo }; 
var p = { a: 4 };
o.foo(); // 3
(p.foo = o.foo)(); // 2
//3 箭头函数
```



#### 词法作用域和动态作用域

两者区别在于，遇到自由变量，词法作用域是去==函数定义时的环境==查询，动态作用域到==函数调用时的环境==中查。

```js
//词法作用域
function foo() { 
    console.log( a ); // 2
}
function bar() { 
    var a = 3;
	foo(); 
}
var a = 2; 
bar();
//动态作用域
function foo() {
	console.log( a ); // 3(不是 2 !)
}
function bar() { 
    var a = 3;
	foo(); 
}
var a = 2; 
bar();
```



#### 柯里化

**1. 参数复用**；**2. 提前返回； 3. 延迟计算/运行**。、

```js
var currying = function(fn) {//参数复用。
    // fn 指官员消化老婆的手段
    var args = [].slice.call(arguments, 1);
    // args 指的是那个合法老婆
    return function() {
        // 已经有的老婆和新搞定的老婆们合成一体，方便控制
        var newArgs = args.concat([].slice.call(arguments));
        // 这些老婆们用 fn 这个手段消化利用，完成韦小宝前辈的壮举并返回
        return fn.apply(null, newArgs);
    };
};

// 下为官员如何搞定7个老婆的测试
// 获得合法老婆
var getWife = currying(function() {
    var allWife = [].slice.call(arguments);
    // allwife 就是所有的老婆的，包括暗渡陈仓进来的老婆
    console.log(allWife.join(";"));
}, "合法老婆");

// 获得其他6个老婆
getWife("大老婆","小老婆","俏老婆","刁蛮老婆","乖老婆","送上门老婆");

// 换一批老婆
getWife("超越韦小宝的老婆");
```

```js
var curryWeight = function(fn) {//延迟计算
    var _fishWeight = [];
    return function() {
        if (arguments.length === 0) {
            return fn.apply(null, _fishWeight);
        } else {
            _fishWeight = _fishWeight.concat([].slice.call(arguments));
        }
    }
};
var fishWeight = 0;
var addWeight = curryWeight(function() {
    var i=0; len = arguments.length;
    for (i; i<len; i+=1) {
        fishWeight += arguments[i];
    }
});

addWeight(2.3);
addWeight(6.5);
addWeight(1.2);
addWeight(2.5);
addWeight();    //  这里才计算

console.log(fishWeight);    // 12.5
```



#### 浅拷贝

-   如果该元素是个对象引用 （不是实际的对象）， 会拷贝这个对象引用到新的数组里。两个对象引用都引用了同一个对象。如果被引用的对象发生改变，则新的和原来的数组中的这个元素也会发生改变。


-   对于字符串、数字及布尔值来说（不是 [`String`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String)、[`Number`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Number) 或者 [`Boolean`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Boolean) 对象），会拷贝这些值到新的数组里。在别的数组里修改这些字符串或数字或是布尔值，将不会影响另一个数组。

###事件

`event.stopPropagation`阻止捕获和冒泡阶段中当前事件的进一步传播

`event.cancelable`返回一个布尔值，表明该事件是否可以被取消默认行为。

`event.preventDefault();`如果事件可取消，则取消该事件，而不停止事件的进一步传播。

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

### event.target 和 event.currentTarget

e.target指的是触发时间的元素，e.currentTarget是绑定事件的元素。

### Element.matches()

如果元素被指定的选择器字符串选择，**Element.matches()**  方法返回true; 否则返回false。

```html
<ul id="birds">
  <li>Orange-winged parrot</li>
  <li class="endangered">Philippine eagle</li>
  <li>Great white pelican</li>
</ul>
```

```js

  var birds = document.getElementsByTagName('li');

  for (var i = 0; i < birds.length; i++) {
    if (birds[i].matches('.endangered')) {
      console.log('The ' + birds[i].textContent + ' is endangered!');
    }
  }
```

## Array（数组）

### Array.isArray()

`Array.isArray`方法返回一个布尔值，表示参数是否为数组。弥补`typeof`运算符

### push()，pop()

`push`方法用于在数组的末端添加一个或多个元素，并返回添加新元素后的数组长度。注意，该方法会改变原数组。

`pop`方法用于删除数组的最后一个元素，并返回该元素。注意，该方法会改变原数组。对空数组使用`pop`方法，不会报错，而是返回`undefined`。

### concat()

`concat`方法用于多个数组的合并。它将新数组的成员，添加到原数组成员的后部，然后返回一个新数组，原数组不变。如果数组成员包括对象，`concat`方法返回当前数组的一个浅拷贝。所谓“浅拷贝”，指的是新数组拷贝的是对象的引用。

### Array.prototype.slice()

```
arr.slice(begin, end);//两个参数可选
```

`slice` 不修改原数组，只会返回一个浅复制了原数组中的元素的一个新数组。原数组的元素会按照下述规则拷贝：

-   如果该元素是个对象引用 （不是实际的对象），`slice` 会拷贝这个对象引用到新的数组里。两个对象引用都引用了同一个对象。如果被引用的对象发生改变，则新的和原来的数组中的这个元素也会发生改变。


-   对于字符串、数字及布尔值来说（不是 [`String`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String)、[`Number`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Number) 或者 [`Boolean`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Boolean) 对象），`slice` 会拷贝这些值到新的数组里。在别的数组里修改这些字符串或数字或是布尔值，将不会影响另一个数组。



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



### Array.prototype.forEach()

forEach对于元素例如数值，字符等是按值传参，即浅复制，对于对象则是引用。所以通过forEach去替换数组中的数值是不可行的。`forEach`对数组的所有成员依次执行参数函数。但是，`forEach`方法不返回值，只用来操作数据。`forEach`方法也会跳过数组的空位。

### some()，every()

`some`方法是**只要一个**成员的返回值是`true`，则整个`some`方法的返回值就是`true`，否则返回`false`。

```js
var arr = [1, 2, 3, 4, 5];
arr.some(function (elem, index, arr) {
  return elem >= 3;
});
```

`every`方法是**所有**成员的返回值都是`true`，整个`every`方法才返回`true`，否则返回`false`。

注意，对于空数组，`some`方法返回`false`，`every`方法返回`true`，回调函数都不会执行。

## String

#### String.prototype.match(regexp)

如果字符串匹配到了表达式，会返回一个**数组**，数组的第一项是**进行匹配完整的字符串**，之后的项是用**圆括号**捕获的结果。如果没有匹配到，返回null

#### String.prototype.search(regexp)

执行正则表达式和 `String`对象之间的一个搜索匹配。如果匹配成功，则 search() 返回正则表达式在字符串中首次匹配项的索引。否则，返回 -1。

#### String.prototype.replace(regexp|substr,newSubStr)

该方法并不改变调用它的字符串本身，而只是返回一个新的替换后的字符串。

### String.prototype.split()

如果正则表达式带有括号，则括号匹配的部分也会作为数组成员返回。

```js
'aaa*a*'.split(/(a*)/)// [ '', 'aaa', '*', 'a', '*' ]

```

​	

### String.fromCharCode()

该方法的参数是一个或多个数值，代表 Unicode 码点，返回值是这些码点组成的字符串。

```js
String.fromCharCode(104, 101, 108, 108, 111)
// "hello"
```

### String.prototype.charAt()

```js
'abc'.charAt(1) // "b" 等同于下面
'abc'[1] // "b"
```

### String.prototype.charCodeAt()

String.fromCharCode()的逆操作

```js
'abc'.charCodeAt(1) // 98
```

### String.prototype.lastIndexOf()

`lastIndexOf`方法的用法跟`indexOf`方法一致，主要的区别是`lastIndexOf`从尾部开始匹配，`indexOf`则是从头部开始匹配。

### String.prototype.trim()

`trim`方法用于去除字符串**两端**的空格，返回一个**新字符串**，不改变原字符串。



## Object

### Object.assign

`Object.assign(target, ...sources)` 方法用于将所有可枚举属性的值从一个或多个源对象==浅复制==到目标对象。它将返回目标对象。

### Object.create

`Object.create(prototype)`方法创建一个新对象，使用现有的对象来提供新创建的对象的\__proto__。 

### Object.defineProperty()

` Object.defineProperty(obj, prop, descriptor) `来在一个对象上添加一个新属性或者修改一个已有属性(如果它是 configurable)并对特性进行设置。 

`Object.defineProperties(obj, props)`可以一个对象上对多个属性进行操作。

### Object.keys()

`Object.keys(obj)` 方法会返回一个由一个给定对象的==自身可枚举属性==组成的==**数组**==，数组中属性名的排列顺序和使用` for..in`循环遍历该对象时返回的顺序一致 。不会访问到原型链。

### Object.getOwnPropertyNames()

·`bject.getOwnPropertyNames(obj)`方法返回一个由指定对象的所有==自身属性的属性名==（包括不可枚举属性但不包括Symbol值作为名称的属性）组成的==数组==。`Object.keys()`的超集。

### Object.prototype.hasOwnProperty

`hasOwnProperty(prop)` 方法会返回一个**布尔值**，指示对象==**自身**==属性中是否具有指定的属性,不访问原型链

### Object.prototype.propertyIsEnumerable()

`propertyIsEnumerable(prop)` 方法返回一个布尔值，表示指定的属性是否可枚举。