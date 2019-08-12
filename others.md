### nginx 反向代理和负载均衡
什么是反向代理  
当我们有一个服务器集群，并且服务器集群中的每台服务器的内容一样的时候，同样我们要直接从个人电脑访问到服务器集群服务器的时候无法访问，
必须通过第三方服务器才能访问集群。  
这个时候，我们通过第三方服务器访问服务器集群的内容，但是我们并不知道是哪一台服务器提供的内容，此种代理方式称为反向代理  

什么是负载均衡  
公司会建立很多的服务器，这些服务器组成了服务器集群，然后，当用户访问网站的时候，先访问一个中间服务器，再让这个中间服务器在服务器集群中选择一个压力较小的服务器，然后将该访问请求引入选择的服务器  
所以，用户每次访问，都会保证服务器集群中的每个服务器压力趋于平衡，分担了服务器压力，避免了服务器崩溃的情况  
一句话：nginx会给你分配服务器压力小的去访问  

### 原生DOM状态切换（改变类）
```js
document.classList.toggle('class') //写了n多判断，才知道有原生的方法 ┭┮﹏┭┮
dom.getBoundingClientRect()   //返回dom的宽高,以及基于viewport左上角的上下左右距离和起点x,y坐标
```
### 判断页面是加载还是刷新
```js
performance.navigation.type    //  0 加载  1 刷新
```
### 从输入地址到页面展现发生了什么
```
简要:  
1. 浏览器调用loadUrl解析url  
2. 浏览器调用DNS模块将域名解析为真实的ip地址,如果浏览器有dns缓存则直接返回ip. 否则查询本地机器的DNS,并逐层往上查找,最终返回IP,然后将其存到DNS缓存并设置过期时间.  
3. 解析完成后浏览器调用网络模块,与目标ip建立连接.  (途中经过3次握手什么的...)
4. 发送http请求,向服务器请求所需的资源.  
5. 解析返回的资源,构建html树,css树,渲染,绘制,最终呈现出来  
```

### 时间戳格式化
```js
/** 
 * 时间戳转化为年 月 日 时 分 秒 
 * number: 传入时间戳 
 * format：返回格式，支持自定义，但参数必须与formateArr里保持一致 
*/  
function formatTime(number,format) {  
  
  var formateArr  = ['Y','M','D','h','m','s'];  
  var returnArr   = [];  
  
  var date = new Date(number * 1000);  
  returnArr.push(date.getFullYear());  
  returnArr.push(formatNumber(date.getMonth() + 1));  
  returnArr.push(formatNumber(date.getDate()));  
  
  returnArr.push(formatNumber(date.getHours()));  
  returnArr.push(formatNumber(date.getMinutes()));  
  returnArr.push(formatNumber(date.getSeconds()));  
  
  for (var i in returnArr)  
  {  
    format = format.replace(formateArr[i], returnArr[i]);  
  }  
  return format;  
} 

//数据转化  
function formatNumber(n) {  
  n = n.toString()  
  return n[1] ? n : '0' + n  
} 
eg:  formatTime(+new Date,'Y/M/D h:m:s')  // '2019/08/12 10:06:20'
```
