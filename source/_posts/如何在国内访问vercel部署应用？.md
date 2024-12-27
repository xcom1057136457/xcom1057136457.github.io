---
title: 如何在国内访问vercel部署应用？
date: 2024-12-27 10:39:27
---

## 1. 将腾讯云域名DNS解析转移到Cloudflare进行

### 1.1 准备一个自己的域名

既然是要代理到原来的域名，所以得准备一个自己在国内服务商购买的域名。具体的流程应该都清楚的吧也就不多说了，在腾讯云里面你只用登录控制台然后搜“域名注册”四个字就会提示你怎么操作了。唯一的要求就是你要付钱，我建议便宜点就行。

![](https://raw.githubusercontent.com/xcom1057136457/DrawingBed/main/20241227104149.png)

### 1.2 登录Cloudflare控制台并添加站点

前往[https://www.cloudflare.com/zh-cn](https://www.cloudflare.com/zh-cn)进行注册，有账号的直接登录。

进来之后看到如下页面，我之前加过一个站点所以会弹出来，没有的就可以不用管：

![](https://raw.githubusercontent.com/xcom1057136457/DrawingBed/main/20241227104320.png)

然后点击右侧的这个添加站点就好了，进入下一步，输入你要加的域名（就是你之前在腾讯云上面买好的）然后点击继续：

![](https://raw.githubusercontent.com/xcom1057136457/DrawingBed/main/20241227104345.png)

然后选择计划，一般无特殊需求直接白嫖然后点击继续：

![](https://raw.githubusercontent.com/xcom1057136457/DrawingBed/main/20241227104403.png)

接下来 Cloudflare 会自动扫描你的部分dns记录。我这个域名是刚刚买的还没有进行一些解析的操作，所以是没有记录的。点击继续：

![](https://raw.githubusercontent.com/xcom1057136457/DrawingBed/main/20241227104421.png)

然后最关键的点来了，**Cloudflare**会自动生成两条**dns**地址，就是下面两个云右边的字符串，你得拿着这两个地址去换掉腾讯云原本的解析：

![](https://raw.githubusercontent.com/xcom1057136457/DrawingBed/main/20241227104503.png)

至此，Cloudflare部分的工作告一段落。

### 1.3 在腾讯云域名管理控制台更改DNS服务器解析

接下来启动腾讯云域名管理后台[https://console.cloud.tencent.com/domain](https://console.cloud.tencent.com/domain)。我这边有一个是已经解析到cloudflare所以DNS状态变成了其他，我现在要改的这个nullvideo.cn待会儿也会变成其他。

![](https://raw.githubusercontent.com/xcom1057136457/DrawingBed/main/20241227104616.png)

点击“解析”按钮右边的“管理”按钮，进入域名管理页，找到DNS解析部分：

![](https://raw.githubusercontent.com/xcom1057136457/DrawingBed/main/20241227104645.png)

然后点击“修改DNS服务器”，把刚刚Cloudflare给我们的两个DNS地址黏贴到原本的DNS地址处：

![](https://raw.githubusercontent.com/xcom1057136457/DrawingBed/main/20241227104708.png)

保存然后等待dns缓存刷新即可，这可能需要1-24小时因为每个域名体质不一样。

然后回到 Cloudflare 控制台就好了。至此，更换完成了。

## 2. 在Vercel上为自己部署好的前端应用添加新的域名解析

### 2.1 将自己的应用通过vercel进行部署

这个就不在这里展开说了，你只要登录vercel的官网[https://vercel.com](https://vercel.com)注册或者登录账号，然后自己跟着它的指示一步步来，最不济可以去查一查资料。。。反正这里不展开说了，默认大家都已经有一个部署好了的vercel应用了。

### 2.2 去Cloudflare添加域名解析记录

在 Cloudflare 添加CNAME类型的解析，比如这个项目就是把nullvideo.cn重定向到null-video.vercel.app，并打开 proxy 服务。我在这边为了对应根路径访问和www访问，两个都加上了。

![](https://raw.githubusercontent.com/xcom1057136457/DrawingBed/main/20241227105001.png)

### 2.3 对部署好的vercel应用添加除了自带域名的新域名解析

进入到部署好了的项目的主页，可以看到一个“Domain”的按钮，点击进入：

![](https://raw.githubusercontent.com/xcom1057136457/DrawingBed/main/20241227105035.png)

然后进入之后，输入你买好的域名然后点击Add：

![](https://raw.githubusercontent.com/xcom1057136457/DrawingBed/main/20241227105054.png)

选择默认的方案，也就是把根域名和www解析一起加上。

![](https://raw.githubusercontent.com/xcom1057136457/DrawingBed/main/20241227105112.png)

添加之后会进行校验，校验完成了之后就可以进行访问了。。。看起来是这样，实际上还是有问题的！！！

### 2.4 Vercel + Cloudflare = 重定向次数过多解决方案

你把域名解析添加好了，校验也通过了，然后你会直接点击访问：

![](https://raw.githubusercontent.com/xcom1057136457/DrawingBed/main/20241227105156.png)

没错，你会遇到重定向次数过多的问题。

这其实是Cloudflare为添加的站点加密模式设置错误导致的。

进入 Cloudflare Dashboard，点击有问题的域名，打开左侧的 SSL/TLS 设置，在 Overview 中设置加密模式为完全或完全（严格）即可。

![](https://raw.githubusercontent.com/xcom1057136457/DrawingBed/main/20241227105236.png)

这样子之后你的vercel应用应该是可以正常的在国内进行访问了。