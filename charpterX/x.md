###寄生继承





#####js事件兼容ie8的方法
```js

function addEvent(obj, type, handle) {
    try {
        // Chrome、FireFox、Opera、Safari、IE9.0及其以上版本
        // 这里没有on
        obj.addEventListener(type, handle, false);
        // 最后一个参数useCapture
        // true - 事件句柄在捕获阶段执行
        // false- 默认。事件句柄在冒泡阶段执行
    } catch (e) {
         // IE8.0及其以下版本
        obj.attachEvent('on' + type, handle);
    }
}
```
#####从输入 URL 到页面加载完成的过程中都发生了什么事情
1. 输入地址   
2. 浏览器查找域名的 IP 地址,这一步包括 DNS 具体的查找过程，包括：
 浏览器缓存，系统缓存，路由器缓存，IPS服务器缓存，根域名服务器缓存，顶级域名服务器缓存，主域名服务器缓存。   
4. 浏览器向 web 服务器发送一个 HTTP 请求   
5. 服务器的永久重定向响应（从 http://example.com 到 http://www.example.com）   
6. 浏览器跟踪重定向地址   
7. 服务器处理请求   
8. 服务器返回一个 HTTP 响应   
9. 浏览器显示 HTML   
10. 浏览器发送请求获取嵌入在 HTML 中的资源（如图片、音频、视频、CSS、JS等等）   
11. 浏览器发送异步请求   

#####状态码
https://segmentfault.com/a/1190000006879700
状态码是由3位数组成，第一个数字定义了响应的类别，且有五种可能取值:

* 1xx：指示信息–表示请求已接收，继续处理。
* 2xx：成功–表示请求已被成功接收、理解、接受。
* 3xx：重定向–要完成请求必须进行更进一步的操作。
* 4xx：客户端错误–请求有语法错误或请求无法实现。
* 5xx：服务器端错误–服务器未能实现合法的请求。
* 平时遇到比较常见的状态码有:200, 204, 301, 302, 304, 400, 401, 403, 404, 422, 500(分别表示什么请自行查找)。


#####图片预加载
```js
function preloadImage(url) {
    return new Promise(function(resolve, reject) {
        var img = new Image();
        img.src = url;
        img.onload = resolve;
        img.onerror = reject;
    });
}

var imgUrls = ['http://api0.map.bdimg.com/images/blank.gif','http://localhost:63342/anc/images/sign.png'];
Promise.all(imgUrls.map(preloadImage))
    .then(function(){
        console.log('finish all');
    }, function(){
        console.log('failed');
    });

```


#####怎么优化加载速度


