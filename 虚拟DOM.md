Virtual DOM：通过JavaScript构造DOM的信息与结构，当页面变更，比较变更前后JavaScript构造的DOM的差异，操作真正的DOM

DOM 树上的结构、属性信息我们都可以很容易地用 JavaScript 对象表示出来：

```js
var element = {
  tagName: 'ul', // 节点标签名
  props: { // DOM的属性，用一个对象存储键值对
    id: 'list'
  },
  children: [ // 该节点的子节点
    {tagName: 'li', props: {class: 'item'}, children: ["Item 1"]},
    {tagName: 'li', props: {class: 'item'}, children: ["Item 2"]},
    {tagName: 'li', props: {class: 'item'}, children: ["Item 3"]},
  ]
}

```

等同于

```html
<ul id='list'>
  <li class='item'>Item 1</li>
  <li class='item'>Item 2</li>
  <li class='item'>Item 3</li>
</ul>
```

所以DOM可以用JavaScript来表示，就可以用JavaScript来表示DOM，

用 JavaScript 对象表示 DOM 信息和结构，当状态变更的时候，只是重新渲染这个 表示DOM的JavaScript 的对象结构，真正的页面其实并不会发生改变。

但是可以用新渲染的对象树去和旧的树进行对比，记录这两棵树差异。记录下来的不同就是我们需要对页面真正的 DOM 操作，然后把它们应用在真正的 DOM 树上，页面就变更了。

[如何理解虚拟DOM?]: https://www.zhihu.com/question/29504639/answer/73607810

