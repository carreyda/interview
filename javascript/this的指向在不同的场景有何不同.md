## this 的指向在不同的场景下(普通函数、箭头函数、构造函数、bind/call/apply)有何不同？

> 考察点: JS 中最常见也是最关键的概念之一,考察上下文(Context)的理解。

### 普通函数调用(默认绑定)

当一个函数作为普通函数被独立调用时,this 的指向取决于代码是否处于**严格模式(StrictMode)**。

- 非严格模式下: this 指向全局对象,在浏览器中是 window,在 Node.js 中是 global。
- 严格模式下:this 的值是 undefined 。

````javascript
function showThis() {
  console.log(this);
}
I;
showThis(); // 非严格模式下输出:window 对象

function showThisStrict() {
  "use strict";
  console.log(this);
}

showThisStrict(); //严格模式下输出:undefined


//常见陷阱:对象方法被赋值给变量后独立调用

const user = {
    name: 'Alice',
    sayHi: function(){
        console.log(this.name);
    }
};

user.sayHi(); // this -> user, 输出: "Alice"(这是对象方法调用,见下文)

​```
````

### 对象方法调用(隐式绑定)

当一个函数作为对象的方法被调用时(即通过 `object.method()` 的形式)，`this`指向调用该方法的对象(也就是`.`前面的那个对象)。

```javascript
const person = {
  name: "Bob",
  greet: function () {
    // 这里的 this 指向调用 greet 方法的 person 对象
    console.log(`Hello, my name is ${this.name}`);
  },
};

person.greet(); //输出:"Hello, my name is Bob"

const anotherPerson = {
  name: "Charlie",
};
// 即使 greet 方法定义在 person 上,但只要通过 anotherPerson 调用, this 就会指向 anotherPerson
anotherPerson.greet = person.greet;
anotherPerson.greet(); //输出:"Hello, my name is Charlie"
```

### 构造函数调用(new 绑定)

当一个函数与 new 关键字一起使用时,它就成了一个构造函数。
在这种情况下,this 指向新创建的对象实例。new 关键字会自动完成以下操作:

1.创建一个新的空对象。 2.将这个新对象的 proto 指向构造函数的 prototype。 3.将 this 绑定到这个新创建的对象上。 4.执行构造函数内部的代码。 5.如果构造函数没有显式返回一个对象,则隐式返回这个新创建的对象。
