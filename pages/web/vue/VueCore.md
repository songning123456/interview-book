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


#### Vue中如何编写可复用的组件？
在编写组件的时候，时刻考虑组件是否可复用是有好处的。一次性组件跟其他组件紧密耦合没关系，但是可复用组件一定要定义一个清晰的公开接口。


Vue.js组件API来自三部分：<span class='forest-green'>prop</span>、<span class='forest-green'>事件</span>、<span class='forest-green'>slot</span>：<br>
1. prop允许外部环境传递数据给组件，在vue-cli工程中也可以使用vuex等传递数据；<br>
2. 事件允许组件触发外部环境的action；<br>
3. slot允许外部环境将内容插入到组件的视图结构内。


```html
<my-component :foo="bar" :bar="qux"
    //子组件调用父组件方法
    @event-a="doThis" @event-b="doThat">
    <!-- content -->
<img slot="icon" src="..." />
<p slot="main-text">Hello!</p>
</my-component>
```


#### 什么是Vue生命周期和生命周期钩子函数？
Vue的生命周期是：Vue实例从创建到销毁，也就是从<span class='forest-green'>开始创建</span>、<span class='forest-green'>初始化数据</span>、<span class='forest-green'>编译模板</span>、<span class='forest-green'>挂载Dom</span>、<span class='forest-green'>渲染</span>、<span class='forest-green'>更新</span>、<span class='forest-green'>渲染</span>、<span class='forest-green'>卸载</span>等一系列过程。


在这个过程中也会运行一些叫做生命周期钩子的函数，这给了用户在不同阶段添加自己的代码的机会。


#### Vue生命周期钩子函数有哪些？
| 生命周期钩子函数(11个) | 类型 | 详细 |
| :----- | :----- | :----- | 
|<div style='width: 210px'>beforeCreate</div>|Function|在实例初始化之后，数据观测(data observer)和event/watcher事件配置之前被调用。|
|<div style='width: 210px'>created</div>|Function|在实例创建完成后被立即调用。在这一步，实例已完成以下的配置：数据观测(data observer)，属性和方法的运算，watch/event事件回调。然而，挂载阶段还没开始，$el属性目前不可见。|
|<div style='width: 210px'>beforeMount</div>|Function|在挂载开始之前被调用：相关的render函数首次被调用。|
|<div style='width: 210px'>mounted</div>|Function|el被新创建的vm.$el替换，并挂载到实例上去之后调用该钩子。如果root实例挂载了一个文档内元素，当mounted被调用时vm.$el也在文档内。|
|<div style='width: 210px'>beforeUpdate</div>|Function|数据更新时调用，发生在虚拟DOM打补丁之前。这里适合在更新之前访问现有的DOM，比如手动移除已添加的事件监听器。<span class='forest-green'>该钩子在服务器端渲染期间不被调用</span>，因为只有初次渲染会在服务端进行。|
|<div style='width: 210px'>updated</div>|Function|由于数据更改导致的虚拟DOM重新渲染和打补丁，在这之后会调用该钩子。|
|<div style='width: 210px'>activated</div>|Function|keep-alive组件激活时调用，<span class='forest-green'>该钩子在服务器端渲染期间不被调用</span>。|
|<div style='width: 210px'>deactivated</div>|Function|keep-alive组件停用时调用，<span class='forest-green'>该钩子在服务器端渲染期间不被调用</span>。|
|<div style='width: 210px'>beforeDestroy</div>|Function|实例销毁之前调用。在这一步，实例仍然完全可用。<span class='forest-green'>该钩子在服务器端渲染期间不被调用</span>。|
|<div style='width: 210px'>destroyed</div>|Function|Vue实例销毁后调用。调用后，Vue实例指示的所有东西都会解绑定，所有的事件监听器会被移除，所有的子实例也会被销毁。<span class='forest-green'>该钩子在服务器端渲染期间不被调用</span>。|
|<div style='width: 210px'>errorCaptured(2.5.0+新增)</div>|(err: Error, vm: Component, info: string) => ?boolean|当捕获一个来自子孙组件的错误时被调用。此钩子会收到三个参数：错误对象、发生错误的组件实例以及一个包含错误来源信息的字符串。此钩子可以返回false以阻止该错误继续向上传播。|


mounted、updated不会承诺所有的子组件也都一起被挂载。如果你希望等到整个视图都渲染完毕，可以用<span class='forest-green'>vm.$nextTick替换掉mounted、updated</span>：


```html
updated: function () {
    this.$nextTick(function () {
        // Code that will run only after the
        // entire view has been re-rendered
    })
}
```


http请求建议在<span class='forest-green'>created</span>生命周期内发出。


![lifecycle](/images/Web/lifecycle.png)


#### Vue如何监听键盘事件中的按键？
**按键修饰符**


在监听键盘事件时，我们经常需要检查常见的键值。Vue允许为v-on在监听键盘事件时添加按键修饰符：


```html
<!-- 只有在 `keyCode` 是 13 时调用 `vm.submit()` -->
<input v-on:keyup.13="submit">
```


记住所有的keyCode比较困难，所以Vue为最常用的按键提供了别名：


```html
<input v-on:keyup.enter="submit">
<!-- 缩写语法 -->
<input @keyup.enter="submit">
```


全部的按键别名：.enter、.tab、.delete(捕获“删除”和“退格”键)、.esc、.space、.up、.down、.left、.right。


可以通过全局config.keyCodes对象自定义按键修饰符别名：


```javascript
// 可以使用 `v-on:keyup.f1`
Vue.config.keyCodes.f1 = 112;
```


**系统修饰键(2.1.0新增)**


可以用.ctrl、.alt、.shift、.meta修饰符来实现仅在按下相应按键时才触发鼠标或键盘事件的监听器。


```
注意：在Mac系统键盘上，meta对应command键(⌘)。在Windows系统键盘meta对应Windows徽标键(⊞)。在Sun操作
系统键盘上，meta对应实心宝石键(◆)。在其他特定键盘上，尤其在MIT和Lisp机器的键盘、以及其后继产品，比如
Knight键盘、space-cadet键盘，meta被标记为“META”。在Symbolics键盘上，meta被标记为“META”或者“Meta”。
```


```html
// e.g
<!-- Alt + C -->
<input @keyup.alt.67="clear">

<!-- Ctrl + Click -->
<div @click.ctrl="doSomething">Do something</div>
```


```html
请注意修饰键与常规按键不同，在和keyup事件一起用时，事件触发时修饰键必须处于按下状态。换句话说，
只有在按住ctrl的情况下释放其它按键，才能触发keyup.ctrl。而单单释放ctrl也不会触发事件。如果你
想要这样的行为，请为ctrl换用keyCode：keyup.17。
```


**.exact修饰符(2.5.0新增)**


.exact修饰符允许你控制由精确的系统修饰符组合触发的事件。


```html
<!-- 即使 Alt 或 Shift 被一同按下时也会触发 -->
<button @click.ctrl="onClick">A</button>

<!-- 有且只有 Ctrl 被按下的时候才触发 -->
<button @click.ctrl.exact="onCtrlClick">A</button>

<!-- 没有任何系统修饰符被按下的时候才触发 -->
<button @click.exact="onClick">A</button>
```


**鼠标按钮修饰符(2.2.0新增)**


.left、.right、.middle这些修饰符会限制处理函数仅响应特定的鼠标按钮。


#### Vue更新数组时触发视图更新的方法？
**变异方法**


Vue包含一组观察数组的变异方法，所以它们也将会触发视图更新。这些方法如下：<span class='forest-green'>push()</span>、<span class='forest-green'>pop()</span>、<span class='forest-green'>shift()</span>、<span class='forest-green'>unshift()</span>、<span class='forest-green'>splice()</span>、<span class='forest-green'>sort()</span>、<span class='forest-green'>reverse()</span>。


**替换数组**


例如：filter()、concat()和slice()。这些不会改变原始数组，但总是返回一个新数组。当使用这些非变异方法时，可以用新数组替换旧数组：


```javascript
example1.items = example1.items.filter(function (item) {
    return item.message.match(/Foo/)
})
```


你可能认为这将导致Vue丢弃现有DOM并重新渲染整个列表。幸运的是，事实并非如此。Vue为了使得DOM元素得到最大范围的重用而实现了一些智能的、启发式的方法，所以用一个含有相同元素的数组去替换原来的数组是非常高效的操作。


注意事项：由于JavaScript的限制，Vue不能检测以下变动的数组：<br>
1. 当你利用索引直接设置一个项时，例如：vm.items[indexOfItem] = newValue；<br>
2. 当你修改数组的长度时，例如：vm.items.length = newLength。


```javascript
// e.g
let vm = new Vue({
  data: {
    items: ['a', 'b', 'c']
  }
});
vm.items[1] = 'x'; // 不是响应性的
vm.items.length = 2 // 不是响应性的
```


为了解决第一类问题，以下两种方式都可以实现和vm.items[indexOfItem] = newValue 相同的效果，同时也将触发状态更新：


```javascript
// Vue.set
Vue.set(vm.items, indexOfItem, newValue);

// Array.prototype.splice
vm.items.splice(indexOfItem, 1, newValue);
```


你也可以使用vm.$set实例方法，该方法是全局方法Vue.set的一个别名：


```javascript
vm.$set(vm.items, indexOfItem, newValue);
```


为了解决第二类问题，你可以使用splice：


```javascript
vm.items.splice(newLength);
```


#### Vue中对象更改检测的注意事项？
由于JavaScript的限制，<span class='forest-green'>Vue不能检测对象属性的添加或删除</span>：


```javascript
let vm = new Vue({
    data: {
        a: 1
    }
});
// `vm.a` 现在是响应式的

vm.b = 2
// `vm.b` 不是响应式的
```


对于已经创建的实例，Vue不能动态添加根级别的响应式属性。但是，可以使用Vue.set(object, key, value)方法向嵌套对象添加响应式属性。例如，对于：


```javascript
let vm = new Vue({
    data: {
        userProfile: {
            name: 'Anika'
        }
    }
})
```


你可以添加一个新的age属性到嵌套的userProfile对象：


```javascript
Vue.set(vm.userProfile, 'age', 27);
```


你还可以使用vm.$set实例方法，它只是全局Vue.set的别名：


```javascript
vm.$set(vm.userProfile, 'age', 27);
```


有时你可能需要为已有对象赋予多个新属性，比如使用Object.assign()或_.extend()。在这种情况下，你应该用两个对象的属性创建一个新的对象。所以，如果你想添加新的响应式属性，不要像这样：


```javascript
Object.assign(vm.userProfile, {
    age: 27,
    favoriteColor: 'Vue Green'
});
```


应该这样做：


```javascript
vm.userProfile = Object.assign({}, vm.userProfile, {
    age: 27,
    favoriteColor: 'Vue Green'
});
```


#### 如何解决非工程化项目，网速慢时初始化页面闪动问题？
使用v-cloak指令，v-cloak不需要表达式，它会在Vue实例结束编译时从绑定的HTML元素上移除，经常和CSS的display: none配合使用。


```html
<div id="app" v-cloak>
    {{ message }}
</div>
<script>
let app = new Vue({
    el:"#app",
    data:{
        message:"这是一段文本"
    }
})
</script>
```


这时虽然已经加了指令v-cloak，但其实并没有起到任何作用。当网速较慢、Vue.js文件还没加载完时，在页面上会显示{{message}}的字样，直到Vue创建实例、编译模版时，DOM才会被替换，所以这个过程屏幕是有闪动的。只要加一句CSS就可以解决这个问题了：


```css
[v-cloak]{
    display: none;
}
```


在一般情况下，v-cloak是一个解决初始化慢导致页面闪动的最佳实践，对于简单的项目很实用。


#### v-for产生的列表，如何实现active样式的切换？
通过设置当前currentIndex实现：


```html
<template>
	<div class="toggleClassWrap">
	 <ul>
		<li @click="currentIndex = index" v-bind:class="{clicked: index === currentIndex}" v-for="(item, index) in desc" :key="index">
			<a href="javascript:;">{{item.ctrlValue}}</a>
		</li>
	</ul>
	</div>
</template>
<script type="text/javascript">
	export default{
		data () {
			return {
				desc:[{
					ctrlValue:"test1"
				},{
					ctrlValue:"test2"
				},{
					ctrlValue:"test3"
				},{
					ctrlValue:"test4"
				}],
				currentIndex:0
			}
		}
	}
</script>
<style type="text/css" lang="less">
.toggleClassWrap{
	.clicked{
		color:red;
	}
}
</style>
```


#### v-model语法糖在组件上的使用？
需要实现效果：如果在一个页面中我们需要引入一个弹窗组件，点击按钮a显示弹窗，然后点击弹窗的关闭按钮，关闭弹窗，用v-model实现。使用v-model来进行双向数据绑定的时：


```html
<input v-model="something">
```


仅仅是一个语法糖：


```html
<input v-bind:value="something" v-on:input="something = $event.target.value">
```


所以在组件中使用的时候，相当于下面的简写：


```html
<custom v-bind:value="something" v-on:input="something = $event.target.value"></custom>
```


所以要组件的v-model生效，它必须：<br>
1. 接受一个value属性；<br>
2. 在有新的value时触发input事件。


使用示例：
 

```html
<template>
	<div class="toggleClassWrap">
	    <modelVue v-if="ifShow" v-model="ifShow"></modelVue>
    </div>
</template>
<script type="text/javascript">
	import modelVue from '../../components/model.vue'
	export default{
		data () {
			return {
				ifShow:true,
			}
		},
		components : {
			modelVue
		}
	}
</script>
```


model.vue组件


```html
<template>
    <div id="showAlert">
        <div>showAlert 内容</div>
        <button class="close" @click="close">关闭</button>
    </div>
</template>

<script>
    export default{
        props:{
            value:{
                type:Boolean,
                default:false,
            }
        },
        data(){
            return{}
        },
        mounted(){
        },
        methods:{
            close(){
                this.$emit('input',false);//传值给父组件, 让父组件监听到这个变化
            }
        },
    }
</script>

<style scoped>
    .close{
        background:red;
        color:white;
    }
</style>
```