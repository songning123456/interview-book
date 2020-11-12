#### Vue打包命令是什么？
vue-cli生成生产环境部署资源的npm命令：


```html
npm run build 
```


用于查看vue-cli生产环境部署资源文件大小的npm命令：


```html
npm run build --report
```


![report](/images/Web/report.jpg)


在浏览器上自动弹出一个展示vue-cli工程打包后app.js、manifest.js、vendor.js文件里面所包含代码的页面。可以具此优化vue-cli生产环境部署的静态资源，提升页面的加载速度。


#### Vue打包后会生成哪些文件？


默认生成dist文件夹：


![dist](/images/Web/dist.jpg)


生产index.html单页面文件将组件中的css编译合并成一个app.[hash].css的文件，js则在合并后又拆解成了3个文件：


| js文件 | 解释 | 
| :----- | :----- | 
|app.[hash].js|包含了所有components中的js代码。|
|vendor.[hash].js|包含了生产环境所有引用的node_modules中的代码。|
|mainfest.[hash].js|则包含了webpack运行环境及模块化所需的js代码。|
|0.[hash].js|vue-router使用了按需加载生产的js文件。|


这样拆分的好处是：每块组件修改重新编译后不影响其他未修改的js文件的hash值，这样能够最大限度地使用缓存，减少HTTP的请求数。


#### 如何配置Vue打包生成文件的路径？
![filePath](/images/Web/filePath.jpg)


在config/index.js里面配置Vue生产环境打包生产文件的路径：


| 属性 | 解释 | 
| :----- | :----- | 
|index|配置生成index.html文件的路径地址。|
|assetsRoot|配置生成static静态资源文件们的路径地址。|


#### Vue如何优化首屏加载速度？
问题描述：在Vue项目中，引入到工程中的所有js、css文件，编译时都会被打包进vendor.js，浏览器在加载该文件之后才能开始显示首屏。若是引入的库众多，那么vendor.js文件体积将会相当的大，影响首屏的体验。


解决方法是：将引用的外部js、css文件剥离开来，不编译到vendor.js中，而是用资源的形式引用，这样浏览器可以使用多个线程异步将vendor.js、外部的js等加载下来，达到加速首开的目的。外部的库文件，可以使用CDN资源，或者别的服务器资源等。


几种常用的优化方法：


* **使用npm run build --report命令进行大文件定位。**


我们可以使用webpack可视化插件Webpack Bundle Analyzer查看工程js文件大小，然后有目的的解决过大的js文件。


```html
npm run build --report
```


* **路由的按需加载。**


* **将打包生成后index.html页面里面的JS文件引入方式放在body的最后。**


默认情况下，build后的index.html中，js的引入是在head中，使用html-webpack-plugin插件，将inject的值改为body。就可以将js引入放到body最后。首先下载插件：(一般vue-cli项目里默认有，可以package.json里面检查是否含有。)


```html
npm install html-webpack-plugin@2 --save-dev
```


在build文件夹下webpack.base.conf.js配置：


```javascript
let htmlWebpackPlugin = require('html-webpack-plugin');
module.exports = {
    entry: './src/script/main.js',
    output: {
        filename: 'js/bundle.js',
        path: path.resolve(__dirname, 'dist')
    },
    plugins: [
        new htmlWebpackPlugin({
            inject: 'body'
        })
    ]
}
```


即在plugins里面加上htmlWebpackPlugin插件。


* **用文档的cdn文件代替npm安装包。**


用文档的cdn文件代替，而不用打包到vender里面去。具体的做法是：


1.在index.html里面引入依赖库js文件。


```html
// index.html
<script src="https://cdn.bootcss.com/vue/2.3.3/vue.min.js"></script>
<script src="https://cdn.bootcss.com/axios/0.16.2/axios.min.js"></script>
```


2.在main.js里面去掉第三方js的import,因为在第一步已经通过script标签引用进来了。


3.把第三方库的js文件从打包文件里去掉。即在build/webpack.base.conf.js文件的module里面与rules同层加入externals。


![webpackBaseConf](/images/Web/webpackBaseConf.png)


* **UI库的按需加载。**


一般UI库都提供按需加载的方法，按照文档即可配置。


* **开启Gzip压缩。**


在config/index.js设置productionGzip为true，开启Gzip压缩。


![gzip](/images/Web/gzip.jpg)