### appply(),bind(),call()
```
// 都是改变this指向，第一个参数都是指向的对象，区别在第二个参数
// apply()第二个参数已数组形式给出
apply(this,[arguments])
// bind()第二个以及之后的参数按顺序给出即可，区别是需要调用
bind(this,a,b,c)()
// call()参数同bind()，但不需要调用
call(this,a,b,c)
```
