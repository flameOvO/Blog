### JSON格式

​	JSON格式：

1.  复合类型的值只能是数组或对象，不能是函数、正则表达式对象、日期对象。
2.  原始类型的值只有四种：字符串、数值（必须以十进制表示）、布尔值和`null`（不能使用`NaN`, `Infinity`, `-Infinity`和`undefined`）。
3.  **字符串**必须使用**双引号**表示，不能使用单引号。
4.  对象的**键名**必须放在**双引号**里面。
5.  数组或对象最后一个成员的后面，不能加逗号。
6.  `null`、空数组和空对象都是合法的 JSON 值。(没有undefined)

```
["one", "two", "three"]

{ "one": 1, "two": 2, "three": 3 }

{"names": ["张三", "李四"] }

[ { "name": "张三"}, {"name": "李四"} ]
```

### JSON.stringify()

对于原始类型的字符串，转换结果会带双引号。

```js
JSON.stringify('foo') === "foo" // false
JSON.stringify('foo') === "\"foo\"" // true
```

如果**对象**的属性是`undefined`、函数或 XML 对象，该属性会被`JSON.stringify`过滤。

```js
var obj = {
  a: undefined,
  b: function () {}
};

JSON.stringify(obj) // "{}"
```

如果**数组**的成员是`undefined`、函数或 XML 对象，则这些值被转成`null`。

`JSON.stringify`方法会忽略**对象**的不可遍历属性。

**正则**对象会被转成空对象。

```js
var arr = [undefined, function () {}];
JSON.stringify(arr) // "[null,null]"
JSON.stringify(/foo/) // "{}"
```

####第二参数

`JSON.stringify`方法还可以接受一个**数组**，作为第二个参数，指定需要转成字符串的属性。

```js
JSON.stringify(['a', 'b'], ['0'])
// "["a","b"]"

JSON.stringify({0: 'a', 1: 'b'}, ['0'])
```

这个类似白名单的数组，只对**对象**的属性有效，对**数组**无效。



第二个参数还可以是一个函数，用来更改`JSON.stringify`的返回值。

```js
var o = {a: {b: 1}};

function f(key, value) {
  console.log("["+ key +"]:" + value);
  return value;
}

JSON.stringify(o, f)
// []:[object Object]
// [a]:[object Object]
// [b]:1
// '{"a":{"b":1}}'
```

上面代码中，对象`o`一共会被`f`函数处理三次，最后那行是`JSON.stringify`的输出。第一次键名为空，键值是整个对象`o`；第二次键名为`a`，键值是`{b: 1}`；第三次键名为`b`，键值为1。

递归处理中，每一次处理的对象，都是前一次返回的值。







#### 第三参数

`JSON.stringify`还可以接受第三个参数，用于增加返回的 JSON 字符串的可读性。如果是数字，表示每个属性前面添加的空格（最多不超过10个）；如果是字符串（不超过10个字符），则该字符串会添加在每行前面。

```
JSON.stringify({ p1: 1, p2: 2 }, null, 2);
/*
"{
  "p1": 1,
  "p2": 2
}"
*/

JSON.stringify({ p1:1, p2:2 }, null, '|-');
/*
"{
|-"p1": 1,
|-"p2": 2
}"
*/
```

如果参数对象有自定义的`toJSON`方法，那么`JSON.stringify`会使用这个方法的返回值作为参数，而忽略原对象的其他属性。

```js
var user = {
  firstName: '三',
  lastName: '张',

  get fullName(){
    return this.lastName + this.firstName;
  },

  toJSON: function () {
    return {
      name: this.lastName + this.firstName
    };
  }
};

JSON.stringify(user)
// "{"name":"张三"}"
```

### JSON.parse()

parse处理的对象都需要用单引号包裹。

```js
JSON.parse('{}') // {}
JSON.parse('true') // true
JSON.parse('"foo"') // "foo"
JSON.parse('[1, 5, "false"]') // [1, 5, "false"]
JSON.parse('null') // null

var o = JSON.parse('{"name": "张三"}');
o.name // 张三
```

第二参数与stringify相似。