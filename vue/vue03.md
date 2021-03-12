**Vue cli**

1. 安装vue cli
2. 创建一个项目`vue create my-project`
3. 进入到项目中 `cd my-project`
4. 打包好运行项目 `npm run serve`
5. 打包项目：`npm run build`打包生成一个dist目录

vue cli是vue官方提供的脚手架工具，默认已经帮我们搭建好了一套利用webpack管理的vue的项目结果。

**通过vue-cli生成的项目结构解读**

node_moduled文件夹：存储了依赖的相关的包。

public文件夹：任何放置在public文件夹中的静态资源都会被简单的复制。而不经过webpack，需要使用绝对路径来引用它们，一般用于存储一些永远不会改变的静态资源或者webpack不支持的第三方库。

src文件夹：代码文件夹

​	assets文件夹：存储项目中自己的一些静态文件（图片/字体等）。

​	components文件夹：存储项目中的自定义组件（小组件，公共组件）。

​	views文件夹：存储项目中的自定义组件（大组件，页面组件，路由级别组件）。

​	router文件夹：存储VueRouter相关文件。

​	store文件夹：存储Vuex相关文件。

​	App.vue：根组件，相当于vue的控制区域。

​	main.js：整个项目的入口，在这里创建我们的vue实例，绑定我们的VueRouter、Vuex，渲染vue控制的区域。

App.vue是利用自定义的组件来替换掉Vue实例的控制区域（index.html)，即以后我们要添加内容就不用回到index中，index.html文件就不会改变，以后我们要添加内容就直接在App.vue文件中进行添加和修改。即App.vue里面的内容就成为了项目的首页。

在.vue中可以导入其他的.vue文件，进行组件的使用。

**如何在vue-cli中使用Vuex状态管理？**

**如何在vue-cli中使用VueRouter路由？**

**如何配置vue-cli创建项目的webpack配置**

默认情况下提供vue-cli创建的项目已经自动为我们配置好了webpack。

但是有时候，我们想要修改输出目录的名称，想增加一些插件等，但是vue-cli项目中又没有webpack配置文件，那么我们应该如何修改或增加webpack配置呢？

我们可以通过新建**vue.config.js**的方式来修改配置。

我们可以在vue.config.js中的configureWebpack属性来新增webpack配置。

Vue-CLI为了方便起见对webpack原有的属性进行了一层封装，如果我们需要修改webpack的配置，那么我们可以在项目中新建个vue.config.js的文件，然后去查询Vue-CLI的封装是否能够满足我们的需求，如果可以满足我们的需求，那么就使用Vue-CLI封装的属性来修改webpack的配置。

```javascript
module.exports = {
//  可以在这里面对webpack进行额外的配置
//  修改打包后的目录文件夹名称
  outputDir: 'bundle'
}
```

如果vue-cli封装的属性不能够满足我们的需求，那么我们可以通过**configureWebpack**的属性来编写原生的webpack配置。

```javascript
const webpack = require('webpack')
module.exports = {
//  可以在这里面对webpack进行额外的配置
//  修改打包后的目录文件夹名称
  outputDir: 'bundle',
  configureWebpack: {
  //  就可以在这个对象中编写原生的webpack配置
    plugins: [
      new webpack.BannerPlugin({
        banner: 'lyy'
      })
    ]
  }
}

```

**ElementUI**

ElementUI是饿了么前端团队推出的一款基于**Vue**的桌面端UI框架。

和bootstrap一样，对原生的HTML标签进行了封装，进行了美化，让我们可以更专注于业务逻辑而不是UI界面。

1. 在main.js中导入elementUI和elementUI的CSS文件；
2. 告诉vue，我们需要在项目中使用elementUI；
3. 在组件中对elementUI进行使用；

```javascript
import Vue from 'vue'
import App from './App'
// 1、导入elementUI所需要的文件
import ElementUI from 'element-ui'
import 'element-ui/lib/theme-chalk/index.css'
import store from './store/index'
import router from './router/index'
// 2、告诉vue，我们需要在项目中使用elementUI
Vue.use(ElementUI)
/* eslint-disable no-new */
new Vue({
  el: '#app',
  store: store,
  router: router,
  // 将导入的组件渲染到页面上，这就可以用自定义的组件来替换掉index中的内容
  render: c => c(App)
})
```

上面导致项目的体积过大，笨重，如何优化？

为了解决这个问题，elementUI退出了**按需导入，按需打包**，也就是只会讲我们用到的组件打包在我们的项目中，没有用到的组件不会被打包。

1. 修改babel.config.js文件
2. 按需导入

```javascript
module.exports = {
  presets: [
    ['@vue/cli-plugin-babel/preset', { modules: false }]
  ],
  plugins: [
    [
      'component',
      {
        libraryName: 'element-ui',
        styleLibraryName: 'theme-chalk'
      }
    ]
  ]
}

```

```javascript
import Vue from 'vue'
import App from './App'
// 1、按需导入elementUI所需要的文件
import button from 'element-ui'
import store from './store/index'
import router from './router/index'
// 2、告诉vue，我们需要在项目中使用button
Vue.use(button)
/* eslint-disable no-new */
new Vue({
  el: '#app',
  store: store,
  router: router,
  // 将导入的组件渲染到页面上，这就可以用自定义的组件来替换掉index中的内容
  render: c => c(App)
})

```

**vue-MintUI**

MintUI是饿了么前端团队推出的一款基于**vue**的**移动端UI**框架。

**Vant**

Vant是有赞前端开发团队推出的一款基于Vue的移动UI框架。适用于移动端的**电商构建**项目。

