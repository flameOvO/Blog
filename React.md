## 生命周期

每一个组件都有几个你可以重写以让代码在处理环节的特定时期运行的“生命周期方法”。方法中带有前缀 **will** 的在特定环节之前被调用，而带有前缀 **did** 的方法则会在特定环节之后被调用。

#### 装配

这些方法会在组件实例被创建和插入DOM中时被调用：

-   [`constructor()`](https://doc.react-china.org/docs/react-component.html#constructor)
-   [`componentWillMount()`](https://doc.react-china.org/docs/react-component.html#componentwillmount)
-   [`render()`](https://doc.react-china.org/docs/react-component.html#render)
-   [`componentDidMount()`](https://doc.react-china.org/docs/react-component.html#componentdidmount)

#### 更新

属性或状态的改变会触发一次更新。当一个组件在被重渲时，这些方法将会被调用：

-   [`componentWillReceiveProps()`](https://doc.react-china.org/docs/react-component.html#componentwillreceiveprops)
-   [`shouldComponentUpdate()`](https://doc.react-china.org/docs/react-component.html#shouldcomponentupdate)
-   [`componentWillUpdate()`](https://doc.react-china.org/docs/react-component.html#componentwillupdate)
-   [`render()`](https://doc.react-china.org/docs/react-component.html#render)
-   [`componentDidUpdate()`](https://doc.react-china.org/docs/react-component.html#componentdidupdate)

#### 卸载

当一个组件被从DOM中移除时，该方法被调用：

-   [`componentWillUnmount()`](https://doc.react-china.org/docs/react-component.html#componentwillunmount)

![react生命周期](img/react生命周期.png)

React是非常灵活的，但它也有一个**严格**的规则：

**所有的React组件必须像纯函数那样使用它们的props**

无论是使用**函数或是类**来声明一个组件，它决不能修改它自己的props。

只有类有局部状态。



 JSX 回调函数中的 `this`，类的方法默认是不会[绑定](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_objects/Function/bind) `this` 的



*   状态：不能直接更新state，需要通过this.setState({···})更新。

状态更新可能是异步的
React 可以将多个setState() 调用合并成一个调用来提高性能

```js
this.setState((prevState, props) => ({
  counter: prevState.counter + props.increment
}));

this.setState(function(prevState, props) {
  return {
    counter: prevState.counter + props.increment
  };
});
//both is ok
```

通过 `bind` 方式向监听函数传参，在类组件中定义的监听函数，事件对象 `e` 要排在所传递参数的后面，



key会作为给React的提示，但不会传递给你的组件。如果您的组件中需要使用和`key`相同的值，请将其作为属性传递：

####提取组件的时机

还要记住如果条件变得过于复杂，可能就是提取组件的好时机了。

如果一个map()嵌套了太多层级，那可能就是你提取出组件的一个好时机。



在React应用中，对应任何可变数据理应只有一个单一“数据源”。通常，状态都是首先添加在需要渲染数据的组件中。此时，如果另一个组件也需要这些数据，你可以将数据提升至离它们最近的父组件中。



一些组件不能提前知道它们的子组件是什么。这对于 `Sidebar` 或 `Dialog` 这类通用容器尤其常见。

我们建议这些组件使用 `children` 属性将子元素直接传递到输出。

```javascript
function FancyBorder(props) {
  return (
    <div className={'FancyBorder FancyBorder-' + props.color}>
      {props.children}//<FrancyBordere>的innerHTML，相当于slot
    </div>
  );
}
```
虽然不太常见，但有时你可能需要在组件中有多个入口，这种情况下你可以使用自己约定的属性而不是 `children`：

```js
function SplitPane(props) {
  return (
    <div className="SplitPane">
      <div className="SplitPane-left">
        {props.left}
      </div>
      <div className="SplitPane-right">
        {props.right}
      </div>
    </div>
  );
}

function App() {
  return (
    <SplitPane
      left={
        <Contacts />
      }
      right={
        <Chat />
      } />
  );
}
```



`class` 属性需要写成 `className` ，`for` 属性需要写成 `htmlFor` ，这是因为 `class` 和 `for` 是 JavaScript 的保留字。

`this.props` 对象的属性与组件的属性一一对应，但是有一个例外，就是 `this.props.children` 属性。它表示组件的所有子节点



`key` 必须可转化为字符串。



React 执行 diff 算法时比较的是两个引用，而不是引用的对象。



React 支持给任意组件添加特殊属性。`ref` 属性接受一个回调函数，它在组件**被加载或卸载时**会立即执行





##Diff算法