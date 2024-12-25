---
title: Axios的简单介绍
lang: zh-CN
date: 2024-08-29 16:49:21
---
> Axios是基于Promise函数的Ajax的框架

**Axios既可以运行在浏览器上又可以运行在nodejs上，如果运行在浏览器中，封装XMLHttpRequest，如果运行在nodejs，封装http模块**

## 1. Ajax

* XMLHttpRequest：不能使用任何框架来完成异步请求
* jQuery.ajax：主要和jQuery、bs、easyui、ECharts等等进行搭配使用
* axios：主要和vue、vuex、vueRouter、element进行搭配使用

## 2. 原理
``` javascript
get(url){
    return new Promise((resolve,reject) => {
        let xhr = new XMLHttpRequest();
        xhr.responseType = "json";
        xhr.open("GET",url);
        xhr.setRequestHeader("Content-Type","application/x-www-form-urlencoded");
        xhr.send();
        xhr.onreadystatechage = function(){
            if(this.readyState === 4){
                if(this.status === 200){
                    resolve(this.response);
                }else{
                    reject(this);
                }
            }
        }
    })
}
```

## 3. 导入axios

### 1)&emsp;模块化
`$ cnpm install axios --save`

### 2)&emsp;script标签导入
`<script src = "xxx/axios.min.js"></script>`

## 4. 底层接口

> axios(config)：返回一个Ajax承诺对象

``` javascript
config是配置对象
{
    url,
    method,
    data,       //请求体参数【post请求】
    params,     //请求体参数【get请求】
    headers:{
        "Content-Type" : "application/x-www-form-urlencoded";
    },
    responseType : "json",
    withCredentials : false，     //默认不携带cookies
    baseURL,        //基础路径
    timeout,        //请求超时最大时间，单位为ms
    transformRequest : [(data) => {
        //在响应结果达到then/catch之前对结果进行处理，data为后端返回来的原始数据
    }]
    paramsSerializer : (params) => {
        return Qs.stringify(params,{arrayFromat : 'brackets'})
    },	//序列化params为查询字符串。get方式传递的参数需要拼接在浏览器地址栏的url的后面，只能为查询字符串
}
```



## 5.快捷接口（RESTFULL）



### 1.查询

`axios.get(url,config)`

### 2. 保存

`axios.post(url[,data][,config])`

### 3. 删除

`axios.delete(url[,config])`

### 4. 修改

`axios.put(url[,data][,config])`



## 6. response

> response 为then回调函数中的参数，也就是axios的请求成功的结果。这个结果不是后台直接返回的对象，而是二次封装的对象

```
response架构
	{
		status，
		statusText,
		data,
		headers,
		config,
		request
	}
```

## 7. axios默认配置

>  axios.defaults用于保存默认的配置信息，这个配置将对所有的axios对象产生影响

```
axios.defaults.baseURL				//默认基目录
axios.defaults.timeout				//默认超时时间
axios.defaults.transfromRequest[(data) => {}]		//当数据请求时默认对数据进行什么操作
axios.defaults.transformResponse([date] => {})		//当数据回应时默认对数据进行什么操作
axios.defaults.headers.common			//默认请求头
axios.defaults.headers.post			//当请求为post时，默认请求头
axios.defaults.headers.get			//当请求为get时，默认请求头
```

## 8. 拦截器

### 1）请求拦截器

``` javascript
axios.interceptors.request.use(function(config){
		// 在这里编写在请求之前你想执行的代码
		return config;
},function(error){
		return Promise.reject(error)
})
```

### 2）响应拦截器

``` javascript
axios.interceptors.response.use(function(response){
		// 在这里编写在响应获取之后你想执行的代码
		return response;
},function(error){
		return Promise.reject(error);
})
```

## 9. 常见的axios配置

``` javascript
let axios = require('axios');
let qs = require('qs');

// 配置默认基路径
axios.defaults.baseURL = "xxxxxx";
// 配置默认请求头
axios.defaults.headers.common["Content-Type] = "application/x-www-form-urlencoded";
// 配置默认请求数据处理
axios.defaults.transformRequest = [(data) => {
	return qs.stringify(data);
}]

//拦截器
axios.interceptors.response.use(function(response){
	return response;
},function(error){
	//当任何一个Ajax请求出现异常的话都会打印错误信息！
	console.log("error!!!!");
	return Promise.reject(error);
})
```

