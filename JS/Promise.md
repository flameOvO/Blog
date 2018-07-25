

## Promise对象

`Promise`，简单说就是一个容器，里面保存着某个未来才会结束的事件（通常是一个==异步操作==）的结果。

`Promise`对象代表一个异步操作，有三种状态：

* `pending`（进行中）
* `fulfilled`（已成功）
* `rejected`（已失败）



`Promise`对象的状态改变，只有两种可能：从`pending`变为`fulfilled`和从`pending`变为`rejected`。==一旦状态改变，就不会再变，任何时候都可以得到这个结果==。

ES6 规定，`Promise`对象是一个构造函数，用来生成`Promise`实例。

``````js
new Promise(异步操作)
``````

`Promise`构造函数接受==**一个函数**==(一般是个异步操作)作为参数，该函数的两个参数分别是`resolve`和`reject`。

它们是两个函数，由JavaScript引擎提供，作用是变更`Promise`的状态，并将值传递给回调函数。

`resolve`或`reject`并不会终结 `Promise` 的参数函数的执行。改变 `Promise`的状态是在本轮事件循环的末尾执行，总是晚于本轮循环的同步任务。

```js
const promise = new Promise(function(resolve, reject) {
  // ... some code

  if (/* 异步操作成功 */){
    resolve(任何想传给回调函数作为参数的值 || 可为空); //TODO：不一定要有参数，如果想给回调函数传参，添加想给回调函数的参数。
  } else {
    reject(任何想传给回调函数作为参数的值 || 可为空);// reject的参数一般是Error对象的实例
  }
  
  // ... some code //将继续执行
  
});
```



> Promise 新建后就会立即执行。

```js
let promise = new Promise(function(resolve, reject) {

  console.log('Promise');

  resolve();

});

promise.then(function() {

  console.log('resolved.');

});

console.log('Hi!');

// Promise

// Hi!

// resolved

//Promise 新建后立即执行，所以首先输出的是Promise。然后，then方法指定的回调函数，将在当前脚本所有同步任务执行完才会执行

```

## Promise.resolve()

将现有对象转为 Promise 对象，`Promise.resolve`方法就起到这个作用。

```js
Promise.resolve('foo')
// 等价于
new Promise(resolve => resolve('foo'))
```

1. **参数是一个 Promise 实例**

   如果参数是 Promise 实例，那么`Promise.resolve`将不做任何修改、原封不动地返回这个实例。

   ```js
   const p1 = new Promise(function (resolve, reject) {
     setTimeout(() => reject(new Error('fail')), 3000)
   })
   
   const p2 = new Promise(function (resolve, reject) {
     setTimeout(() => resolve(p1), 1000) //原封不动地返回P1。此时P2就是P，P2===P1
   })
   
   p2
     .then(result => console.log(result))
     .catch(error => console.log(error))
   // Error: fail
   //p1是一个 Promise，3 秒之后变为rejected。
   
   //p2的状态在 1 秒之后改变，resolve方法返回的是p1。由于p2返回的是另一个 Promise，导致p2自己的状态无效了，由p1的状态决定p2的状态。
   
   //所以，后面的then语句都变成针对p1。又过了 2 秒，p1变为rejected，导致触发catch方法指定的回调函数。
   
   ```

   

2. **参数不是具有then方法的对象，或根本就不是对象，或者参数为空**

   如果参数是一个原始值，或者是一个不具有then方法的对象，则Promise.resolve方法返回一个新的 Promise 对象，状态为resolved。

   

3. **参数是一个thenable对象**
   thenable对象指的是具有then方法的对象。

   ```js
   let thenable = {
     then: function(resolve, reject) {
       resolve(42);
     }
   };
   ```

   Promise.resolve方法会将这个对象转为 Promise 对象，然后就立即执行thenable对象的then方法。



立即`resolve`的 Promise 对象，是在本轮“事件循环”（event loop）的结束时，而不是在下一轮“事件循环”的开始时。

```js
setTimeout(function () {
  console.log('three');
}, 0);

Promise.resolve().then(function () {
  console.log('two');
});

console.log('one');

// one
// two
// three
//setTimeout(fn, 0)在下一轮“事件循环”开始时执行，Promise.resolve()在本轮“事件循环”结束时执行，console.log('one')则是立即执行，因此最先输出。
```

## Promise.reject()

`Promise.reject(reason)`方法也会返回一个新的 Promise 实例，该实例的状态为`rejected`。

==注意== `reject`方法的作用，等同于抛出错误`throw`。

如果 `Promise` 状态已经变成`resolved`，再抛出错误是无效的。

```js
const p2 = new Promise((resolve, reject) => {
  reject(new Error('报错了'));
})
.then(result => result)
.catch(e => console.log(e));
//等价于
const p2 = new Promise((resolve, reject) => {
  throw new Error('报错了');
})
.then(result => result)
.catch(e => console.log(e));

```



`Promise.reject()`方法的参数，会原封不动地作为`reject`的理由，变成后续方法的参数。这一点与`Promise.resolve`方法不一致。

```js
const thenable = {
  then(resolve, reject) {
    reject('出错了');
  }
};

Promise.reject(thenable)
.catch(e => {
  console.log(e === thenable)
})
// true
//Promise.reject方法的参数是一个thenable对象，执行以后，后面catch方法的参数不是reject抛出的“出错了”这个字符串，而是thenable对象。


```

## Promise.prototype.then()

`then`方法可以接受两个回调函数作为参数。第一个回调函数是`Promise`对象的状态变为`resolved`时调用，第二个回调函数(可选)是`Promise`对象的状态变为`rejected`时调用。

回调函数接受之前`Promise`==传递出来==的值作为参数。

`then`方法返回值为一个`Promise`，它的行为与then中的回调函数的返回值有关。`catch`同理。

- 如果`then`中的回调函数返回一个值，那么`then`返回的`Promise`将会成为接受状态，并且将**返回的值**作为接受状态的回调函数的参数值
- 如果then中的回调函数抛出一个错误，那么then返回的Promise将会成为拒绝状态，并且将抛出的错误作为拒绝状态的回调函数的参数值。
- 如果`then`中的回调函数返回一个`Promise`，那么then返回的`Promise`的状态跟回调函数返回的`Promise`一致，并且将那个`Promise`的对应状态的回调函数的参数值作为`then`返回的`Promise`的对应状态回调函数的参数值。即使回调函数返回的`Promise`的状态是`pending`，则`then`返回的`Promise`状态也会一直与其同步。前者发生变化，后者会跟着变化。

```js
const p2 = new Promise((resolve, reject) => {
  resolve(1);
})
.then(result => result+1)// 回调返回值为一个值，则then返回的promise状态为fulfilled且下一个回调的参数为2；
.then(e => throw new Error(e)); //回调抛出一个错误，则then返回的promise状态为rejected且下一个回调的参数为2；
.catch(e => {		//回调函数返回一个Promise,则catch返回的promise状态为回调返回的						Promise的状态，即rejected。
    return new Promise((resolve,rejected){
                  //...somde code
                  reject(e)       
    			})
	})
```



##  Promise.prototype.catch()

`catch`方法是`then(null,rejection)`方法的别名，用于`Promise`发生错误时的调用回调函数。

`Promise` 对象的错误会一直向后传递，直到被捕获为止。

`then`方法指定的回调函数，如果运行中抛出错误，也会被`catch`方法捕获。

`catch`不会捕获在它之后的报错。

```js
p.then((val) => console.log('fulfilled:', val)) 
	.then((res) => console.log('fulfilled:', res)) 
  		.catch((err) => console.log('rejected', err));//catch之前的then都没有第二个回调来捕捉抛出的错误，故可被catch捕获。

// 等同于
p.then((val) => console.log('fulfilled:', val))
	.then((res) => console.log('fulfilled:', res)) 	
  		.then(null, (err) => console.log("rejected:", err));
```

`catch`方法返回值为一个`Promise`，它的行为与`then`一致。

## Promise.prototype.finally()

`finally`方法用于指定不管 Promise 对象最后状态如何，都会执行的操作



```js
promise
.then(result => {···})
.catch(error => {···})
.finally(() => {···});//必定被执行的
```



## Promise.all()

`Promise.all`方法用于将多个 Promise 实例，包装成一个新的 Promise 实例。



 ```js
const p = Promise.all([p1, p2, p3]);
//p的状态由p1、p2、p3决定，分成两种情况。

//（1）只有p1、p2、p3的状态都变成fulfilled，p的状态才会变成fulfilled，此时p1、p2、p3的返回值组成一个数组，传递给p的回调函数。

//（2）只要p1、p2、p3之中有一个被rejected，p的状态就变成rejected，此时第一个被reject的实例的返回值，会传递给p的回调函数。
 ```



==注意==，如果作为参数的 Promise 实例，自己定义了`catch`方法，那么它一旦被`rejected`，并不会触发`Promise.all()`的`catch`方法。

```js
const p1 = new Promise((resolve, reject) => {
  resolve('hello');
})
.then(result => result)

const p2 = new Promise((resolve, reject) => {
  throw new Error('报错了');
})
.then(result => result)
.catch(e => e);   //但是p2有自己的catch方法，该方法返回的是一个新的 Promise 实例，p2指向的实际上是这个实例。

Promise.all([p1, p2])
.then(result => console.log(result))
.catch(e => console.log(e));
// ["hello", Error: 报错了]

//上面代码中，p1会resolved，p2首先会rejected，但是p2有自己的catch方法，该方法返回的是一个新的 Promise 实例，p2指向的实际上是这个实例。该实例执行完catch方法后，也会变成resolved，导致Promise.all()方法参数里面的两个实例都会resolved，因此会调用then方法指定的回调函数，而不会调用catch方法指定的回调函数。


```



## Promise.race()

`Promise.race`方法同样是将多个 Promise 实例，包装成一个新的 Promise 实例。

```js
const p = Promise.race([
  fetch('/resource-that-may-take-a-while'),
  new Promise(function (resolve, reject) {
    setTimeout(() => reject(new Error('request timeout')), 5000)
  })
]);

p
.then(console.log)
.catch(console.error);
//Promise.race()方法与Promise.all()的差别在于，Promise.race()方法只要p1、p2、p3之中有一个实例率先改变状态，p的状态就跟着改变。
```



## Promise.try() 待定

> 写在try之前。 

将同步函数变成异步执行。

```js
const f = () => console.log('now');
Promise.resolve().then(f);
console.log('next');
// next
// now
```



实际开发中，经常遇到一种情况：不知道或者不想区分，函数`f`是同步函数还是异步操作，但是想用 Promise 来处理它因为这样就可以不管`f`是否包含异步操作，都用`then`方法指定下一步流程，用`catch`方法处理`f`抛出的错误。

`Promise.try`就是让同步函数同步执行，异步函数异步执行的一个API

事实上，`Promise.try`就是模拟`try`代码块，就像`promise.catch`模拟的是`catch`代码块。



```js
Promise.try(database.users.get({id: userId}))
  .then(...)
  .catch(...)
// 等价于

```