## 目录
- [关于appply(),bind(),call()](#appply(),bind(),call())
- [事件委托](#事件委托)
- [JavaScript中的this指向问题](#JavaScript中的this)

### ```js
appply(),bind(),call()
```
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
1. 在调用函数时使用new关键字，函数内的this是一个全新的对象。
2. 如果apply、bind或call方法用于调用、创建一个函数，函数内的 this 就是作为参数传入这些方法的对象。
3. 当函数作为对象里的方法被调用时，函数内的this是调用该函数的对象。
    比如当obj.method()被调用时，函数内的 this 将绑定到obj对象。
4. 如果调用函数不符合上述规则，那么this的值指向全局对象（global object）。
    浏览器环境下this的值指向window对象，但是在严格模式下('use strict')，this的值为undefined。
5. 如果符合上述多个规则，则较高的规则（1 号最高，4 号最低）将决定this的值。
6. 如果该函数是 ES2015 中的箭头函数，将忽略上面的所有规则，this被设置为它被创建时的上下文。
    箭头函数的this具有穿透性，将this的作用域延伸至上一层区域，如：
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
