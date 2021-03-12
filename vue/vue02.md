**Vue组件**

vue组件中的methods和data的用法：

```html
<body>
<div id="app">
    <abc></abc>
</div>
<!--这个是专门用于编写组件模板的标签-->
<template id="con">
    <div>
        <p>我是段落1</p>
        <p>我是段落2{{msg}}</p>
        <button @click="fun">我是组件中的按钮</button>
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
                template:'#con',
                //在组件中的数据必须是一个函数，通过return的形式来将数据进行返回
                data(){
                    return {
                        msg:'wahaha'
                    }
                },
                methods:{
                    fun(){
                        alert('我是组件中的响应事件')
                    }
                }
            }
        }
    })
</script>
</body>
```

**组件中的data为什么是一个函数的形式？**

注意：组件中的data如果不是通过函数返回的，那么多个组件就会共用一份数据，会导致数据混乱。

如果组件中的data是通过函数返回的，那么每创建一个新的组件，就会调用一次这个方法，将这个方法返回的数据和当前创建的组件绑定在一起，就可以有效的避免组件数据之间的混乱。

**动态组件与异步组件**

```html
<body>
<div id="app">
    <button @click="toggle">切换组件</button>
<!--    这里写的是动态组件，可以动态的实现切换  keep alive可保存组件切换之前的状态-->
    <keep-alive>
        <component v-bind:is="name"></component>
    </keep-alive>
</div>
<!--这个是专门用于编写组件模板的标签-->
<template id="home">
    <div>
        <p>我是主页</p>
        <input type="checkbox">
    </div>
</template>
<template id="part">
    <div>
        <p>我不是主页</p>
    </div>
</template>
<script>
    var app=new Vue({
        el: '#app',
        data: {
            name:'home'
        },
        methods: {
            toggle(){
                this.name=this.name==='home'?'part':'home';
            }
        },
        //专门用于自定义局部组件
        components:{
            "home":{
                template:'#home'
            },
            "part":{
                template: "#part"
            }
        }
    })
</script>
</body>
```

**父子组件**

在自定义组件中再定义其他组件。

子组件永远只能在定义它的那个父组件中使用。

```html
<body>
<div id="app">
    <father></father>
</div>
<template id="father">
    <div>
        <p>我是父组件</p>
<!--        在父组件中使用定义好的子组件-->
        <son></son>
    </div>
</template>
<template id="son">
    <div>
        <p>我是子组件</p>
    </div>
</template>
<script>
    var app=new Vue({
        el: '#app',
        data: {},
        methods: {},
        //专门用于自定义局部组件
        components:{
            "father":{
                template:'#father',
                //定义我的子组件
                components: {
                    'son':{
                        template: '#son'
                    }
                }
            }
        }
    })
</script>
</body>
```

**父子组件传递**

在vue中，默认子组件是不能访问父组件的数据的。

如果子组件想要访问父组件的数据，**必须通过父组件传递**。

**父组件将数据传递给子组件步骤：**

1. 在父组件中通过v-bind传递数据

   传递格式  v-bind:自定义接收名称=“要传递的数据”

2. 在子组件中通过props接收数据

   接收格式  props：["自定义接收名称"]

注意：传递到子组件的自定义名称要和接收的名称一致。

```html
<body>
<div id="app">
    <father></father>
</div>
<template id="father">
    <div>
        <p>我是父组件</p>
<!--        这里通过v-bind将数据传递给子组件-->
        <son v-bind:parentname="name"></son>
    </div>
</template>
<template id="son">
    <div>
        <p>我是子组件</p>
<!--        在这里使用了父组件传递过来的数据-->
        <p>{{parentname}}</p>
    </div>
</template>
<script>
    var app=new Vue({
        el: '#app',
        data: {},
        methods: {},
        //专门用于自定义局部组件
        components:{
            "father":{
                template:'#father',
                data(){
                    return{
                        name:'wahaha',
                        age:12
                    }
                },
                components: {
                    'son':{
                        template: '#son',
                        //接收父组件传递过来的数据
                        props:['parentname']
                    }
                }
            }
        }
    })
</script>
</body>
```

**父组件将方法传递给自组件步骤：**

vue中，默认子组件是不能访问父组件的方法的。

如果子组件要访问父组件的方法，必须通过父组件传递。

父组件将方法传递给子组件的步骤：

1. 在父组件中通过v-on传递方法

   传递格式  v-on:自定义接收名称=“要传递的方法”

2. 在子组件中定义一个方法

3. 在自定义方法中通过this.$emit("自定义接收名称")来触发传递过来的方法

```html
<body>
<div id="app">
    <father></father>
</div>
<template id="father">
    <div>
        <p>我是父组件</p>
<!--        这里通过v-bind将数据传递给子组件-->
        <son v-bind:parentname="name" v-on:parentsay="fun"></son>
    </div>
</template>
<template id="son">
    <div>
        <p>我是子组件</p>
<!--        在这里使用了父组件传递过来的数据-->
        <p>{{parentname}}</p>
<!--        调研表父组件传递过来的方法-->
        <button @click="sonFun">我是子组件中的按钮</button>
    </div>
</template>
<script>
    var app=new Vue({
        el: '#app',
        data: {},
        methods: {},
        //专门用于自定义局部组件
        components:{
            "father":{
                template:'#father',
                data(){
                    return{
                        name:'wahaha',
                        age:12
                    }
                },
                methods: {
                    fun(){
                        alert('我调用的是父组件中的方法')
                    }
                },
                components: {
                    'son':{
                        template: '#son',
                        //接收父组件传递过来的数据
                        props:['parentname'],
                        methods:{
                            sonFun(){
                                //触发父组件传递过来的方法
                                this.$emit('parentsay')
                            }
                        }
                    }
                }
            }
        }
    })
</script>
</body>
```

**子组件将数据传递给父组件**

既然我们可以将父组件的方法传递给子组件，即在子组件中调用父组件中的方法，

那么我们就可以在调用方法的时候给方法传递参数，传递的参数，就是我们需要传递的数据。

```html
<body>
<div id="app">
    <father></father>
</div>
<template id="father">
    <div>
        <p>我是父组件</p>
<!--        这里通过v-bind将数据传递给子组件-->
        <son v-bind:parentname="name" v-on:parentsay="fun"></son>
    </div>
</template>
<template id="son">
    <div>
        <p>我是子组件</p>
<!--        在这里使用了父组件传递过来的数据-->
        <p>{{parentname}}</p>
<!--        调研表父组件传递过来的方法-->
        <button @click="sonFun">我是子组件中的按钮</button>
    </div>
</template>
<script>
    var app=new Vue({
        el: '#app',
        data: {},
        methods: {},
        //专门用于自定义局部组件
        components:{
            "father":{
                template:'#father',
                data(){
                    return{
                        name:'wahaha',
                        age:12
                    }
                },
                methods: {
                    fun(sonname){
                        alert('我调用的是父组件中的方法');
                        alert(sonname);
                    }
                },
                components: {
                    'son':{
                        template: '#son',
                        data(){
                            return {
                                sonname:'ahaha'
                            }
                        },
                        //接收父组件传递过来的数据
                        props:['parentname'],
                        methods:{
                            sonFun(){
                                //触发父组件传递过来的方法
                                //第一个参数：函数名称，后面的参数：函数调用时候的参数
                                this.$emit('parentsay',this.sonname)
                            }
                        }
                    }
                }
            }
        }
    })
</script>
</body>
```

​	**vue组件-数据和方法的多级传递**

在vue中如果儿子想使用爷爷的数据，必须一层一层往下传递。

在vue中如果儿子想使用爷爷的方法，必须一层一层往下传递。

**组件中的插槽**

在使用子组件的时候，给子组件动态的添加一些内容。

默认情况下是不能在使用子组件的时候，给子组件动态的添加内容的。

如果想在使用子组件的时候，给子组件动态的添加内容，那么就必须使用插槽。

slot标签就是插槽，插槽其实就是一个坑，预先定义好的坑，由使用者来填这个坑。

插槽可指定默认数据，如果使用者没有填这个坑，那么就会显示默认数据。

如果使用者填了这个坑，那么就会利用使用者填坑的内容替换掉整个插槽。

**匿名插槽**：

```html
<body>
<div id="app">
    <son>
        <p>我是自己加入的</p>
        <p>我是自己加入的</p>
    </son>
</div>
<template id="son">
    <div>
        <p>我是头部</p>
        <slot>我是默认的插槽内容</slot>
    </div>
</template>
<script>
    var vm=new Vue({
        el:"#app",
        components:{
            "son":{
                template:'#son'
            }
        }
    });
</script>
</body>
```

一般只建议使用一个匿名插槽。

**具名插槽**

通过插槽的name属性给插槽指定名称。

在组件中使用插槽的时候可以通过slot="name"方式，指定当前内容用于替换哪一个内容。

```html
<body>
<div id="app">
    <son>
        <p slot="one">1111111</p>
        <p slot="one">22222</p>
        <p slot="two">33333</p>
    </son>
</div>
<template id="son">
    <div>
        <p>我是头部</p>
        <slot name="one">我是默认的插槽内容</slot>
        <slot name="two">2222</slot>
    </div>
</template>
<script>
    var vm=new Vue({
        el:"#app",
        components:{
            "son":{
                template:'#son'
            }
        }
    });
</script>
</body>
```

v-slot指令：

```html
<body>
<div id="app">
    <son>
<!--        v-slot指令代替了上面的slot属性-->
        <template v-slot:one>
            <p>111111</p>
            <p>999999</p>
        </template>
<!--        v-slot指令的简写-->
        <template #two>
            <p>787y878</p>
            <p>djfiuyfdi</p>
        </template>
    </son>
</div>
<template id="son">
    <div>
        <p>我是头部</p>
        <slot name="one">我是默认的插槽内容</slot>
        <slot name="two">2222</slot>
    </div>
</template>
<script>
    var vm=new Vue({
        el:"#app",
        components:{
            "son":{
                template:'#son'
            }
        }
    });
</script>
</body>
```

**vue组件-作用域插槽**

作用域插槽就是带数据的插槽，就是让父组件在填充子组件插槽内容时也能**使用子组件的数据**。

如何使用作用域插槽？

1. 在slot中通过`v-bind：数据名称=“数据名称”`方式暴露数据
2. 在父组件中通过<template slot-scope="作用域名称">接收数据
3. 在父组件的`<template></template>`中通过 作用域名称.数据名称  方式使用数据

作用域插槽的应用场景：子组件提供数据，父组件决定如何渲染。

```html
<body>
<div id="app">
    <son>
        <template slot-scope="abc">
            <div>
                <p>{{abc.names}}</p>
            </div>
        </template>
    </son>
</div>
<template id="son">
    <div>
        <p>我是头部</p>
        <slot v-bind:names="names">我是默认的插槽内容</slot>
    </div>
</template>
<script>
    var vm=new Vue({
        el:"#app",
        components:{
            "son":{
                template:'#son',
                data(){
                    return{
                        names:'wahahah'
                    }
                }
            }
        }
    });
</script>
</body>
```

在2.6.0中，可以使用v-slot指令来实现具名插槽和作用域插槽，即代替了slot和slot-scope指令。

**v-slot指令**可以告诉vue内容要填充到哪一个具名插槽，还可以告诉vue如何接收作用域插槽暴露的数据。

```html
<body>
<div id="app">
    <son>
        <template v-slot:one="abc">
            <div>
                <p>{{abc.names}}</p>
            </div>
        </template>
        <template #two="nn">
            <p>aaaaaaa</p>
            <p>{{nn.age}}</p>
        </template>
    </son>
</div>
<template id="son">
    <div>
        <p>我是头部</p>
        <slot v-bind:names="names" name="one">我是默认的插槽内容</slot>
        <slot :age="age" name="two">我是默认插槽22222</slot>
    </div>
</template>
<script>
    var vm=new Vue({
        el:"#app",
        components:{
            "son":{
                template:'#son',
                data(){
                    return{
                        names:'wahahah',
                        age:111
                    }
                }
            }
        }
    });
</script>
</body>
```

**VueX 状态管理模式**

在上面的学习我们知道在vue中，默认组件之间的数据传递是非常麻烦的，所以我们可以使用VueX来进行解决组件之间的数据传递。

注意：在导入Vuex之前必须先导入vue。

Vuex是vue配套的 公共数据管理工具，我们可以将共享的数据保存到Vuex中，方便整个程序中的任何组件都可以获取和修改Vuex中保存的公共数据。

```html
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<!--    注意：在导入Vuex之前必须先导入vue-->
<!--    1、导入Vuex-->
    <script src="vuex.js"></script>
</head>
<body>
<div id="app">
    <father></father>
</div>
<template id="father">
    <div>
        <!--在使用Vuex中保存的共享数据的时候，必须通过如下的格式来使用-->
        <p>我是父组件{{this.$store.state.msg}}</p>
        <son></son>
    </div>
</template>
<template id="son">
    <div>
        <p>我是子组件</p>
        <p>{{this.$store.state.msg}}</p>
    </div>
</template>
<script>
//2、创建Vuex对象
    const store = new Vuex.Store({
        //这里的state就相当于组件中的data，就是专门用于保存共享数据的
        state: {
            msg:"wahaha"
        }
    });
    var vm=new Vue({
        el:"#app",
        store:store,
        components:{
            'father':{
                template:'#father',
                components: {
                    'son':{
                        template: '#son'
                    }
                }
            }
        }
    });
</script>
</body>
```

注意：在祖先组件中添加**store**的key保存Vuex对象，只要祖先组件中保存了Vuex对象，祖先组件和所有的后代组件就可以使用Vuex中保存的共享数据了。

在使用Vuex中保存的共享数据的时候，必须通过**this.$store.state.msg**的格式来使用。

注意：在Vuex中不推荐直接修改共享数据，不利于维护。

**VueRouter 路由管理器**

Vue Router和v-if/v-show一样，是用来切换组件的显示的。

v-if和v-show是标记来切换（布尔值true、false）

Vue Router是用哈希来切换（#/xxx)

比v-if和v-show强大的是Vue Router不仅仅可以切换组件的显示，还可以在切换的时候传递参数。

```html
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<!--    1、导入vue Router-->
    <script src="vue-router.js"></script>
    <style>
        .lyy-active{
            background: green;
        }
    </style>
</head>
<body>
<div id="app">
    <router-link to="/one" tag="button">第一个页面</router-link>
    <router-link to="/two" tag="button">第二个页面</router-link>
<!--    路由匹配到的组件将被渲染到这里-->
    <router-view></router-view>
</div>
<template id="one">
    <div>
        <p>我是第一个页面</p>
    </div>
</template>
<template id="two">
    <div>
        <p>我是第二个页面</p>
    </div>
</template>
<script>
    var one={
        template:'#one'
    };
    var two={
        template: '#two'
    };
    //2、定义切换的规则（定义路由规则）
    const routes = [
        //重定向
        {path:'/',redirect:'/one'},
        { path: '/one', component: one },
        { path: '/two', component: two }
    ];
    //3、根据自定义的切换规则创建路由对象
    const router = new VueRouter({
        routes, // (缩写) 相当于 routes: routes
        linkActiveClass:'lyy-active'
    });
    var vm=new Vue({
        el:'#app',
        //4、将创建好的路由对象绑定到Vue实例对象上
        router:router
    });
</script>
</body>
```

**vue Router传递参数**

只要将vue Router挂载到了vue实例对象上，我们就可以通过vue.$route拿到路由对象

只要拿到路由对象，就可以通过路由对象拿到传递的参数

传递参数的两种方法：

1. 通过URL参数（？key=value&key=value），通过this.$route.query获取
2. 通过占位符传递（路由规则中/:key/:key，路径中/value/value)，通过this.$route.params获取

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<!--    1、导入vue Router-->
    <script src="vue-router.js"></script>
    <style>
        .lyy-active{
            background: green;
        }
    </style>
</head>
<body>
<div id="app">
    <router-link to="/one?name=lyy&value=wahaha" tag="button">第一个页面</router-link>
    <router-link to="/two/:ahaha/:111" tag="button">第二个页面</router-link>
<!--    路由匹配到的组件将被渲染到这里-->
    <router-view></router-view>
</div>
<template id="one">
    <div>
        <p>我是第一个页面</p>
    </div>
</template>
<template id="two">
    <div>
        <p>我是第二个页面</p>
    </div>
</template>
<script>
    var one={
        template:'#one',
        created(){
            console.log(this.$route);
            console.log(this.$route.query);
        }
    };
    var two={
        template: '#two',
        created() {
            console.log(this.$route);
            console.log(this.$route.params);
        }
    };
    //2、定义切换的规则（定义路由规则）
    const routes = [
        //重定向
        {path:'/',redirect:'/one'},
        { path: '/one', component: one },
        { path: '/two/:name/:age', component: two }
    ];
    //3、根据自定义的切换规则创建路由对象
    const router = new VueRouter({
        routes, // (缩写) 相当于 routes: routes
        linkActiveClass:'lyy-active'
    });
    var vm=new Vue({
        el:'#app',
        //4、将创建好的路由对象绑定到Vue实例对象上
        router:router
    });
</script>
</body>
</html>
```

**嵌套路由**

嵌套路由也称之为子路由，即在被切换的组件中又切换其他子组件。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<!--    1、导入vue Router-->
    <script src="vue-router.js"></script>
    <style>
        .lyy-active{
            background: green;
        }
    </style>
</head>
<body>
<div id="app">
    <router-link to="/one" tag="button">第一个页面</router-link>
    <router-link to="/two" tag="button">第二个页面</router-link>
<!--    路由匹配到的组件将被渲染到这里-->
    <router-view></router-view>
</div>
<template id="one">
    <div>
        <p>我是第一个页面</p>
        <router-link to="/one/one1" tag="button">第一个页面1</router-link>
        <router-link to="/one/one2" tag="button">第一个页面2</router-link>
        <router-view></router-view>
    </div>
</template>
<template id="oneSub1">
    <div>
        <p>我是第一个页面的第一个部分</p>
    </div>
</template>
<template id="oneSub2">
    <div>
        <p>我是第一个页面的第二个部分</p>
    </div>
</template>
<template id="two">
    <div>
        <p>我是第二个页面</p>
    </div>
</template>
<script>
    var one={
        template:'#one'
    };
    var one1={
        template:'#oneSub1'
    };
    var one2={
        template:'#oneSub2'
    };
    var two={
        template: '#two'
    };
    //2、定义切换的规则（定义路由规则）
    const routes = [
        //重定向
        {path:'/',redirect:'/one'},
        { path: '/one', component: one,children:[
                {
                    path:'one1',component:one1
                },
                {
                    path: 'one2',component: one2
                }
            ]},
        { path: '/two', component: two }
    ];
    //3、根据自定义的切换规则创建路由对象
    const router = new VueRouter({
        routes, // (缩写) 相当于 routes: routes
        linkActiveClass:'lyy-active'
    });
    var vm=new Vue({
        el:'#app',
        //4、将创建好的路由对象绑定到Vue实例对象上
        router:router
    });
</script>
</body>
</html>
```

**vue Router命名视图**

命名视图和具名插槽很像，就是让不同的出口显示不同的内容。

命名视图就是当路由地址被匹配的时候同时指定多个出口，并且每个出口中显示的内容不同。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<!--    1、导入vue Router-->
    <script src="vue-router.js"></script>
    <style>
        .lyy-active{
            background: green;
        }
    </style>
</head>
<body>
<div id="app">
<!--    路由匹配到的组件将被渲染到这里-->
    <router-view name="name1"></router-view>
    <router-view name="name2"></router-view>
</div>
<template id="one">
    <div>
        <p>我是第一个页面</p>
    </div>
</template>
<template id="two">
    <div>
        <p>我是第二个页面</p>
    </div>
</template>
<script>
    var one={
        template:'#one'
    };
    var two={
        template: '#two'
    };
    //2、定义切换的规则（定义路由规则）
    const routes = [
        //重定向
        {path:'/',redirect:'/one'},
        { path: '/one', components:
                {
                    name1:one,
                    name2:two
                }
        }
    ];
    //3、根据自定义的切换规则创建路由对象
    const router = new VueRouter({
        routes, // (缩写) 相当于 routes: routes
        linkActiveClass:'lyy-active'
    });
    var vm=new Vue({
        el:'#app',
        //4、将创建好的路由对象绑定到Vue实例对象上
        router:router
    });
</script>
</body>
</html>
```

**Watch属性**

watch属性是专门用于监听数据变化的，只要数据发生了变化，就会自动调用对应数据的回调方法。

watch属性监听路由变化：watch属性不仅可以监听数据的变化，还可以监听路由地址的变化。

我们可以通过watch来判断当前是从哪个界面跳转过来的。

**vue生命周期方法**

vue生命周期方法分类：

1. **创建**期间的生命周期方法

   beforeCreate：实例刚刚被创建出来，还没有初始化好data和methods属性。

   created：是我们最早可以访问vue实例中的data和methods的地方。

   beforeMount：vue已经编译好了最终模板，但是还没有将最终的模板渲染到界面上。

   mounted：vue已经完成了模板的渲染，可以看到界面上的数据了。

2. **运行**期间的生命周期方法

   beforeUpdate：只有保存的数据被修改了才会调用此方法，否则不会调用，此时数据已经更新成功，但是界面还没有同步好。

   updated：数据被修改了，界面也同步了修改的数据，数据和界面都同步更新了。

3. **销毁**期间的生命周期方法

   beforeDestory：当前组件即将被销毁。

   destoryed：组件完全被销毁之后会调用。

**vue的特殊特性**

在vue中不推荐我们去直接操作DOM元素，所以在vue中我们可以通过**ref**来获取DOM元素。

ref的使用：

1. 在需要被获取的元素上添加ref属性，如`<p ref="mypp">我是段落</p>`。
2. 在使用的地方通过this.$refs.XXX获取，如`this.$ref.mypp`。

**vue渲染组件的两种方式**

1. 先定义注册组件，然后在vue实例中当做标签来使用。不会覆盖vue实例控制区域，会将组件渲染到vue控制区域内。
2. 先定义注册组件，然后通过vue实例的render方法来渲染。会覆盖vue实例控制区域，会直接替换掉vue实例的控制区域。