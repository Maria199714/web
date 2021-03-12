**使用框架的目的**

提升效率。

**vue的核心概念**

1. 通过**数据驱动**界面更新，无需操作DOM来更新数据。使用vue我们只需要关系如何获取数据，如何处理数据，如何编写业务逻辑代码。我们只需要将处理好的数据交给vue，vue就会**自动**将数据渲染到界面中。

2. **组件化开发**，我们可以将网页拆分成一个个独立的组件来编写，将来再通过封装好的组件拼接成一个完整的网页。

   组件化开发思想

   ![](./images\组件化开发思想.png)

**vue的基本使用**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
<!--    1、引入Vue.js-->
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
</head>
<body>
<div id="app">
   <!-- 使用插值表达式来对数据进行引入-->
    <p>{{name}}</p>
</div>
<script>
    //   2、创建一个Vue的实例对象
    let vue=new Vue({
    //   3、告诉Vue的实例对象，将来需要控制界面上的哪个区域
        el:'#app',
    //   4、告诉Vue的实例对象，被控制区域的数据是什么
        data:{
            name:'刘媛媛'
        }
    });
</script>
</body>
</html>
```

在上面的代码中，我们从始至终都没有去操作DOM元素，而是vue自动的将数据渲染到了页面中。

**MVVM设计模式**

M：Model      数据模型

V：View          视图

VM：View Model 数据模型和视图的桥梁

MVVM设计模式最大的特点就是支持数据的双向传递：

数据可以从   M-》VM-》V

也可以从       V-》VM-》M

**Vue中MVVM的划分**

vue是基于MVVM设计模式的。

被控制的区域：VIew

Vue实例对象：View Model

实例对象中的data：Model

**vue中数据的双向绑定**

数据双向绑定的限制条件：

默认情况下vue只支持数据的单向传递 M-》VM-》V。

但是由于Vue是基于MVVM设计模式的，所以也提供了双向传递的能力。

即在**input、textarea、select**元素上可以使用v-model指令创建双向数据绑定。

注意点：**v-model**会忽略所有表单元素的value、checked、selected特性的初始值，而总是将vue实例的数据作为数据来源。你应该通过 JavaScript 在`data` 选项中声明初始值。

```html
<body>
<div id="app">
    <input type="text" v-model="name">
</div>
<script>
    var app=new Vue({
        el:'#app',
        data:{
            name:'娃哈哈'
        }
    });
</script>
</body>
```

即双向数据的传递需要特定的标签（input、textarea、selected）和特定的指令（v-model）。

**vue中的指令**

指令是vue内部提供的一些自定义属性。这些属性中封装好了vue内部实现的一些功能，只要使用这些指令就可以使用vue中实现的这些功能。即提供指令来实现vue内部中的一些功能。

**数据绑定**

只要数据发生改变，界面中就会更新显示。

v-once指令：让界面不要跟着数据变化，只渲染一次。

v-cloak指令：在数据没有成功渲染之前，先让元素隐藏；数据渲染成功之后将元素显示出来。和 CSS 需要和CSS规则如`[v-cloak] { display: none }` 一起用时才能达到上面隐藏和显示的效果。

v-text指令：相当于DOM中的innerText。

v-html指令：相当于DOM中的innerHTML。

v-if指令：条件渲染，如果v-if的取值为true就渲染元素，为false就不渲染这个元素。如果条件不满足根本就**不会创建**这个元素。

v-show指令：和v-if指令一样都是条件渲染，取值为true就显示，取值为false就不显示。如果条件不满足还是**会创建**这个元素，只是设置该元素的display为none。（显示与隐藏推荐使用v-show指令）

**v-for指令**：相当于JS中的for in 循环，可以根据数据多次的渲染元素。

v-for特点：可以遍历数组、对象、字符串、数字。

```html
<body>
<div id="app">
    <ul>
        <li v-for="(value,index) in list">{{index}}----{{value}}</li>
    </ul>
</div>
<script>
    var app=new Vue({
        el:'#app',
        data:{
            list:['张三','李四','王五','王小二']
        }
    });
</script>
</body>
```

**v-bind指令**：

如果想要给元素绑定数据，我们可以使用{{}}、v-text、v-html。

但是如果要给**元素的属性绑定数据**，就必须使用v-bind，即v-bind是专门用于给元素的属性绑定数据的。

```html
<body>
<div id="app">
    <p v-bind:id="cls">我是元素属性的数据绑定</p>
    <p :id="tls">我也是 简写</p>
</div>
<script>
    var app=new Vue({
        el:'#app',
        data:{
            cls：'wahaha'
            tls:'haha'
        }
    });
</script>
</body>
```

v-bind指令给任意标签的任意属性绑定数据。

对于大部分的属性我们只需要像上面直接赋值即可，例如  `：value="name"`

但是对于**class和style**属性而言，他们的格式比较特殊：

如果需要通过v-bind给class绑定类名，那么不能直接赋值。默认情况下v-bind回去Model中查找数据，但是Model中没有对应的类名，所以不能直接赋值。我们希望它去style标签中去查找类名，那么我们就必须把类名放在**数组**中，但是放到数组中默认还是回去Model中查找，还需要将类名用**单引号**将类名括起来，这样，v-bind绑定的class数据就回去style标签中进行查找。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <style>
       .one{
           font-size: 50px;
       }
    </style>
</head>
<body>
<div id="app">
    <p :class="['one']">我是段落</p>
</div>
<script>
    var app=new Vue({
        el:'#app',
        data:{
        }
    });
</script>
</body>
</html>
```

**v-on指令**：专门用于给元素绑定监听事件。

如果通过v-on来绑定监听事件，那么在指定事件名称的时候不需要写on；

如果通过v-on来绑定监听事件，那么在赋值的时候必须赋值一个回调函数。

```html
<body>
<div id="app">
    <button v-on:click="fun">点击我一下</button>
    <button @click="fun">点击我一下</button>
</div>
<script>
    var app=new Vue({
        el:'#app',
        data:{
        },
        methods:{
            fun(){
                alert('wahaha')
            }
        }
    });
</script>
</body>
```

v-on修饰符：

在事件中有很多东西需要我们处理，例如事件冒泡、事件捕获、阻止默认行为等，我们可以通过v-on事件修饰符来处理。

常见修饰符：

.once：只触发一次回调。

.prevent：阻止元素的默认行为。

.self： 只当事件是从侦听器绑定的元素本身触发时才触发回调。

.stop：阻止事件冒泡。

.capture：默认情况下是事件冒泡，将事件冒泡变成事件捕获。

**自定义全局指令**

在vue中除了可以使用vue内置的一些指令以外，我们还可以自定义指令。

```html
<body>
<div id="app">
<!--    自定义指令来改变文字的颜色-->
    <p v-color>我是段落</p>
</div>
<script>
    /**
     * directive接收两个参数
     * 第一个参数：自定义指令的名称
     * 第二个参数：对象
     */
    Vue.directive("color",{
        //这里的el就是被绑定指令的那个元素
        bind:function (el) {
            el.style.color='red';
        }
    });
    var app=new Vue({
        el: '#app',
        data: {}
    })
</script>
</body>
```

**自定义局部指令**

```html
<body>
<div id="app">
<!--    自定义指令来改变文字的颜色-->
    <p v-color>我是段落</p>
</div>
<script>
    var app=new Vue({
        el: '#app',
        data: {},
        methods:{},
        //专门用于定义局部指令的
        directives:{
            "color":{
                bind:function (el) {
                    el.style.color='green';
                }
            }
        }
    })
</script>
</body>
```

**vue中的计算属性computed**

虽然在定义计算属性的时候是通过一个函数返回的数据，但是在使用计算属性的时候不能在计算属性名称后面加上（），因为它是一个属性不是一个函数。

函数处理和计算属性处理数据的区别：

函数的特点：每次调用都会执行。

计算属性的特点：只要返回的结果没有变化，那么计算属性就只会执行一次。

所以如果返回的数据不经常发生变化，那么使用计算属性的性能会比使用函数的性能高。

**vue中的过滤器filter**

过滤器和函数和计算属性一样都是用来处理数据的

但是过滤器一般用于格式化插入的文本数据。

自定义全局过滤器

```html
<body>
<div id="app">
<!--    在这里使用过滤器 vue会把name交给指定的过滤器处理之后，再把处理之后的结果插入到指定的元素中-->
    <p>{{name | formartStr}}</p>
</div>
<script>
    /**
     * 如何定义一个全局过滤器
     * 通过Vue.filter()p
     * 第一个参数：过滤器名称
     * 第二个参数：处理数据的函数
     * 注意点：默认情况下处理数据的函数接收一个参数，就是当前需要被处理的数据
     * @type {Vue}
     */
    Vue.filter("formartStr",function (value) {
        return value+'我是新增加的';
    });
    var app=new Vue({
        el: '#app',
        data: {
            name:'wahaha'
        },
        methods:{}
    })
</script>
</body>
```

自定义局部过滤器

```html
<body>
<div id="app">
<!--    在这里使用过滤器 vue会把name交给指定的过滤器处理之后，再把处理之后的结果插入到指定的元素中-->
    <p>{{name | formartStr}}</p>
</div>
<script>
    var app=new Vue({
        el: '#app',
        data: {
            name:'wahaha'
        },
        methods:{},
        //专门用来定义局部过滤器
        filters:{
            'formartStr':function (value) {
                return value+"我也是新添加的";
            }
        }
    })
</script>
</body>
```

**vue中的过渡动画**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <style>
        .box{
            width: 100px;
            height: 100px;
            background: rebeccapurple;
        }
        .v-enter{
            opacity: 0;
        }
        .v-enter-to{
            opacity: 1;
        }
        .v-enter-active{
            transition: all 3s;
        }
        .v-leave{
            opacity: 1;
        }
        .v-leave-to{
            opacity: 0;
        }
        .v-leave-active{
            transition: all 3s;
        }
    </style>
</head>
<body>
<div id="app">
    <button @click="toggle">我是按钮</button>
    <transition>
        <div class="box" v-show="isShow"></div>
    </transition>
</div>
<script>
    var app=new Vue({
        el: '#app',
        data: {
            isShow:true
        },
        methods:{
            toggle(){
                this.isShow=!this.isShow
            }
        }
    })
</script>
</body>
</html>
```

**vue组件**

在前端开发中组件就是把一个很大的界面拆分为多个小的界面，每一个小的界面就是一个组件。

将大界面拆分成小界面就是组件化。

**组件化的好处**

1. 可以简化Vue实例代码
2. 可以提高复用性

**Vue中如何创建组件**

1. 创建组件构造器
2. 注册已经创建好的组件
3. 使用注册好的组件

注意点：在创建组件指定组件的模板的时候，模板**只能有一个根元素**。

全局组件：

```html
<body>
<div id="app">
    <mycon></mycon>
</div>
<!--这个是专门用于编写组件模板的标签-->
<template id="con">
    <div>
        <p>我是段落1</p>
        <p>我是段落2</p>
    </div>
</template>
<script>
    /*
    创建、注册一个全局组件
     */
    Vue.component("mycon",{
        template:'#con'
    });
    var app=new Vue({
        el: '#app',
        data: {
        }
    })
</script>
</body>
```

局部组件：

```html
<body>
<div id="app">
    <abc></abc>
</div>
<!--这个是专门用于编写组件模板的标签-->
<template id="con">
    <div>
        <p>我是段落1</p>
        <p>我是段落2</p>
    </div>
</template>
<script>
    var app=new Vue({
        el: '#app',
        data: {
        },
        //专门用于自定义局部组件
        components:{
            "abc":{
                template:'#con'
            }
        }
    })
</script>
</body>
```

