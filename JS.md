## 目录
- [一、js原始数据类型](#一js原始数据类型)
- [二、关于apply(),bind(),call()](#二applybindcall)
- [三、事件委托](#三事件委托)
- [四、JavaScript中的this指向问题](#四JavaScript中的this)
- [五、闭包](#五JavaScript作用域以及闭包)
- [六、多个原生对象的常用方法](#六原生对象的常用方法)
- [七、排序sort()](#七排序)
- [八、数组去重](#八数组去重)
- [九、forEach()和map()](#九forEach和map)
- [十、web存储](#十web存储-cookie-localStorage-sessionStorage)
- [十一、捕获和冒泡](#十一捕获和冒泡)
- [十二、深拷贝和浅拷贝](#十二深拷贝和浅拷贝)
- [十三、防抖和节流](#十三防抖和节流)
- [十四、常见状态码](#十四常见状态码)
- [十五、for in & for of](#十五for-in--for-of)
### 一、js原始数据类型
1. undefined  
2. null  
3. Boolean 
4. [String](http://www.runoob.com/jsref/jsref-obj-string.html)
5. [Number](http://www.runoob.com/jsref/jsref-obj-number.html) 
6. [Object(Array,Date,Math,RegExp)](http://www.runoob.com/jsref/jsref-tutorial.html) 
7. [Symbol](http://www.runoob.com/w3cnote/es6-symbol.html)

### 二、apply(),bind(),call
```js
// 都是改变this指向，第一个参数都是指向的对象，区别在第二个参数
// apply()第二个参数以数组形式给出
apply(this,[arguments])
// bind()第二个以及之后的参数按顺序给出即可，区别是需要调用
bind(this,a,b,c)()
// call()参数同bind()，但不需要调用
call(this,a,b,c)
```
### 三、事件委托
个人理解：事件监听的一种用法，子元素较多的情况下，将监听的任务委托给其父元素，利用事件冒泡来监听子元素  
jQuery里的 ```$('ul').on('click',fn)```  
js的 ```document.addEventListener('click',fn)```  
都能通过冒泡来给子元素注册事件，性能更优，且动态添加的子元素也可以触发事件

### 四、JavaScript中的this
1. 在调用函数时使用`new`关键字，函数内的`this`是一个全新的对象。
2. 如果`apply`、`bind`或`call`方法用于调用、创建一个函数，函数内的`this`就是作为参数传入这些方法的对象。
3. 当函数作为对象里的方法被调用时，函数内的`this`是调用该函数的对象。
    比如当`obj.method()`被调用时，函数内的`this`将绑定到obj对象。
4. 如果调用函数不符合上述规则，那么this的值指向全局对象`（global object）`。
    浏览器环境下`this`的值指向`window`对象，但是在严格模式下`('use strict')`，`this`的值为`undefined`。
5. 如果符合上述多个规则，则较高的规则（1 号最高，4 号最低）将决定this的值。
6. 如果该函数是 ES2015 中的箭头函数，将忽略上面的所有规则，`this`被设置为它被创建时的上下文。
   指向箭头函数外层非箭头函数的this指向 ;如：
```js
var Widget={
    init:function () {
        document.addEventListener("click", function() {
            console.log(this) // 指向document 
            //此处调用this.doSomething()会报错
        }, false);
    },
    doSomething:function (type) {
        console.log(this); // 指向Widget对象
    }
};
Widget.init();

var Widget={
    init:function () {
        document.addEventListener("click", () => {
            console.log(this) // 指向Widget 因此可以调用Widget的doSomething方法
            this.doSomething() //
        }, false);
    },
    doSomething:function (type) {
        console.log(this);  // 指向Widget对象
    }
};
Widget.init(); 
```
[返回顶部 ▲](#目录)

### 五、JavaScript作用域以及闭包
作用域分为三种：
- 全局作用域
- 当前作用域/函数作用域
- 块级作用域（ES6）

前置知识:
在A作用域中使用变量x,却没有在A作用域中声明x(即在其它作用域声明的),x为自由变量
```js
var x = 10;
function fn(){
    console.log(x)  // x为自由变量
}
function show(f){
    var x = 20;
    f();        
}
show(fn) // 10   
```
执行show(fn)时,上下文环境从全局变为show()  
show()调用fn()时上下文环境变为fn()  
fn()执行时打印的x取值要去`创建`这个函数(即fn)的作用域取而不是`调用`函数的作用域  
创建fn时,x存在于全局作用域,所以是`10`

#闭包  
记住两种情况即可:  
1. 函数作为返回值  
```js
function fn(){
    var max = 10;
    return function bar(x){      // 
        if(x>max){
            console.log(x)
        }
    }
}
var f1 = fn();  // bar函数作为返回值赋值给f1
f1(15);  // 15  
```
`执行时用到了fn作用域下声明的max变量,导致fn函数无法销毁,产生闭包`  

2. 函数作为参数传递  
```js
var max = 10,
    fn = function(x){
        if(x>max){
            console.log(x)
        }
    }
;(function(f){
    var max = 100;
    f(15) // 这里把fn传进来执行,max取值要看声明fn时候max所在作用域
})(fn)  // 15 
```
`函数fn作为参数传递给另一个函数,赋值给形参,执行f(15)时,max取值是10,不是100`  

[返回顶部 ▲](#目录)

### 六、原生对象的常用方法
1. 数组  
<img src="https://github.com/lu-xiansen/myNote/blob/master/img/Array.jpg?raw=true">  

[返回顶部 ▲](#目录)

### 七、排序
```js
// sort()默认按首字母或者数字的第一位排序 a-z,A-Z 
let arr = [3,1,10,-2,0,8,-35]
arr.sort() // [-2,-35,0,1,10,3,8]
// 升序
arr.sort((a,b)=>{
    return a-b      // 理解为a到b  便于记忆
})  // [-35, -2, 0, 1, 3, 8, 10]
arr.sort((a,b)=>{
    return b-a      // 理解为b到a
})  // [10, 8, 3, 1, 0, -2, -35]

数组项为引用类型排序 [obj,obj,obj]
function sortObj(propertypeName){
    return function(obj1,obj2){
        return obj1[propertypeName]-obj2[propertypeName]
    }
}
arr.sort(sortObj("age"))   // 数组根据数组项的age属性排序  
```
[返回顶部 ▲](#目录)

### 八、数组去重
```js
// ES6 Set
let arr1 = [1,2,3,2,5,1];
let set = new Set(arr1); // {1,2,3,5,}
let arr2 = Array.from(set); // [1,2,3,5]

// 其它方法
1. 
for(let i=0;i<arr1.length-1;i++){
    for(let j=i+1;j<arr1.length;j++){
        if(arr1[j] == arr1[i]){
            arr1.splice(j,1);       // splice()返回的是数组是删除了的元素 原数组arr1已经被改变
            j--;
        }
    }
}
// console.log(arr1)  [1,2,3,5]
2. 
for(let i=0;i<arr1.length;i++){
    if(arr1.indexOf(arr1[i]) != i){
        arr1.splice(i,1)   
    }
}
// console.log(arr1)  [1,2,3,5]
```
[返回顶部 ▲](#目录)

### 九、forEach()和map()
```js
//都接受3个参数 item(当前项) index(当前项索引) arr(原数组)

let num = [1,2,3,4,5,6];
num.forEach(function(item,index,arr){
    console.log(item,index)
    if(item == 4){
        num.shift()
        // 这步执行完之后，原数组变为[2,3,4,5,6]
        // 所以下一步原本打印5的位置上的元素变为6
    }
},this)
// 1 0
// 2 1
// 3 2
// 4 3 
// 6 4
// num [2,3,4]
在使用forEach()时候，如果数组在迭代的过程被修改，后续函数执行时传入的就是修改后的数组。
因为forEach()不会在迭代之前创建数组的副本
forEach()返回值为undefined，不能链式调用  
用来遍历数组，并且做一些操作。

let arr = [1,4,9,16]
arr.map(function(item,index,arr){
    return Math.sqrt(item)
})
// [1,2,3,4]
上面函数可简写为
arr.map(Math.sqrt)

map()返回一个新数组,数组的元素是回调函数处理过值,也不检测空数组
不改变原数组  
用来处理数组中的每一项，并且返回一个处理后的新数组
```
[返回顶部 ▲](#目录)

### 十、web存储 cookie localStorage sessionStorage
- H5之前，主要的存储方式是cookie
- localStorage 存储于本地浏览器，不像cookie一样存储在http请求头字段里，节约了带宽
    大小一般限制为5M，只要不删除，就会一直存在
    存储方式为字符串，所以通常需要JSON.parse(localStorage.key)转换为json对象来操作
    存储对象时也需要通过JSON.stringify(obj)转为字符串存储
```js
localstorage.length: 获取当前存储的键值对数量
localstorage.key(n):获取第n项的键值
localstorage.getItem(key):获取对应键值的数据 或者localStorage.key
localstorage.setItem(key,value):设置对应的键值对 或者localStorage.key = value
localstorage.remove(key):清除某个数据
localstorage.clear():清除存储的所有数据
```
- sessionStorage 存储周期为当前会话，浏览器关闭就会删除，用法基本同localStorage

[返回顶部 ▲](#目录)

### 十一、捕获和冒泡
- 事件分为捕获和冒泡事件
- 个人理解：
1. 代码自上而下执行，从外层标签到内层标签为捕获过程（window>document>body>div...）
2. 捕获完成后会执行冒泡过程，和捕获过程相反（div>body>document>window）
```js
document.addEventListener('event',function,type)
// event: 事件类型
// function: 事件触发后执行的函数
// type: true/false  指定为冒泡或捕获（可选,默认false,即默认为冒泡）
// 阻止冒泡：e.stopProppagation() 或 e.cancelBubble = true (IE)
// 捕获时，会触发父级到子级的同一事件
回调函数参数
```
[返回顶部 ▲](#目录)

### 十二、深拷贝和浅拷贝
区别：`B复制了A，A的属性不论是基础类型还是引用类型，B都不随A改变，则是深拷贝
    B的基础类型属性没有改变(即第一层拷贝),引用类型(即更深层)随着A发生了改变,则是浅拷贝`
这个概念针对引用类型数据
1. 实现深拷贝
```js
// 1  
let obj1 = {
    name: 'wjl',
    age: 25
    };
let obj2 = JSON.parse(JSON.stringify(obj1));  
    obj1.name = 'czy';
    obj1   // {name: 'czy',age:25}  原对象属性被改变
    obj2   // {name: 'wjl',age:25}  深拷贝的对象拥有独立的内存，不受obj1变化的影响  
// 2  
function deepCopy(obj) {
      var result = Array.isArray(obj) ? [] : {};
      for (var key in obj) {
        if (obj.hasOwnProperty(key)) {
          if (typeof obj[key] === 'object') {
            result[key] = deepCopy(obj[key]);   //递归复制
          } else {
            result[key] = obj[key];
          }
        }
      }
      return result;
    }
```
2. 浅拷贝(只实现一层的深拷贝)
```js
let a = {
    name: 'wjl',
    info:{
        age: 25,
        sexy:'boy'
        }
    },
    b = Object.assign({},a);  // 该方法只能实现一层的深拷贝，如果对象属性也是对象，则不会实现深拷贝
b.name = 'czy';
b.info.sexy = 'girl';
a // {name:'wjl',info:{age:25,sexy:'girl'}} 对象属性name为基础类型,没有改变
b // {name:'czy',info:{age:25,sexy:'girl'}} 对象属性info为引用类型,发生了改变
```
也可以通过展开运算符`...`来实现浅拷贝
```js
 let a = {
    name: 'wjl',
    info:{
        age: 25,
        sexy:'boy'
        }
    },
    b = {...a};
b.name = 'czy';
b.info.sexy = 'girl';
a //{name: 'wjl',info: {age:25,sexy: 'girl'}} 对象属性name为基础类型,没有改变
b //{name: 'czy',info: {age:25,sexy: 'girl'}} 对象属性info为引用类型,发生了改变
```   

[返回顶部 ▲](#目录)

### 十三、防抖和节流

[返回顶部 ▲](#目录)  

### 十四、常见状态码  
`200` 成功  
`301` 重定向  
`304` (未修改) 自从上次请求后，请求的网页未修改过。 服务器返回此响应时，不会返回网页内容。  
`400` (错误请求) 服务器不理解请求的语法。  
`403` (禁止) 服务器拒绝请求。  
`404` (未找到) 服务器找不到请求的网页。  
`500` (服务器内部错误) 服务器遇到错误，无法完成请求。  
`501` (尚未实施) 服务器不具备完成请求的功能。 例如，服务器无法识别请求方法时可能会返回此代码。  
`502` (错误网关) 服务器作为网关或代理，从上游服务器收到无效响应。  
`503` (服务不可用) 服务器目前无法使用(由于超载或停机维护)。 通常，这只是暂时状态。  
`504` (网关超时) 服务器作为网关或代理，但是没有及时从上游服务器收到请求。  
`505` (HTTP 版本不受支持) 服务器不支持请求中所用的 HTTP 协议版本。  

[返回顶部 ▲](#目录)

### 十五、for in & for of

```for in ```可以遍历对象和数组,遍历对象时,i为属性名,遍历数组时,i为索引  
```for of ```不能遍历对象,只能遍历数组/数组对象/字符串/map/set等拥有迭代器对象的集合,i为数组项  
