---
title: OneDrive+宝塔面板搭建olaindex
date: 2018-07-12 10:40:12
categories: 
  - 日常捣鼓
---

**谷歌云创建VM实例。**

![](/images/OneDrive+宝塔面板搭建olaindex1.png)

谷歌云默认不可以使用xshell等第三方软件进行ssh登录，为了方便，我们修改下即可。

1. 点击右边进入ssh控制台，执行以下命令

   ```sh
   vi /etc/ssh/sshd_config
   ```

敲击键盘“i”进入编辑模式，找到下面两个参数去掉#号并修改

<!-- more -->

```sh
#PermitRootLogin prohibit-password
#PasswordAuthentication no

##去掉"#"号并修改
PermitRootLogin yes
PasswordAuthentication yes
```

保存后重启ssh这个时候就能用第三方终端登录软件进行连接

```sh
systemctl restart sshd.service              
```

哦，对了  记得更改ssh连接密码,根据提示进行

```sh
passwd
```

好了，可以直接用xshell搞了

**1、安装宝塔**

- Centos系统 

  ```sh
  yum install -y wget && wget -O install.sh http://download.bt.cn/install/install.sh && sh install.sh
  ```

  

- Ubuntu系统

  ```sh
  wget -O install.sh http://download.bt.cn/install/install-ubuntu.sh && sudo bash install.sh 
  ```

  

- Debian系统

  ```sh
  wget -O install.sh http://download.bt.cn/install/install-ubuntu.sh && bash install.sh
  ```

  

**安装挺快的，一分钟左右！！**

**接下来进入宝塔主页面：**[**http://xxxx:8888**](http://xxxx:8888)

**在弹出来的窗口安装一整套lnmp，耐心等待，mysql在本教程可以不安装。**

![](/images/OneDrive+宝塔面板搭建olaindex2.png)

全部完成后，再找到左侧软件管理-PHP管理-设置-安装Fileinfo扩展(非必需扩展，不过不安装的话，不保证安装程序能成功)。

然后同样的在PHP设置里找到禁用函数，删除exec、proc_open、proc_get_status和putenv函数，最后重启PHP

2、安装Composer（终端命令执行）

```sh
curl -sS https://getcomposer.org/installer | php mv composer.phar /usr/local/bin/composer 
```

​             

**3、安装程序**

​      我们先点击左侧网站，添加域名，此时网站根目录就是: /www/wwwroot/weiduni.xyz。

![](/images/OneDrive+宝塔面板搭建olaindex3.png)

**下一步，运行命令：**

```sh
cd /www/wwwroot/weiduni.xyz   #记住改成自己域名
git clone https://github.com/WangNingkai/OLAINDEX.git tmp 
mv tmp/.git .  #将.git目录移至当前目录
rm -rf tmp 
git reset --hard 
cp database/database.sample.sqlite database/database.sqlite  # 数据库文件
composer install -vvv # 这里确保已成功安装 composer ，如果报权限问题，建议给予用户完整权限。
chmod -R 777 storage 
chown -R www:www * # 此处 www 根据服务器具体用户组而定
php artisan od:install # 此处绑定域名需根据实际域名谨慎填写（包含http/https）
```



**5、伪静态设置**

点击域名设置-网站目录，运行目录选择public，并把防跨站的勾去掉并重启PHP。然后点击伪静态，输入以下代码：

```sh
location / {    try_files $uri $uri/ /index.php?$query_string; }
```

   

最后就可以打开域名进行安装配置了。

![](/images/OneDrive+宝塔面板搭建olaindex4.png)

注意回调地址redirect_uri需要是https地址，可以直接在宝塔开启免费SSL证书。如果你使用上面的一键申请绑定账号失败了，可以试试手动申请client_id、client_secret，申请方法→[传送门](https://github.com/WangNingkai/OLAINDEX/wiki/3.申请client_id、client_secret)。

后台地址：https://xx.com/admin，密码：12345678。

**其它设置**

```sh
#重置全部数据，删除数据库数据 
php artisan od:reset 

#重置OneDrive登陆账号 
php artisan od:logout     

#升级程序 
git pull 
composer install -vvv 
php artisan od:update
```



**特殊文件功能**

- 不建议创建和以下同名的文件夹和文件，否则会导致文件无法查看下载 README.md、HEAD.md 、.password 、.deny特殊文件使用 
- 在文件夹底部添加说明 在onedrive的文件夹中添加README.md文件，使用markdown语法。 
- 在文件夹头部添加说明 在onedrive的文件夹中添加HEAD.md 文件，使用markdown语法。 
- 加密文件夹 在onedrive的文件夹中添加.password文件，填入密码，密码不能为空。 
- 禁止访问文件夹 在onedrive的文件夹中添加.deny文件，该文件夹被禁止访问。

https://github.com/WangNingkai/OLAINDEX   开源地址