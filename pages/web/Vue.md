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


#### 请说出Vue几种常用的指令？
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


#### Vue常用的修饰符？
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


#### Vue中key值的作用？
<span class="forest-green">key值：</span>用于管理可复用的元素。因为Vue会尽可能高效地渲染元素，通常会复用已有元素而不是从头开始渲染。这么做使Vue变得非常快，但是这样也不总是符合实际需求。2.2.0+的版本里，当在组件中使用v-for时，key现在是必须的。


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


#### vue事件中如何使用event对象？
注意在事件中要使用<span class="forest-green">$</span>符号


```html
<a href="javascript:void(0);" data-id="12" @click="showEvent($event)">event</a>
```


```javascript
function showEvent(event){
    //获取自定义data-id
	console.log(event.target.dataset.id);
   //阻止事件冒泡
    event.stopPropagation(); 
    //阻止默认
    event.preventDefault()
}
```


#### 什么是$nextTick？
简单回答：因为Vue的异步更新队列，$nextTick是用来知道什么时候DOM更新完成的。


详细解读：我们先来看这样一个场景：有一个div，默认用v-if将它隐藏，点击一个按钮后，改变v-if的值，让它显示出来，同时拿到这个div的文本内容。如果v-if的值是false，直接去获取div内容是获取不到的，因为此时div还没有被创建出来，那么应该在点击按钮后，改变v-if的值为true，div才会被创建，此时再去获取，示例代码如下：


```html
<div id="app">
    <div id="div" v-if="showDiv">这是一段文本</div>
    <button @click="getText">获取div内容</button>
</div>
```


```javascript
let app = new Vue({
    el : "#app",
    data:{
        showDiv : false
    },
    methods:{
        getText:function(){
            this.showDiv = true;
            let text = document.getElementById('div').innnerHTML;
             console.log(text);
        }
    }
})
```


这段代码并不难理解，但是运行后在控制台会抛出一个错误：Cannot read property 'innnerHTML of null，意思就是获取不到div元素。这里就涉及Vue一个重要的概念：异步更新队列。


```
// 异步更新队列
Vue在观察到数据变化时并不是直接更新DOM，而是开启一个队列，并缓冲在同一个事件循环中发生的所以数据改变。在缓冲时会去除重复数据，从而避免不必要的计算和DOM操作。然后，在下一个事件循环tick中，Vue刷新队列并执行实际(已去重的)工作。所以如果你用一个for循环来动态改变数据100次，其实它只会应用最后一次改变，如果没有这种机制，DOM就要重绘100次，这固然是一个很大的开销。
```


Vue会根据当前浏览器环境优先使用原生的Promise.then和MutationObserver，如果都不支持，就会采用setTimeout代替。知道了Vue异步更新DOM的原理，上面示例的报错也就不难理解了。事实上，在执行this.showDiv=true时，div仍然还是没有被创建出来，直到下一个Vue事件循环时，才开始创建。$nextTick就是用来知道什么时候DOM更新完成的，所以上面的示例代码需要修改为：


```javascript
let app = new Vue({
    el : "#app",
    data:{
        showDiv : false
    },
    methods:{
        getText:function(){
            this.showDiv = true;
            this.$nextTick(function(){
                  let text = document.getElementById('div').innnerHTML;
                 console.log(text);  
            });
        }
    }
})
```


这时再点击事件，控制台就打印出div的内容“这是一段文本”了。


理论上，我们应该不用去主动操作DOM，因为Vue的核心思想就是数据驱动DOM，但在很多业务里，我们避免不了会使用一些第三方库，比如popper.js、swiper等，这些基于原生javascript的库都有创建和更新及销毁的完整生命周期，与Vue配合使用时，就要利用好$nextTick。


#### Vue组件中data为什么必须是函数？
```javascript
// 为什么data函数里面要return一个对象
export default {
    data() {
        return {  // 返回一个唯一的对象，不要和其他组件共用一个对象进行返回
            menu: MENU.data,
            poi: POILIST.data
        }
    }
}
```


因为一个组件是可以共享的，但他们的data是私有的，所以每个组件都要return一个新的data对象，返回一个唯一的对象，不要和其他组件共用一个对象。


```javascript
Vue.component('my-component', {
  template: '<div>OK</div>',
  data() {
    return {} // 返回一个唯一的对象，不要和其他组件共用一个对象进行返回
  },
})
```


这个操作是一个简易操作，实际上，它首先需要创建一个组件构造器；然后注册组件；注册组件的本质其实就是建立一个组件构造器的引用；使用组件才是真正创建一个组件实例。所以，注册组件其实并不产生新的组件类，但会产生一个可以用来实例化的新方式。


理解这点之后，再理解js的原型链：


```javascript
let MyComponent = function() {};
MyComponent.prototype.data = {
  a: 1,
  b: 2,
};
```


上面是一个虚拟的组件构造器，真实的组件构造器方法很多。


```javascript
let component1 = new MyComponent();
let component2 = new MyComponent();
```


上面实例化出来两个组件实例，也就是通过调用，创建的两个实例。


```javascript
component1.data.a === component2.data.a; // true
component1.data.b = 5;
component2.data.b; // 5
```


可以看到上面代码中最后三句，这就比较坑爹了，如果两个实例同时引用一个对象，那么当你修改其中一个属性的时候，另外一个实例也会跟着改。这怎么可以，两个实例应该有自己各自的域才对。所以，需要通过下面方法来进行处理：


```javascript
let MyComponent = function() {
    this.data = this.data()
};
MyComponent.prototype.data = function() {
    return {
        a: 1,
        b: 2,
    }
};
```


这样每一个实例的data属性都是独立的，不会相互影响了。所以，你现在知道为什么Vue组件的data必须是函数了吧。这都是因为js本身的特性带来的，跟Vue本身设计无关。


#### v-for与v-if的优先级？
当它们处于同一节点，v-for的优先级比v-if更高，这意味着v-if将分别重复运行于每个v-for循环中。当你想为仅有的一些项渲染节点时，这种优先级的机制会十分有用，如下：


```
<li v-for="todo in todos" v-if="!todo.isComplete">
    todo
</li>
```


上面的代码只传递了未完成的todos。而如果你的目的是有条件地跳过循环的执行，那么可以将v-if置于外层元素(或template)上。如：


```
<ul v-if="todos.length">
    <li v-for="todo in todos">
        todo
    </li>
</ul>
<p v-else>No todos left!</p>
```


#### Vue中子组件调用父组件的方法？
通过v-on监听和$emit触发来实现：<br>
1. 在父组件中通过v-on监听当前实例上的自定义事件；<br>
2. 在子组件中通过'$emit'触发当前实例上的自定义事件。


父组件：


```html
<template>
    <div class="fatherPageWrap">
        <h1>这是父组件</h1>
        <!-- 引入子组件，v-on监听自定义事件 -->
        <emitChild v-on:emitMethods="fatherMethod"></emitChild>
    </div>
</template>

<script type="text/javascript">
    import emitChild from '@/page/children/emitChild.vue';
	export default{
		data () {
		    return {}
		},
		components : {
                    emitChild
		},
		methods : {
		    fatherMethod(params){
                alert(JSON.stringify(params));
            }
		}
	}
</script>
```


子组件：


```html
<template>
    <div class="childPageWrap">
        <h1>这是子组件</h1>
    </div>
</template>

<script type="text/javascript">
    export default{
		data () {
		   return {}
		},
		mounted () {
            //通过 emit 触发
            this.$emit('emitMethods',{"name" : 123});
		}
	}
</script>
```


结果：子组件会调用父组件的fatherMethod()方法，该并且会alert传递过去的参数：{"name":123}。


#### Vue中keep-alive组件的作用？
<span class='forest-green'>keep-alive</span>主要用于保留组件状态或避免重新渲染。


比如有一个列表页面和一个详情页面，那么用户就会经常执行打开详情=>返回列表=>打开详情这样的话列表和详情都是一个频率很高的页面，那么就可以对列表组件使用keep-alive进行缓存，这样用户每次返回列表的时候，都能从缓存中快速渲染，而不是重新渲染。


**属性：**


<span class='forest-green'>include：</span>字符串或正则表达式。只有匹配的组件会被缓存。<br>
<span class='forest-green'>exclude：</span>字符串或正则表达式。任何匹配的组件都不会被缓存。


**用法：**


包裹动态组件时，会缓存不活动的组件实例，而不是销毁它们。和transition相似，keep-alive是一个抽象组件：它自身不会渲染一个DOM元素，也不会出现在父组件链中。


当组件在keep-alive内被切换，在2.2.0及其更高版本中，activated和deactivated生命周期将会在树内的所有嵌套组件中触发。


```html
<!-- 基本 -->
<keep-alive>
    <component :is="view"></component>
</keep-alive>

<!-- 多个条件判断的子组件 -->
<keep-alive>
    <comp-a v-if="a > 1"></comp-a>
    <comp-b v-else></comp-b>
</keep-alive>

<!-- 和 `<transition>` 一起使用 -->
<transition>
    <keep-alive>
        <component :is="view"></component>
    </keep-alive>
</transition>
```


注意：keep-alive是用在其一个直属的子组件被开关的情形。如果你在其中有v-for则不会工作。如果有上述的多个条件性的子元素，keep-alive要求同时只有一个子元素被渲染。


**include和exclude属性的使用：**


(2.1.0新增)include和exclude属性允许组件有条件地缓存。二者都可以用逗号分隔字符串、正则表达式或一个数组来表示：


```html
<!-- 逗号分隔字符串 -->
<keep-alive include="a,b">
    <component :is="view"></component>
</keep-alive>

<!-- 正则表达式 (使用 `v-bind`) -->
<keep-alive :include="/a|b/">
    <component :is="view"></component>
</keep-alive>

<!-- 数组 (使用 `v-bind`) -->
<keep-alive :include="['a', 'b']">
    <component :is="view"></component>
</keep-alive>
```


匹配首先检查组件自身的name选项，如果name选项不可用，则匹配它的局部注册名称(父组件components选项的键值)。匿名组件不能被匹配。


不会在函数式组件中正常工作，因为它们没有缓存实例。