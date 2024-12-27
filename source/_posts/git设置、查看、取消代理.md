---
title: git设置、查看、取消代理
date: 2024-12-27 10:54:03
---

> 设置代理：前提你得有代理哈

### 设置代理：

````javascript
//http || https
git config --global http.proxy 127.0.0.1:7890
git config --global https.proxy 127.0.0.1:7890

//sock5代理
git config --global http.proxy socks5 127.0.0.1:7891
git config --global https.proxy socks5 127.0.0.1:7891
````

### 查看代理：

````javascript
git config --global --get http.proxy
git config --global --get https.proxy
````

### 取消代理：

````javascript
git config --global --unset http.proxy
git config --global --unset https.proxy
````