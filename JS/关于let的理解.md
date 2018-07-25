ES6规定，如果代码块中存在`let`，这个区块从一开始就形成了封闭作用域,凡是在声明之前就使用，就会报错。即在代码块内，在`let`声明之前使用变量都是不可用的。



今天突然想起前端面试题里的一个题，如下

```js
for (var i = 0; i < 5; i++) {
    setTimeout(function() {
        console.log(i);
    }, 1000);
}// 5 5 5 5 5 

```

然而将var 改成let 之后，结果却发生了改变。

```js
for (let i = 0; i < 5; i++) {
    setTimeout(function() {
        console.log(i);
    }, 1000);
} // 0 1 2 3 4
```

关于let可知

-   let 声明的变量的作用域是块级的；
-   let 不能重复声明已存在的变量；
-   let 有暂时死区，不会被提升。

但是，

```js
let i = 0;
for (; i < 5; i++) {
    setTimeout(function() {
        console.log(i);
    }, 1000);
} // 5 5 5 5 5 
```

将let提出for ，结果却与上面不同。

所以，let之于for循环，其实没那么简单。简单的可以理解

1.  **for( let i = 0; i< 5; i++) 这句话的圆括号之间，有一个隐藏的作用域**

2.  **for( let i = 0; i< 5; i++) { 循环体 } 在每次执行循环体之前，JS 引擎会把 i 在循环体的上下文中重新声明及初始化一次。**

    ```
    var liList = document.querySelectorAll('li') // 共5个li
    for( let i=0; i<liList.length; i++){
      let i = 隐藏作用域中的i // 看这里看这里看这里
      liList[i].onclick = function(){
        console.log(i)
      }
    }
    ```

　for循环头部的let不仅将i绑定到了for循环的块中，事实上它将其重新绑定到了循环的每一个迭代中，确保使用上一个循环迭代结束时的值重新进行赋值







------



```js
    for (let i=0; i<10; i++) { 
    	console.log( i ); 
    }
    console.log( i ); // ReferenceError
```

for 循环头部的 let 不仅将 i 绑定到了 for 循环的块中，事实上它将其重新绑定到了循环 的每一个迭代中，确保使用上一个循环迭代结束时的值重新进行赋值。 



下面通过另一种方式来说明每次迭代时进行重新绑定的行为:

```js
{ 
	let j;

    for (j=0; j<10; j++) { 

    	let i = j; // 每个迭代重新绑定! 

        console.log( i );
    }
} 
```

