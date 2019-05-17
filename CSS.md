### 1. 半圆
```css
div {
  width: 50px;
  height: 100px;
  background: #000;
  border-radius: 50px 0 0 50px;
}
```
### 2.清浮动  
 · 父级div定义height  
 · 结尾处加空div标签clear:both  
 · 父级div定义伪类:after和zoom   
    ```.clearfloat:after{display:block;clear:both;content:"";visibility:hidden;height:0}  
    .clearfloat{zoom:1}  ```
 · 父级div定义overflow:hidden  
 · 父级div也浮动，需要定义宽度  
 · 结尾处加br标签clear:both  
    
### 3. BFC清浮动  摘自[齐修_qixiuss](https://www.jianshu.com/p/09bd5873bed4)
我们可以给父元素添加以下属性来触发BFC：
1. float 为 left | right
2. overflow 为 hidden | auto | scorll
3. display 为 table-cell | table-caption | inline-block | flex | inline-flex
4. position 为 absolute | fixed

BFC特性 推荐[CSS深入理解流体性和BFC特性](https://www.zhangxinxu.com/wordpress/2015/02/css-deep-understand-flow-bfc-column-two-auto-layout/)

### 4. flex布局
<img src="https://github.com/lu-xiansen/myNote/blob/master/img/flex.jpg?raw=true">
