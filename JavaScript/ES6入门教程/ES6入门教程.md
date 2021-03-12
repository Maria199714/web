**ES6与ECMAScript 2015的关系**

ES6 既是一个历史名词，也是一个泛指，含义是 5.1 版以后的 JavaScript 的下一代标准，涵盖了 ES2015、ES2016、ES2017 等等，现在的ES6一般还是指的是ES2015。

l用let声明的变量只在它所在的代码块中有效。

```javascript
{
    var a=1;
    let b=2;
}
console.log(a);//ReferenceError: b is not defined
console.log(b);//1
```

**作用域**：作用域（scope）指的是变量存在的范围。在 ES5 的规范中，JavaScript 只有两种作用域：一种是全局作用域，变量在整个程序中一直存在，所有地方都可以读取；另一种是函数作用域，变量只在函数内部存在。

