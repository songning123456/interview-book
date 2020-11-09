#### 构建的vue-cli工程都到了哪些技术，它们的作用分别是什么？
| 名称 | 作用 | 
| :----- | :----- | 
|vue.js|vue-cli工程的核心，主要特点是<span class='forest-green'>双向数据绑定</span>和<span class='forest-green'>组件系统</span>。|
|vue-router|vue官方推荐使用的路由框架。|
|vuex|专为vue.js应用项目开发的状态管理器，主要用于维护vue组件间共用的一些变量和方法。|
|axios/fetch/ajax |用于发起GET或POST等http请求，基于Promise设计。|
|vux等|一个专为vue设计的移动端UI组件库|
|emit.js文件|用于vue事件机制的管理。|
|webpack|模块加载和vue-cli工程打包器。|


#### vue-cli工程常用的npm命令有哪些？
| 名称 | 解释 | 
| :----- | :----- | 
|<div style='width: 180px'>npm install</div>|下载node_modules资源包的命令。|
|<div style='width: 180px'>npm run dev</div>|启动vue-cli开发环境的npm命令。|
|<div style='width: 180px'>npm run build</div>|vue-cli生成生产环境部署资源的npm命令。|
|<div style='width: 180px'>npm run build --report</div>|在浏览器上自动弹出一个展示vue-cli工程打包后app.js、manifest.js、vendor.js文件里面所包含代码的页面。可以具此优化vue-cli生产环境部署的静态资源，提升页面的加载速度。|


#### 请说出vue-cli工程中每个文件夹和文件的用处？
![vue-cli目录](/images/Web/vue-cli.jpg)


| 名称 | 解释 | 
| :----- | :----- | 
|<div style='width: 130px'>build文件夹</div>|用于存放webpack相关配置和脚本。开发中仅偶尔使用到此文件夹下webpack.base.conf.js用于配置less、sass等css预编译库，或者配置一下UI库。|
|<div style='width: 130px'>config文件夹</div>|主要存放配置文件，用于区分开发环境、线上环境的不同。 常用到此文件夹下config.js配置开发环境的端口号、是否开启热加载或者设置生产环境的静态资源相对路径、是否开启gzip压缩、npm run build命令打包生成静态资源的名称和路径等。|
|<div style='width: 130px'>dist文件夹</div>|默认npm run build命令打包生成的静态资源文件，用于生产部署。|
|<div style='width: 130px'>node_modules</div>|存放npm命令下载的开发环境和生产环境的依赖包。|
|<div style='width: 130px'>src</div>|存放项目源码及需要引用的资源文件。|
|<div style='width: 130px'>src下assets</div>|存放项目中需要用到的资源文件，css、js、images等。|
|<div style='width: 130px'>src下components</div>|存放vue开发中一些公共组件：header.vue、footer.vue等。|
|<div style='width: 130px'>src下emit</div>|自己配置的vue集中式事件管理机制。|
|<div style='width: 130px'>src下router</div>|vue-router vue路由的配置文件。|
|<div style='width: 130px'>src下service</div>|自己配置的vue请求后台接口方法。|
|<div style='width: 130px'>src下page</div>|存在vue页面组件的文件夹。|
|<div style='width: 130px'>src下util</div>|存放vue开发过程中一些公共的.js方法。|
|<div style='width: 130px'>src下vuex</div>|存放vuex为vue专门开发的状态管理器。|
|<div style='width: 130px'>src下app.vue</div>|使用标签<route-view></router-view>渲染整个工程的.vue组件。|
|<div style='width: 130px'>src下main.js</div>|vue-cli工程的入口文件。|
|<div style='width: 130px'>index.html</div>|设置项目的一些meta头信息和提供<div id="app"></div>用于挂载vue节点。|
|<div style='width: 130px'>package.json</div>|用于node_modules资源部和启动、打包项目的npm命令管理。|


#### config文件夹下index.js的对于工程开发环境和生产环境的配置？
![config-index](/images/Web/config-index.jpeg)


| 名称 | 环境 | 功能 | 
| :----- | :----- | :----- | 
|build|<div style='width: 80px'>生产环境</div>|<span class='forest-green'>index：</span>配置打包后入口.html文件的名称以及文件夹名称；<br><span class='forest-green'>assetsRoot：</span>配置打包后生成的文件名称和路径；<br><span class='forest-green'>assetsPublicPath：</span>配置打包后.html引用静态资源的路径，一般要设置成 "./"；<br><span class='forest-green'>productionGzip：</span>是否开发gzip压缩，以提升加载速度。|
|dev|<div style='width: 80px'>开发环境</div>|<span class='forest-green'>port：</span>设置端口号；<br><span class='forest-green'>autoOpenBrowser：</span>启动工程时，自动打开浏览器；<br><span class='forest-green'>proxyTable：</span>vue设置的代理，用以解决跨域问题。|


#### 请你详细介绍一些package.json里面的配置？
| 名称 | 解释 | 
| :----- | :----- | 
|scripts|npm run xxx命令调用node执行的.js文件。|
|dependencies|生产环境依赖包的名称和版本号，即这些依赖包都会打包进生产环境的JS文件里面。|
|devDependencies|开发环境依赖包的名称和版本号，即这些依赖包只用于代码开发的时候，不会打包进生产环境js文件里面。|


#### vue.js的两个核心是什么？
| 名称 | 解释 | 
| :----- | :----- | 
|<div style='width: 180px'>数据驱动(双向数据绑定)</div>|Vue.js数据观测原理在技术实现上，利用的是ES5Object.defineProperty和存储器属性：<span class='forest-green'>getter和setter(所以只兼容IE9及以上版本)</span>，可称为基于依赖收集的观测机制。核心是<span class='forest-green'>VM</span>，即ViewModel，保证数据和视图的一致性。|
|<div style='width: 180px'>组件系统</div>|<span class='forest-green'>模板(template)：</span>模板声明了数据和最终展现给用户的DOM之间的映射关系；<br><span class='forest-green'>初始数据(data)：</span>一个组件的初始数据状态。对于可复用的组件来说，这通常是私有的状态；<br><span class='forest-green'>接受的外部参数(props)：</span>组件之间通过参数来进行数据的传递和共享；<br><span class='forest-green'>方法(methods)：</span>对数据的改动操作一般都在组件的方法内进行；<br><span class='forest-green'>生命周期钩子函数(lifecycle hooks)：</span>一个组件会触发多个生命周期钩子函数，最新2.0版本对于生命周期函数名称改动很大；<br><span class='forest-green'>私有资源(assets)：</span>vue.js当中将用户自定义的指令、过滤器、组件等统称为资源。一个组件可以声明自己的私有资源，私有资源只有该组件和它的子组件可以调用。|


#### 对于Vue是一套构建用户界面的渐进式框架的理解？
渐进式代表的含义是：没有多做职责之外的事。vue.js只提供了vue-cli生态中最核心的组件系统和双向数据绑定。像vuex、vue-router都属于围绕vue.js开发的库。


比如说，你要使用Angular，必须接受以下东西：


```
1. 必须使用它的模块机制；
2. 必须使用它的依赖注入；
3. 必须使用它的特殊形式定义组件(这一点每个视图框架都有，难以避免)。
```


所以Angular是带有比较强的排它性的，如果你的应用不是从头开始，而是要不断考虑是否跟其他东西集成，这些主张会带来一些困扰。


比如说，你要使用React，你必须理解：


```
1. 函数式编程的理念；
2. 需要知道什么是副作用；
3. 什么是纯函数；
4. 如何隔离副作用；
5. 它的侵入性看似没有Angular那么强，主要因为它是软性侵入。
```


Vue与React、Angular的不同是，但它是渐进的：


```
1. 你可以在原有大系统的上面，把一两个组件改用它实现，当jQuery用；
2. 也可以整个用它全家桶开发，当Angular用；
3. 还可以用它的视图，搭配你自己设计的整个下层用；
4. 你可以在底层数据逻辑的地方用OO和设计模式的那套理念；
5. 也可以函数式，都可以，它只是个轻量视图而已，只做了最核心的东西。
```


#### 请说出vue几种常用的指令？
| 指令 | 解释 | 
| :----- | :----- | 
|<div style='width: 80px'>v-if</div>|根据表达式的值的真假条件渲染元素。在切换时元素及它的数据绑定/组件被销毁并重建。|
|<div style='width: 80px'>v-show</div>|根据表达式之真假值，切换元素的display CSS属性。|
|<div style='width: 80px'>v-for</div>|循环指令，基于一个数组或者对象渲染一个列表，vue2.0以上必须需配合key值使用。|
|<div style='width: 80px'>v-bind</div>|动态地绑定一个或多个特性，或一个组件prop到表达式。|
|<div style='width: 80px'>v-on</div>|用于监听指定元素的DOM事件，比如点击事件、绑定事件监听器。|
|<div style='width: 80px'>v-model</div>|实现表单输入和应用状态之间的双向绑定。|
|<div style='width: 80px'>v-pre</div>|跳过这个元素和它的子元素的编译过程。可以用来显示原始Mustache标签，跳过大量没有指令的节点会加快编译。|
|<div style='width: 80px'>v-once</div>|只渲染元素和组件一次，随后的重新渲染。元素/组件及其所有的子节点将被视为静态内容并跳过，这可以用于优化更新性能。|


#### 请问v-if和v-show有什么区别？
共同点：v-if和v-show都是动态显示DOM元素。


| 区别 | v-if | v-show | 
| :----- | :----- | :----- | 
|<div style='width: 100px'>编译过程</div>|v-if真正的条件渲染，因为它会确保在切换过程中条件块内的事件监听器和子组件适当地被销毁和重建。|v-show的元素始终会被渲染并保留在DOM中。v-show只是简单地切换元素的CSS属性display。|
|<div style='width: 100px'>编译条件</div>|v-if是惰性的，如果在初始渲染时条件为假，则什么也不做。直到条件第一次变为真时，才会开始渲染条件块。|v-show不管初始条件是什么，元素总是会被渲染，并且只是简单地基于CSS进行切换。|
|<div style='width: 100px'>性能消耗</div>| v-if有更高的切换消耗。|v-show有更高的初始渲染消耗。|
|<div style='width: 100px'>应用场景</div>|v-if适合运行时条件很少改变时使用。|v-show适合频繁切换。|


#### vue常用的修饰符？
v-on指令常用修饰符：


```
.stop：调用event.stopPropagation()，禁止事件冒泡。
.prevent：调用event.preventDefault()，阻止事件默认行为。
.capture：添加事件侦听器时使用capture模式。
.self：只当事件是从侦听器绑定的元素本身触发时才触发回调。
.{keyCode | keyAlias}：只当事件是从特定键触发时才触发回调。
.native：监听组件根元素的原生事件。
.once：只触发一次回调。
.left：(2.2.0)只当点击鼠标左键时触发。
.right：(2.2.0)只当点击鼠标右键时触发。
.middle：(2.2.0)只当点击鼠标中键时触发。
.passive：(2.3.0)以{passive: true}模式添加侦听器。
```


**注意：**如果是在自己封装的组件或者是使用一些第三方的UI库时，会发现并不起效果，这时就需要用.native修饰符了，如：


```html
<el-input v-model="inputName" placeholder="搜索你的文件" @keyup.enter.native="searchFile(params)"></el-input>
```


v-bind指令常用修饰符：


```
.prop：被用于绑定DOM属性(property)。(差别在哪里？)
.camel：(2.1.0+)将kebab-case特性名转换为camelCase。(从2.1.0开始支持)
.sync：(2.3.0+)语法糖，会扩展成一个更新父组件绑定值的v-on侦听器。
```


v-model指令常用修饰符：


```
.lazy：取代input监听change事件。
.number：输入字符串转为数字。
.trim：输入首尾空格过滤。
```


#### v-on可以监听多个方法吗？
v-on可以监听多个方法，例如：


```html
<input type="text" :value="name" @input="onInput" @focus="onFocus" @blur="onBlur" />
```


但是同一种事件类型的方法，vue-cli工程会报错，例如：


```html
<a href="javascript:;" @click="methodsOne" @click="methodsTwo"></a>
```


#### vue中key值的作用？
<span class="forest-green">key值：</span>用于管理可复用的元素。因为vue会尽可能高效地渲染元素，通常会复用已有元素而不是从头开始渲染。这么做使vue变得非常快，但是这样也不总是符合实际需求。2.2.0+的版本里，当在组件中使用v-for时，key现在是必须的。


例如，如果你允许用户在不同的登录方式之间切换：


```html
<template v-if="loginType === 'username'">
  <label>Username</label>
  <input placeholder="Enter your username">
</template>

<template v-else>
  <label>Email</label>
  <input placeholder="Enter your email address">
</template>
```


那么在上面的代码中切换loginType，loginType将不会清除用户已经输入的内容。因为两个模板使用了相同的元素，‘</span>’不会被替换掉，仅仅是替换了它的placeholder。


这样也不总是符合实际需求，所以vue为你提供了一种方式来表达这两个元素是完全独立的，不要复用它们。只需添加一个具有唯一值的key属性即可：


```html
<template v-if="loginType === 'username'">
  <label>Username</label>
  <input placeholder="Enter your username" key="username-input">
</template>

<template v-else>
  <label>Email</label>
  <input placeholder="Enter your email address" key="email-input">
</template>
```


现在，每次切换时，输入框都将被<span class="forest-green">重新渲染</span>。


#### vue-cli如何进行工程升级？
<span class="forest-green">前言：</span>此命令谨慎使用，实际开发中如需升级建议直接使用vue-cli脚手架搭建，只需要了解即可！


```
// 首先安装插件，建议用npm源安装，测试时用cnpm安装未下载成功：
npm install npm-check-updates -g
// 然后在待升级工程的目录结构下，命令行输入：
ncu
// 然后升级所有版本，命令行输入：
ncu -a
// 再输入：
npm install
```


执行效果图


![npm-check-updates](/images/Web/npm-check-updates.jpeg)
