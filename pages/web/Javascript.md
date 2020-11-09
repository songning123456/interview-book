#### 原始类型有哪几种？null是对象嘛？
在JS中，存在着6种原始值，分别是：<span class='forest-green'>boolean</span>、<span class='forest-green'>null</span>、<span class='forest-green'>undefined</span>、<span class='forest-green'>number</span>、<span class='forest-green'>string</span>、<span class='forest-green'>symbol</span>。


首先原始类型存储的都是值，是没有函数可以调用的，比如undefined.toString()。此时你肯定会有疑问，这不对呀，明明'1'.toString()是可以使用的。其实在这种情况下，'1'已经不是原始类型了，而是被强制转换成了String类型也就是对象类型，所以可以调用toString函数。


除了会在必要的情况下强转类型以外，原始类型还有一些坑：


其中JS的number类型是浮点类型的，在使用中会遇到某些Bug，比如0.1 + 0.2 !== 0.3，但是这一块的内容会在进阶部分讲到。string类型是不可变的，无论你在string类型上调用何种方法，都不会对值有改变。


另外对于null来说，很多人会认为他是个对象类型，其实这是错误的。虽然typeof null会输出object，但是这只是JS存在的一个悠久Bug。在JS的最初版本中使用的是32位系统，为了性能考虑使用低位存储变量的类型信息，000开头代表是对象，然而null表示为全零，所以将它错误的判断为object。虽然现在的内部类型判断代码已经改变了，但是对于这个Bug却是一直流传下来。


#### 对象类型和原始类型的不同之处？函数参数是对象会发生什么问题？
在JS中，除了原始类型那么其他的都是对象类型了。对象类型和原始类型不同的是，原始类型存储的是值，对象类型存储的是地址(指针)。当你创建了一个对象类型的时候，计算机会在内存中帮我们开辟一个空间来存放值，但是我们需要找到这个空间，这个空间会拥有一个地址(指针)。


```javascript
const a = [];
```


对于常量a来说，假设内存地址(指针)为#001，那么在地址#001的位置存放了值[]，常量a存放了地址(指针)#001，再看以下代码：


```javascript
const a = [];
const b = a;
b.push(1);
```


当我们将变量赋值给另外一个变量时，复制的是原本变量的地址(指针)，也就是说当前变量b存放的地址(指针)也是#001，当我们进行数据修改的时候，就会修改存放在地址(指针)#001上的值，也就导致了两个变量的值都发生了改变。


接下来我们来看函数参数是对象的情况：


```javascript
function test(person) {
    person.age = 26;
    person = {
        name: 'yyy',
        age: 30
    };
    return person;
}
const p1 = {
    name: 'yck',
    age: 25
};
const p2 = test(p1);
console.log(p1); // -> ?
console.log(p2); // -> ?
```


对于以上代码，你是否能正确的写出结果呢？接下来让我为了解析一番：<br>
1. 首先，函数传参是传递对象指针的副本；<br>
2. 到函数内部修改参数的属性这步，我相信大家都知道，当前p1的值也被修改了；<br>
3. 但是当我们重新为了person分配了一个对象时就出现了分歧。


所以最后person拥有了一个新的地址(指针)，也就和p1没有任何关系了，导致了最终两个变量的值是不相同的。


#### typeof是否能正确判断类型？instanceof能正确判断对象的原理是什么？
typeof对于原始类型来说，除了null都可以显示正确的类型。


```javascript
typeof 1; // 'number'
typeof '1'; // 'string'
typeof undefined; // 'undefined'
typeof true ;// 'boolean'
typeof Symbol(); // 'symbol'
```


typeof对于对象来说，除了函数都会显示object，所以说typeof并不能准确判断变量到底是什么类型。


```javascript
typeof []; // 'object'
typeof {}; // 'object'
typeof console.log; // 'function'
```


如果我们想判断一个对象的正确类型，这时候可以考虑使用instanceof，因为内部机制是通过原型链来判断的。


```javascript
const Person = function() {};
const p1 = new Person();
p1 instanceof Person; // true

var str = 'hello world';
str instanceof String; // false

var str1 = new String('hello world');
str1 instanceof String; // true
```


对于原始类型来说，你想直接通过instanceof来判断类型是不行的，当然我们还是有办法让instanceof判断原始类型的。


```javascript
class PrimitiveString {
    static [Symbol.hasInstance](x) {
        return typeof x === 'string';
    }
}
console.log('hello world' instanceof PrimitiveString); // true
```


你可能不知道Symbol.hasInstance是什么东西，其实就是一个能让我们自定义instanceof行为的东西，以上代码等同于typeof 'hello world' === 'string'，所以结果自然是true了。这其实也侧面反映了一个问题，instanceof也不是百分之百可信的。


#### 类型转换？
首先我们要知道，在JS中类型转换只有三种情况，分别是：<br>
1. 转换为布尔值；<br>
2. 转换为数字；<br>
3. 转换为字符串。


**转Boolean**


在条件判断时，除了undefined，null，false，NaN，''，0，-0，其他所有值都转为true，包括所有对象。


**对象转原始类型**


对象在转换类型的时候，会调用内置的[[ToPrimitive]]函数，对于该函数来说，算法逻辑一般来说如下：<br>
1. 如果已经是原始类型了，那就不需要转换了；<br>
2. 调用x.valueOf()，如果转换为基础类型，就返回转换的值；<br>
3. 调用x.toString()，如果转换为基础类型，就返回转换的值；<br>
4. 如果都没有返回原始类型，就会报错。


当然你也可以重写Symbol.toPrimitive，该方法在转原始类型时调用优先级最高。


```javascript
let a = {
    valueOf() {
        return 0;
    },
    toString() {
        return '1';
    },
    [Symbol.toPrimitive]() {
        return 2;
    }
};
1 + a; // => 3
```


**四则运算符**


加法运算符不同于其他几个运算符，它有以下几个特点：<br>
1. 运算中其中一方为字符串，那么就会把另一方也转换为字符串；<br>
2. 如果一方不是字符串或者数字，那么会将它转换为数字或者字符串。


```javascript
1 + '1'; // '11'
true + true; // 2
4 + [1,2,3]; // "41,2,3"
```


如果你对于答案有疑问的话，请看解析：<br>
1. 对于第一行代码来说，触发特点一，所以将数字1转换为字符串，得到结果'11'；<br>
2. 对于第二行代码来说，触发特点二，所以将true转为数字1；<br>
3. 对于第三行代码来说，触发特点二，所以将数组通过toString转为字符串1,2,3，得到结果41,2,3。


另外对于加法还需要注意这个表达式'a' + + 'b'。


```javascript
'a' + + 'b'; // -> "aNaN"
```


因为+ 'b'等于NaN，所以结果为"aNaN"，你可能也会在一些代码中看到过+ '1'的形式来快速获取number类型。那么对于除了加法的运算符来说，只要其中一方是数字，那么另一方就会被转为数字。


```javascript
4 * '3'; // 12
4 * []; // 0
4 * [1, 2]; // NaN
```


**比较运算符**


1. 如果是对象，就通过toPrimitive转换对象；
2. 如果是字符串，就通过unicode字符索引来比较。


```javascript
let a = {
    valueOf() {
        return 0
    },
    toString() {
        return '1'
    }
};
a > -1; // true
```


在以上代码中，因为a是对象，所以会通过valueOf转换为原始类型再比较值。


#### 如何正确判断this？箭头函数的this是什么？
this是很多人会混淆的概念，但是其实它一点都不难，只是网上很多文章把简单的东西说复杂了。在这一小节中，你一定会彻底明白this这个概念的。


我们先来看几个函数调用的场景：


```javascript
function foo() {
    console.log(this.a)
}
let a = 1;
foo();
const obj = {
    a: 2,
    foo: foo
};
obj.foo();
const c = new foo();
```


接下来我们一个个分析上面几个场景：<br>
1. 对于直接调用foo来说，不管foo函数被放在了什么地方，this一定是window；<br>
2. 对于obj.foo()来说，我们只需要记住，谁调用了函数，谁就是this，所以在这个场景下foo函数中的this就是obj对象；<br>
3. 对于new的方式来说，this被永远绑定在了c上面，不会被任何方式改变this。


说完了以上几种情况，其实很多代码中的this应该就没什么问题了，下面让我们看看箭头函数中的this：


```javascript
function a() {
    return () => {
        return () => {
            console.log(this);
        }
    }
}
console.log(a()()());
```


首先箭头函数其实是没有this的，箭头函数中的this只取决包裹箭头函数的第一个普通函数的this。在这个例子中，因为包裹箭头函数的第一个普通函数是a，所以此时的this是window。另外对箭头函数使用bind这类函数是无效的。最后种情况也就是bind这些改变上下文的API了，对于这些函数来说，this取决于第一个参数，如果第一个参数为空，那么就是window。


那么说到bind，不知道大家是否考虑过，如果对一个函数进行多次bind，那么上下文会是什么呢？


```javascript
let a = {};
let fn = function () { console.log(this) };
fn.bind().bind(a)(); // => ?
```


如果你认为输出结果是a，那么你就错了，其实我们可以把上述代码转换成另一种形式：


```javascript
// fn.bind().bind(a) 等于
let fn2 = function fn1() {
    return function() {
        return fn.apply()
    }.apply(a)
};
fn2()
```


可以从上述代码中发现，不管我们给函数bind几次，fn中的this永远由第一次bind决定，所以结果永远是window。


```javascript
let a = { name: 'vue' };
function foo() {
    console.log(this.name)
}
foo.bind(a)(); // => 'vue'
```


以上就是this的规则了，但是可能会发生多个规则同时出现的情况，这时候不同的规则之间会根据优先级最高的来决定this最终指向哪里。


首先，new的方式优先级最高，接下来是bind这些函数，然后是obj.foo()这种调用方式，最后是foo这种调用方式，同时，箭头函数的this一旦被绑定，就不会再被任何方式所改变。


#### ==和===有什么区别？
对于==来说，如果对比双方的类型不一样的话，就会进行类型转换，这也就用到了第四题：类型转换的问题。假如我们需要对比x和y是否相同，就会进行如下判断流程：


1. 首先会判断两者类型是否相同。相同的话就是比大小了；<br>
2. 类型不相同的话，那么就会进行类型转换；<br>
3. 会先判断是否在对比null和undefined，是的话就会返回true；<br>
4. 判断两者类型是否为string和number，是的话就会将字符串转换为number；


```
1 == '1'
      ↓
1 ==  1
```


5.判断其中一方是否为boolean，是的话就会把boolean转为number再进行判断；


```
'1' == true
        ↓
'1' ==  1
        ↓
 1  ==  1
```


6.判断其中一方是否为object且另一方为string、number或者symbol，是的话就会把object转为原始类型再进行判断。


```
'1' == { name: 'vue' }
        ↓
'1' == '[object Object]'
```


思考题：看完了上面的步骤，对于 [] == ![] 你是否能正确写出答案呢？


```javascript
[] == ![];
// true
```


#### 什么是闭包？
闭包的定义其实很简单：函数A内部有一个函数B，函数B可以访问到函数A中的变量，那么函数B就是闭包。


```javascript
function A() {
    let a = 1;
    window.B = function () {
        console.log(a);
    }
}
A();
B(); // 1
```


很多人对于闭包的解释可能是函数嵌套了函数，然后返回一个函数。其实这个解释是不完整的，就比如我上面这个例子就可以反驳这个观点。在JS中，闭包存在的意义就是让我们可以间接访问函数内部的变量。


经典面试题，循环中使用闭包解决var定义函数的问题：


```javascript
for (var i = 1; i <= 5; i++) {
    setTimeout(function timer() {
        console.log(i);
    }, i * 1000)
}
```


首先因为setTimeout是个异步函数，所以会先把循环全部执行完毕，这时候i就是6了，所以会输出一堆6。解决办法有三种，第一种是使用闭包的方式。


```javascript
for (var i = 1; i <= 5; i++) {
    (function(j) {
        setTimeout(function timer() {
            console.log(j)
        }, j * 1000)
    })(i)
}
```


在上述代码中，我们首先使用了立即执行函数将i传入函数内部，这个时候值就被固定在了参数j上面不会改变，当下次执行timer这个闭包的时候，就可以使用外部函数的变量j，从而达到目的。


第二种就是使用setTimeout的第三个参数，这个参数会被当成timer函数的参数传入。


```javascript
for (var i = 1; i <= 5; i++) {
    setTimeout(
        function timer(j) {
            console.log(j)
        },
        i * 1000,
        i
    )
}
```


第三种就是使用let定义i了来解决问题了，这个也是最为推荐的方式。


```javascript
for (let i = 1; i <= 5; i++) {
    setTimeout(function timer() {
        console.log(i)
    }, i * 1000)
}
```


#### 什么是浅拷贝？如何实现浅拷贝？什么是深拷贝？如何实现深拷贝？
在上面，我们了解了对象类型在赋值的过程中其实是复制了地址，从而会导致改变了一方其他也都被改变的情况。通常在开发中我们不希望出现这样的问题，我们可以使用浅拷贝来解决这个情况。


```javascript
let a = {
    age: 1
};
let b = a;
a.age = 2;
console.log(b.age); // 2
```


**浅拷贝**


首先可以通过Object.assign来解决这个问题，很多人认为这个函数是用来深拷贝的。其实并不是，Object.assign只会拷贝所有的属性值到新的对象中，如果属性值是对象的话，拷贝的是地址，所以并不是深拷贝。


```javascript
let a = {
    age: 1
};
let b = Object.assign({}, a);
a.age = 2;
console.log(b.age); // 1
```


另外我们还可以通过展开运算符...来实现浅拷贝。


```javascript
let a = {
    age: 1
};
let b = { ...a };
a.age = 2;
console.log(b.age); // 1
```


通常浅拷贝就能解决大部分问题了，但是当我们遇到如下情况就可能需要使用到深拷贝了。


```javascript
let a = {
    age: 1,
    jobs: {
        first: 'FE'
    }
};
let b = { ...a };
a.jobs.first = 'native';
console.log(b.jobs.first); // native
```


浅拷贝只解决了第一层的问题，如果接下去的值中还有对象的话，那么就又回到最开始的话题了，两者享有相同的地址。要解决这个问题，我们就得使用深拷贝了。


**深拷贝**


这个问题通常可以通过JSON.parse(JSON.stringify(object))来解决。


```javascript
let a = {
    age: 1,
    jobs: {
        first: 'FE'
    }
};
let b = JSON.parse(JSON.stringify(a));
a.jobs.first = 'native';
console.log(b.jobs.first); // FE
```


但是该方法也是有局限性的：<br>
1. 会忽略undefined；<br>
2. 会忽略symbol；<br>
3. 不能序列化函数；<br>
4. 不能解决循环引用的对象。


```javascript
let obj = {
    a: 1,
    b: {
        c: 2,
        d: 3,
    },
};
obj.c = obj.b;
obj.e = obj.a;
obj.b.c = obj.c;
obj.b.d = obj.b;
obj.b.e = obj.b.c;
let newObj = JSON.parse(JSON.stringify(obj));
console.log(newObj);
```


如果你有这么一个循环引用对象，你会发现并不能通过该方法实现深拷贝。在遇到函数、undefined或者symbol的时候，该对象也不能正常的序列化。


```javascript
let a = {
    age: undefined,
    sex: Symbol('male'),
    jobs: function() {},
    name: 'vue'
};
let b = JSON.parse(JSON.stringify(a));
console.log(b); // {name: "vue"}
```


你会发现在上述情况中，该方法会忽略掉函数和undefined。但是在通常情况下，复杂数据都是可以序列化的，所以这个函数可以解决大部分问题。如果你所需拷贝的对象含有内置类型并且不包含函数，可以使用MessageChannel。


```javascript
function structuralClone(obj) {
    return new Promise(resolve => {
        const { port1, port2 } = new MessageChannel();
        port2.onmessage = ev => resolve(ev.data);
        port1.postMessage(obj)
    })
}

const obj = {
  a: 1,
  b: {
    c: 2
  }
};

obj.b.d = obj.b;

// 注意该方法是异步的
// 可以处理 undefined 和循环引用对象
const test = async () => {
    const clone = await structuralClone(obj);
    console.log(clone);
};
test()
```


当然你可能想自己来实现一个深拷贝，但是其实实现一个深拷贝是很困难的，需要我们考虑好多种边界情况，比如原型链如何处理、DOM 如何处理等等，所以这里我们实现的深拷贝只是简易版，并且我其实更推荐使用lodash的深拷贝函数。


```javascript
function deepClone(obj) {
    function isObject(o) {
        return (typeof o === 'object' || typeof o === 'function') && o !== null
    }
    
    if (!isObject(obj)) {
        throw new Error('非对象')
    }
    
    let isArray = Array.isArray(obj);
    let newObj = isArray ? [...obj] : { ...obj };
    Reflect.ownKeys(newObj).forEach(key => {
    newObj[key] = isObject(obj[key]) ? deepClone(obj[key]) : obj[key]
    });
    
    return newObj
}

let obj = {
    a: [1, 2, 3],
    b: {
        c: 2,
        d: 3
    }
};
let newObj = deepClone(obj);
newObj.b.c = 1;
console.log(obj.b.c); // 2
```


#### 如何理解原型？如何理解原型链？
当我们创建一个对象时let obj = { age: 25 }，我们可以发现能使用很多种函数，但是我们明明没有定义过它们，对于这种情况你是否有过疑惑？


当我们在浏览器中打印obj时你会发现，在obj上居然还有一个__proto__属性，那么看来之前的疑问就和这个属性有关系了。其实每个JS对象都有__proto__属性，这个属性指向了原型。这个属性在现在来说已经不推荐直接去使用它了，这只是浏览器在早期为了让我们访问到内部属性[[prototype]]来实现的一个东西。


讲到这里好像还是没有弄明白什么是原型，接下来让我们再看看__proto__里面有什么吧。


看到这里你应该明白了，原型也是一个对象，并且这个对象中包含了很多函数，所以我们可以得出一个结论：对于obj来说，可以通过__proto__找到一个原型对象，在该对象中定义了很多函数让我们来使用。


在上面的图中我们还可以发现一个constructor属性，也就是构造函数。打开constructor属性我们又可以发现其中还有一个prototype属性，并且这个属性对应的值和先前我们在__proto__中看到的一模一样。所以我们又可以得出一个结论：原型的constructor属性指向构造函数，构造函数又通过prototype属性指回原型，但是并不是所有函数都具有这个属性，Function.prototype.bind()就没有这个属性。


其实原型就是那么简单，接下来我们再来看一张图，相信这张图能让你彻底明白原型和原型链。看完这张图，我再来解释下什么是原型链吧。其实原型链就是多个对象通过__proto__的方式连接了起来。为什么obj可以访问到valueOf函数，就是因为obj通过原型链找到了valueOf函数。


对于此问题知识点，总结起来就是以下几点：<br>
1. Object是所有对象的爸爸，所有对象都可以通过__proto__找到它；<br>
2. Function是所有函数的爸爸，所有函数都可以通过__proto__找到它；<br>
3. 函数的prototype是一个对象；<br>
4. 对象的__proto__属性指向原型，__proto__将对象和原型连接起来组成了原型链。