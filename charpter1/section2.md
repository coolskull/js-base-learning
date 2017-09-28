1.2 客户端JavaScript
-------------------------
知识点
----------

  use strict: http://www.cnblogs.com/jiqing9006/p/5091491.html

  http的状态：http://www.w3school.com.cn/tags/html_ref_httpmessages.asp

  instanceof:

#### click事件，兼容ie\chrome\firefox

    window.onload = function() {
        var images = document.getElementsByTagName("img");
        for (var i = 0; i < images.length; i++) {
            var image = images[i];
            if (image.addEventListener) {
                image.addEventListener("click", hide, false); // 最后一个参数：true，在捕获阶段执行；false,在冒泡阶段执行
            } else {
                image.attachEvent('onclick', hide);    // 兼容IE8及以前的版本
            }
        }

        function hide(event) {
            event.target.style.visibility = "hidden";
        }
    }

 ####debug 代码片段

    function debug(msg){
        var log=document.getElementById("debuglog");
        if(!log){
            log=document.createElement('div');
            log.id='debuglog';
            log.innerHtml='<h1>Debug log</h1>';
            document.body.appendChild(log);
            var pre=document.createElement('pre');
            var text=document.createTextNode(msg);  // 将msg包装在一个文本节点中
            pre.appendChild(text);
            log.appendChild(pre);
        }
    }

    debug("abc”);




