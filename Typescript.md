### 类型注解

简而言之：约束类型。

TypeScript里的类型注解是一种轻量级的为函数或变量添加约束的方式。 在这个例子里，我们希望 `greeter`函数接收一个字符串参数。 然后尝试把 `greeter`的调用改成传入一个数组，编译会失败。

```typescript
function greeter(person: string) {
    return "Hello, " + person;
}

let user = [0, 1, 2];

document.body.innerHTML = greeter(user);//greeter.ts(7,26): error TS2345: Argument of type 'number[]' is not assignable to parameter of type 'string'.
```

### 接口

使用接口来描述一个拥有`firstName`和`lastName`字段的对象，在实现接口时，只要参数对象保证**包含**了接口要求的结构就行。

```ts
interface Person {
    firstName: string;
    lastName: string;
}

function greeter(person: Person) {
    return "Hello, " + person.firstName + " " + person.lastName;
}

let user = { Family:"123",firstName: "Jane", lastName: "User" };
//结构属性名(firstName之类)不能省
document.body.innerHTML = greeter(user);//可行
```



### 类

在在构造函数的参数上使用`public`等同于创建了同名的成员变量。

即public firstName等价于this.firstName = firstName。

```ts
class Student {
    fullName: string;
    constructor(public firstName, public middleInitial, public lastName) {
        this.fullName = firstName + " " + middleInitial + " " + lastName;
    }
}

interface Person {
    firstName: string;
    lastName: string;
}

function greeter(person : Person) {
    return "Hello, " + person.firstName + " " + person.lastName;
}

let user = new Student("Jane", "M.", "User");

document.body.innerHTML = greeter(user);
```

