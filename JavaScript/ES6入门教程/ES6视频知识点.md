**ES6**

**ES6 实际上是一个泛指，泛指 ES 2015 及后续的版本**。

**为什么要学习ES6**

ES6加入了许多新的语法特性，编程实现更加简单、高效。也是现在前端发展的趋势。而现在主流的框架 Vue.js 和 React.js 的默认语法，都是用的 ES6。

**const关键字**

对于数组和对象的元素修改，不算作对常量的修改，不会报错。因为保存对象的变量是一个地址，我们的地址还是没有发生改变的。

即const允许对对象和数组进行修改。

所以在开发中建议：声明对象类型用const，非对象类型声明选择let。

**变量的解构赋值**

ES6允许按照一定的模式，从数组和对象中提取值，对变量进行赋值，即被称为解构赋值。

**ES6中的模板字符串==》引入 反引号  ${}**

**箭头函数 箭头 => 来定义函数**

在箭头函数中，this是静态的，this始终**指向函数声明时所在作用域**下的this值。

ES6规范中的箭头函数里面的this是在**定义它时绑定**的，指向它的父级作用域，而不是调用它对象，这个this的指向是**不能通过call和apply改变**的。

即箭头函数不仅可以让代码更加的简洁和优雅，还可以利用它的this指向。

ES6 之前的普通函数中：this 指向的是函数被调用的对象（也就是说，谁调用了函数，this 就指向谁）。

ES6 的箭头函数中：**箭头函数本身不绑定 this**，this 指向的是**箭头函数定义位置的 this**（也就是说，箭头函数在哪个位置定义的，this 就跟这个位置的 this 指向相同）。

无论怎么样都无法改变箭头函数中的this指向。

箭头函数不能作为用于构造函数。

箭头函数中不能使用arguments变量。

**箭头函数不会更改this指向，用来指定回调函数会非常合适**

**ES6中的rest参数**

ES6引入rest参数，用来获取函数的实参，用来代替arguments（伪数组），rest参数数组。

**扩展运算符  ...  **

将一个数组转换为用逗号分隔的参数序列，即**对数组进行解包**。

**注意：**rest参数是放在定义函数的形参处，扩展运算符一般放在函数的调用的实参处。

**ES6中的symbol**

ES6引入的一种新的原始数据类型，表示**独一无二**的值。

Symbol的值不能和其他数据进行运算。

Symbol定义的对象属性不能使用for ... in 循环遍历得到，但是可以使用Reflect.ownKeys来获取对象的所有键名。

```html
<script>
    //创建Symbol
    let s=Symbol();
    console.log(s,typeof s);

    //里面的参数只是一个标识，这里创建出来的任然是一个独一无二的值
    let s2=Symbol('wahaha');
    let s3=Symbol('wahaha');
    console.log(s2===s3); //false

    let s4=Symbol.for('wahaha');
    let s5=Symbol.for('wahaha');
    console.log(s4===s5); //true
</script>
```

**七种数据类型  USONB**

U：undefined

S：string     symbol

O：object

N：null    number

B：boolean

**迭代器**

Iterator接口指的是对象里面的一个属性。

**自定义实现一个迭代器**

```javascript
//当我们自定义遍历数据的时候，要想到迭代器
const banji={
    names:'中级一般',
    stus:['wahaha','nalala','wlala','ewuwuwu'],
//    自定义实现迭代器
    [Symbol.iterator](){
        let index=0;
        let _this=this;
        return{
            next(){
                if(index<_this.stus.length){
                    const res={value:_this.stus[index],done:false};
                    index++;
                    return res;
                }else{
                    return {value: undefined,done: true}
                }
            }
        };
    }
};
//使用for ... of 来遍历这个对象
for (let v of banji){
    console.log(v);
}
```

**生成器**

生成器其实就是一个特殊的函数。**异步编程**。是对异步编程的一种新的解决方案，以前是用回调函数。

```html
<script>
    //yield可以表示为函数的分隔符
    function * gen() {
        console.log("11111");
        yield '第一部分';
        console.log("22222");
        yield '第二部分';
        console.log("333333");
        yield '第三部分';
        console.log("44444");
    }
    //这里产生的是一个迭代器对象
    let iterator=gen();
    for (let v of iterator){
        console.log(v);
    }
</script>
```

生成器可以解决回调地狱的问题。

**Promise**

promise是ES6引入的异步编程想新解决方案，语法上Promise是一个构造函数。

用来**封装异步操作**并可以获取其成功和失败的结果。

基本使用：

```javascript
<script>
    //实例化 Promise对象
    const p=new Promise(function (resolve, reject) {
    //    在里面对异步操作进行封装
        setTimeout(function () {
            let data='数据';
            resolve(data);
        })
    });
    p.then(function (value) {
        console.log(value)
    })
</script>
```

使用**Promise来封装Ajax**：结构更加清晰，也不会产生回调地狱的问题。

```html
<script>
//    使用Promise对Ajax进行封装
    const p=new Promise(function (resolve, reject) {
        let xhr=new XMLHttpRequest();
        xhr.open('GET','http://www.baidu.com');
        xhr.send();
        xhr.onreadystatechange=function () {
            if (xhr.readyState===4){
                if(xhr.status>=200&&xhr.status<300){
                    resolve(xhr.response);
                }else{
                    reject(xhr.status);
                }
            }
        }
    });
    p.then(function (value) {
        console.log(value)
    }).catch(function (error) {
        console.log('失败');
        console.log(error);
    })
</script>
```

**Set**

ES6提供了新的数据结构Set集合，类似于数组，但是成员的值是唯一的，可以自动进行去重，集合实现了 iterator接口，所以可以使用for ... of进行遍历。

**Map**

Es6提供了Map数据结构，类似于对象，也是键值对的集合。但是“键”的范围不局限于字符串，各种类型的值（包括对象）都可以当做键。Map也实现了iterator接口，所以可以使用 for ... of来遍历。

**class类**

ES6提供了更接近传统语言的写法，引入了class类的概念。提供class关键字，可以定义类，但只是一个语法糖，只是让对象原型的写法更加清晰，更像面向对象编程的语法而已。

class静态成员：属于类，但是不属于实例对象的属性，就叫做静态成员。

**类继承**

**async和await**

async和await两种语法结合可以让异步代码像同步代码一样。

