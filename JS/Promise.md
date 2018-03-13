## Promise.prototype.finally()

`finally`方法用于指定不管 Promise 对象最后状态如何，都会执行的操作



```
promise
.then(result => {···})
.catch(error => {···})
.finally(() => {···});
```