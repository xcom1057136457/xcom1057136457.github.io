---
title: Linux下配置Mysql
date: 2019-10-10 14:03:39
---
## 1.安装MySQL

在普通用户下【不要使用超级管理员！！】使用如下命令下载mysql   

`sudo apt-get install mysql-server`   

根据提示输入用户名、密码即可【密码设置成好记忆的，以后会经常使用】   

安装完成

## 2.配置MySQL

### 进入MySQL

使用如下命令行，并根据提示输入密码【密码就是你刚刚设置好的密码】，登陆进入MySQL   

`mysql -uroot -p`   

### 配置MySQL用户和环境

在MySQL中分别执行代码

> GRANT ALL PRIVILEGES ON *.* TO root@'%' IDENTIFIED BY '你刚刚设置好的密码' with grant option;

执行完代码出现`Query OK， 0 rows affected, 1 warnning`即可

> flush privileges;

更新权限，执行代码没有出现错误即可   
   
CTRL + z提出MySQL，执行如下命令进入/etc/mysql/mysql.conf.d/目录，对mysqld.cnf文件的内容进行更改，要使用sudo，否则更改后无法退出

```
cd /                            //从家目录退回到根目录目录
cd /etc/mysql/mysql.conf.d      //进入到mysql.conf.d目录
sudo vim mysqld.cnf             //使用超级管理员权限编辑该文件

将43行的bind-address后面的值改为 0.0.0.0

保存退出
```
至此mysql配置完成

### 重启MySQL

`sudo service mysql restart`

## 3. 使用navicat连接服务器上的MySQL

1.首先在阿里云控制台将**3306端口**开放

---
2.使用navicat连接mysql
![20190904185515592.png](https://i.loli.net/2019/10/10/H3jcLVfXKB9vTmu.png)

填完点击确认即可
