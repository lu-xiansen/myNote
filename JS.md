## 目录
- [js原始数据类型](#js原始数据类型)
- [关于apply(),bind(),call()](#applybindcall)
- [事件委托](#事件委托)
- [JavaScript中的this指向问题](#JavaScript中的this)
- [多个原生对象的常用方法](#原生对象的常用方法)
- [排序sort()](#排序)
- [数组去重](#数组去重)
- [forEach()和map()(待完善)](#forEach和map)
- [web存储](#web存储-cookie-localStorage-sessionStorage)
- [捕获和冒泡](#捕获和冒泡)

### js原始数据类型
1. undefined  
2. null  
3. Boolean 
4. [String](http://www.runoob.com/jsref/jsref-obj-string.html)
5. [Number](http://www.runoob.com/jsref/jsref-obj-number.html) 
6. [Object(Array,Date,Math,RegExp)](http://www.runoob.com/jsref/jsref-tutorial.html) 
7. [Symbol](http://www.runoob.com/w3cnote/es6-symbol.html)

### apply(),bind(),call
```js
// 都是改变this指向，第一个参数都是指向的对象，区别在第二个参数
// apply()第二个参数已数组形式给出
apply(this,[arguments])
// bind()第二个以及之后的参数按顺序给出即可，区别是需要调用
bind(this,a,b,c)()
// call()参数同bind()，但不需要调用
call(this,a,b,c)
```
### 事件委托
个人理解：事件监听的一种用法，子元素较多的情况下，将监听的任务委托给其父元素，利用事件冒泡来监听子元素
### JavaScript中的this
1. 在调用函数时使用`new`关键字，函数内的`this`是一个全新的对象。
2. 如果`apply`、`bind`或`call`方法用于调用、创建一个函数，函数内的`this`就是作为参数传入这些方法的对象。
3. 当函数作为对象里的方法被调用时，函数内的`this`是调用该函数的对象。
    比如当`obj.method()`被调用时，函数内的`this`将绑定到obj对象。
4. 如果调用函数不符合上述规则，那么this的值指向全局对象`（global object）`。
    浏览器环境下`this`的值指向`window`对象，但是在严格模式下`('use strict')`，`this`的值为`undefined`。
5. 如果符合上述多个规则，则较高的规则（1 号最高，4 号最低）将决定this的值。
6. 如果该函数是 ES2015 中的箭头函数，将忽略上面的所有规则，`this`被设置为它被创建时的上下文。
    箭头函数的`this`具有穿透性，将`this`的作用域延伸至上一层区域，如：
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

### 原生对象的常用方法
```js
待补充
```
[返回顶部 ▲](#目录)

### 排序
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
```
[返回顶部 ▲](#目录)

### 数组去重
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

### forEach()和map()
```js
都接受3个参数 item(当前项) index(当前项索引) arr(原数组)

let num = [1,2,3,4];
num.forEach(function(item,index,arr){
    console.log(item)
    if(item == 2){
        num.shift()
    }
},this)
// 1
// 2
// 4
// num [2,3,4]
在使用forEach()时候，如果数组在迭代的过程被修改，则其他元素会被跳过。因为 forEach()不会在迭代之前创建数组的副本
forEach()返回值为undefined，不能链式调用
原数组被改变

arr.map(function(item,index,arr){
    
})
map()返回一个新数组,数组的元素是回调函数处理过值,不改变原数组,也不检测空数组

```
[返回顶部 ▲](#目录)

### web存储 cookie localStorage sessionStorage
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

### 捕获和冒泡
- 事件分为捕获和冒泡事件
- 个人理解：
1. 代码自上而下执行，从外层标签到内层标签为捕获过程（window>document>body>div...）
2. 捕获完成后会执行冒泡过程，和捕获过程相反（div>body>document>window）
```js
document.addEventListener('event',function,type)
// event: 事件类型
// function: 事件触发后执行的函数
// type: true/false  指定为冒泡或捕获（可选,默认false,即默认为冒泡）
```
[返回顶部 ▲](#目录)

