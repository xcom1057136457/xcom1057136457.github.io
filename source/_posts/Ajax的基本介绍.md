---
title: Ajax的基本介绍
lang: zh-CN
date: 2024-08-29 16:49:21
---
## Ajax的解释

**Ajax:async javascript and xml（异步的JavaScript和xml）**
**用于浏览器和后台服务进行异步交互（传递信息）**

## 特点

**1.不会导致页面的全局刷新就可以进行与后台的交互
2.交互需要在"查看元素 -> 网络"中监控**

## 使用方式

``` javascript
XMLHttpRequest
		1. 实例化
			var xhr = new XMLHttpRequest();
		2. 设置请求
			xhr.open(method,url);	
		3. 设置头信息
			xhr.setRequestHeader()
		4. 设置体信息（method为post时候）
			xhr.send(data);

		5. 设置监听
			xhr.onreadystatechange = function(){
				this.readyState	// 	1 2 3 4 
				this.status			// 	200 404 500
				this.response		//	响应信息
			}

			200 	ok
			404 	找不到资源
			500 	后台异常
```

> 举例

``` javascript
var $ = {
	// get请求
	get : function(url,handler){
		// 1. 实例化xhr对象
		var xhr = new XMLHttpRequest();
		// 2. 设置请求行
		xhr.open("get",url);
		// 3. 设置请求头（Content-Type）
		xhr.responseType = "json";
		// 4. 设置请求体，并且发送
		xhr.send()
		// 5.监听响应
		xhr.onreadystatechange = function(){
			if(this.readyState === 4){
				if(this.status === 200){
					var response = this.response;
					handler.call(this,response)
				}
			}
		}
	},
	//post提交
	post : function(url,data,handler,type){
		// 1. 实例化xhr对象
		var xhr = new XMLHttpRequest();
		// 2. 设置请求行
		xhr.open("post",url);
		// 3. 设置请求头（）
		xhr.responseType = "json";
		if(!type){
			type = "application/x-www-form-urlencoded";
		}
		xhr.setRequestHeader("Content-Type",type)
		// 4. 设置请求体，并且发送
		xhr.send(data)
		// 5.监听响应
		xhr.onreadystatechange = function(){
			if(this.readyState === 4){
				if(this.status === 200){
					var response = this.response;
					handler.call(this,response)
				}
			}
		}
	}
}

```

## HTTP请求报文

```
请求行
    GET / HTTP/1.1
请求头信息
    Content-Type:application/x-www-form-urlencoded
    Content-Size:1204
    ...
请求体信息（post参数）
```

## HTTP响应报文

```
响应行
    HTTP/1.1 200 OK
响应头
    Server: Apache
    Content-Length: 29769
    Content-Type: text/html
    ...
响应体
    <!DOCTYPE html...
```
## jQuery中的Ajax【基于回调函数】

### 1.速写方式

``` javascript
$.get(url[,data][,success][,dataType])
    以get方式请求
        url 	请求地址
        data  请求参数，对象
        success		回调函数
        dataType 	responseType
    =>
    $.ajax(url,{
        method:"get",
        success:function(){},
        data:data,
        dataType:'json'
    })


$.post(url[,data][,success][,dataType])
    以post方式请求
        url 	请求地址
        data  请求参数，对象
        success		回调函数
        dataType 	responseType
    =>
    $.ajax(url,{
        method:"post",
        success:function(){},
        data:data,
        dataType:'json'
    })

```

### 2.低级别接口

``` javascript
$.ajax(url,settings)
		settings是一个对象，配置信息；
			async 			异步，true;
			beforeSend	回调函数，在请求发送前调用
			complete		回调函数，请求接受后调用
			success 		回调函数，请求成功后
			error 			回调函数，请求结束后
			contentType 参数类型，默认为querystring
				默认值application/x-www-form-urlencoded
				如果参数为json，要改为application/json
			processData	boolean,默认将data转换为查询字符串
				如果参数为json，要将其设置为false
			data 				对象，参数
				如果参数为json；JSON.stringify(对象)
			dataType 		responseType,json/xml/script...
			headers 		对象，setRequestHeader(key,val)
			method 			请求方式，get/post
```

### 如何使用jQuery发送json数据？

``` javascript
xhr.setRequestHeader("Content-Type","application/json");
		xhr.send(JSON.stringify(data))
		=>
		$.ajax(url,{
			method:"post",
			contentType:"application/json",
			processData:false,
			data:JSON.stringify(参数对象),
			success:function(){}
		})
```
