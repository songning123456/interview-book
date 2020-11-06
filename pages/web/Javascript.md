#### 说一下事件代理？
事件委托是指将事件绑定到目标元素的父元素上，利用冒泡机制触发该事件。


```javascript
ulEl.addEventListener('click', function(event) {
    let target = event.target || event.srcElement;
    if(!!target && target.nodeName.toUpperCase() === "LI"){
        console.log(target.innerHTML);
    }
}, false);
```


#### target、currentTarget的区别？
| 名称 | 解释 | 
| :----- | :----- | 
|target|当前所绑定事件的元素|
|currentTarget|当前被点击的元素|


#### 说一下宏任务和微任务？
| 名称 | 解释 | 
| :----- | :----- | 
|<div style="width: 60px">宏任务</div>|当前调用栈中执行的任务称为宏任务。(主代码快，定时器等等)|
|<div style="width: 60px">微任务</div>| 当前(此次事件循环中)宏任务执行完，在下一个宏任务开始之前需要执行的任务为微任务。(可以理解为回调事件，promise.then，proness.nextTick等等。)|


**宏任务**中的事件放在callback queue中，由事件触发线程维护；**微任务**的事件放在微任务队列中，由js引擎线程维护。


#### export和export default的区别？
```
export default  xxx;
import xxx from './';

export xxx;
import {xxx} from './';
```


#### get、post的区别？
* get传参方式是通过地址栏URL传递，是可以直接看到get传递的参数；post传参方式参数URL不可见。
* get把请求的数据在URL后通过?连接，通过&进行参数分割；post将参数存放在HTTP的包体内。
* get传递数据是通过URL进行传递，对传递的数据长度是受到URL大小的限制，URL最大长度是2048个字符；post没有长度限制。
* get后退不会有影响；post后退会重新进行提交。
* get请求可以被缓存；post不可以被缓存。
* get请求只URL编码；post支持多种编码方式。
* get请求的记录会留在历史记录中；post请求不会留在历史记录。
* get只支持ASCII字符；post没有字符类型限制。