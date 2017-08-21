---
title: WHAT IS AJAX ?
date: 2017-03-25 19:12:37
tags:
    - ajax
    - 异步刷新
    - XMLHttpRequest
    - get/post
categories: Javascript
---
> 今天唠一下JavaScript中最重要的一个东西——AJAX。那什么是AJAX？
ajax的全称是Asynchronous JavaScript and XML，也就是异步的js和xml
ajax不是新的编程语言，而是一种使用现有标准的新方法，是与服务器交换数据并更新部分网页的艺术，在不重新加载整个页面的情况下。
ajax是一种用于创建快速动态网页的技术，通过在后台与服务器进行少量数据交换，ajax可以使网页实现异步更新。这意味着可以在不重新加载整个网页的情况下，对网页的某部分进行更新。


> XMLHttpRequest（对象）是ajax的基础，XMLHttpRequest对象用于在后台与服务器交换数据。
接下来看下AJAX编写步骤：


```
//创建XMLHttpRequest对象
var xhr = new XMLHttpRequest();--非IE写法
var xhr = new ActiveXObject(Msxml2.XMLHTTP); --IE写法
//给onreadystatechange事件添加函数
xhr.onreadystatechange = function(){
//判断状态值是否等于4
if (xhr.readyState == 4 &&  xhr.status == 200){
    console.log(xhr.responseText);
    }
}
//设置请求参数
xhr.open("get","url",true);
//发送请求
xhr.send();
```




> open()中参数的使用：

- GET/POST区别
get大多数用于向服务器请求数据，post用于向服务器发送数据。
get把参数数据队列加到提交表单的action属性所指的url中，在url中可以看到，post是通过http post机制用户看不到这个过程。所以get安全性没有post高。
get请求的数据量较小所以简单也更快，而post数据量较大，一般默认不受限制。所以执行效率比post略强。

    ```
    POST:
    xmlhttp.open("POST","ajax_test.asp",true);
    xmlhttp.setRequestHeader("Content-type","application/x-www-form-urlencoded");
    xmlhttp.send("fname=Bill&lname=Gates");
    
    //setRequestHeader(header，value) 向请求添加头部
    //header:规定头的名称
    //value：规定头的值
    ```


- URL 可以是任何类型的文件，比如 .txt 和 .xml，或者服务器脚本文件，比如 .asp 和 .php （在传回响应之前，能够在服务器上执行任务）。

- open方法中的async 可以是true，也可以是false。
当Async = false时，请将 open() 方法中的第三个参数改为 false：
xmlhttp.open("GET","test1.txt",false);
我们不推荐使用 async=false，但是对于一些小型的请求，也是可以的。
请记住，JavaScript 会等到服务器响应就绪才继续执行。如果服务器繁忙或缓慢，应用程序会挂起或停止。
注释：当您使用 async=false 时，请不要编写 onreadystatechange 函数 - 要把代码放到 send() 语句后面:

    ```
    xmlhttp.open("GET","test1.txt",false);
    xmlhttp.send();
    document.getElementById("myDiv").innerHTML=xmlhttp.responseText;
    ```




- onreadystatechange事件
当请求被发送到服务器时，我们需要执行一些基于响应的任务。每当 readyState 改变时，就会触发onreadystatechange 事件。readyState 属性存有 XMLHttpRequest 的状态信息。下面是 XMLHttpRequest 对象的三个重要的属性：
![](https://ooo.0o0.ooo/2017/06/11/593d06640a101.png)

- 当 readyState 等于 4 且状态为 200 时，表示响应已就绪。
注释：onreadystatechange 事件被触发 5 次（0 - 4），对应着 readyState 的每个变化。

```
var ajax = {
    get: function (url, func) {
    var xhr = null;
    if (window.ActiveXObject) {
        var xhr = new ActiveXObject("Msxml2.XMLHTTP") || new ActiveXObject("Microsoft.XMLHTTP");
    } else {
         var xhr = new XMLHttpRequest();
        }

        xhr.open("get", url, "true");//这里已经准备好发送了
        xhr.send();
        xhr.onreadystatechange = function () {
            if (xhr.readyState == 4 && xhr.status == 200) {
                var str = xhr.responseText;
                 //回调
                    /*
                    func = function(str){
                         if(str == "ok"){

                         }
                    }
                */
                if(typeof func == "function"){
                    func(str);
                }

                 }
        }
    },
    post: function (url,data,func) {
        var xhr = null;
        if(window.ActiveXObject){
            xhr = new ActiveXObject("Msxml2.XMLHTTP") || new ActiveXObject("Microsoft.XMLHTTP") ;
        }else{
            xhr = new XMLHttpRequest();
        }

        xhr.open("post",url,true);
        xhr.setRequestHeader('Content-Type','application/x-www-form-urlencoded');
        xhr.send(data);

        xhr.onreadystatechange = function () {
        if(xhr.status == 200 && xhr.readyState==4){
            var str = xhr.responseText;
                func(str);
            }
        }

    }
}
```





