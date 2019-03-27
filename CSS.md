### 1. 半圆
```css
div {
  width: 50px;
  height: 100px;
  background: #000;
  border-radius: 50px 0 0 50px;
}
```
### 2. BFC清浮动  摘自[齐修_qixiuss](https://www.jianshu.com/p/09bd5873bed4)
我们可以给父元素添加以下属性来触发BFC：
1. float 为 left | right
2. overflow 为 hidden | auto | scorll
3. display 为 table-cell | table-caption | inline-block | flex | inline-flex
4. position 为 absolute | fixed

BFC特性 推荐[CSS深入理解流体性和BFC特性](https://www.zhangxinxu.com/wordpress/2015/02/css-deep-understand-flow-bfc-column-two-auto-layout/)

