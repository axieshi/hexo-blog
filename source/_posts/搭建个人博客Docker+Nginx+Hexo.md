---
title: Docker下搭建Nginx+Hexo个人博客
date: 2022-03-15 15:58:05
categories: 
  - 日常捣鼓
---

### 开篇
	本文内容仅作为练手与折腾，也是结合网上大佬的经验分享进行整理！（看得再多，不如实践出真章）

- ### Docker：环境隔离、资源复用。
- ### Hexo：快速、简洁且高效的博客框架 
  - [GitHub: Tommy Chen](https://github.com/hexojs/hexo)

- **想法**：
  
  - Hexo容器内产生的静态资源文件，宿主机目录内同步生成
  - Nginx容器与宿主机的映射，容器内也同步Hexo的静态资源文件
  - 搭配GitHub，文章随处无障碍提交更新
  
- **使用环境&应用**：
  
   - CentOS 7.9 + Docker + Hexo + Nginx + Git
   <!-- more -->

# 开始： 

- ## **Dcoker-CE**
    - 安装，国内服务器推荐使用 [阿里巴巴开源镜像站](https://developer.aliyun.com/mirror/)
    ```sh 
    ######step1:安装必要的一些系统工具
    sudo yum install -y yum-utils device-mapper-persistent-data lvm2
    ######step2:添加软件源信息
    sudo yum-config-manager --add-repo https://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
    ######step3:
    sudo sed -i 's+download.docker.com+mirrors.aliyun.com/docker-ce+' /etc/yum.repos.d/docker-ce.repo
    ######step4:更新并安装Docker-CE
    sudo yum makecache fast
    sudo yum -y install docker-ce
    ######step4:开启Docker服务
    sudo service docker start
    注意：
    官方软件源默认启用了最新的软件，您可以通过编辑软件源的方式获取各个版本的软件包。
    例如官方并没有将测试版本的软件源置为可用，您可以通过以下方式开启。同理可以开启各种测试 版本等。
    vim /etc/yum.repos.d/docker-ce.repo
    将[docker-ce-test]下方的enabled=0修改为enabled=1
    安装指定版本的Docker-CE:
    Step 1: 查找Docker-CE的版本:
    yum list docker-ce.x86_64 --showduplicates | sort -r
    Loading mirror speeds from cached hostfile
    Loaded plugins: branch, fastestmirror, langpacks
    docker-ce.x86_64            17.03.1.ce-1.el7.centos            docker-ce-stable
    docker-ce.x86_64            17.03.0.ce-1.el7.centos            docker-ce-stable
    Available Packages
    Step2: 安装指定版本的Docker-CE: (VERSION例如上面的17.03.0.ce.1-1.el7.centos)
    sudo yum -y install docker-ce-[VERSION]
    ```



- ## **Hexo**
  1. **拉取node镜像（hexo基于nodejs环境）** 
     - 命令前加sudo获取root执行权限，本文服务器处于root用户，可忽略。

     ```sh
     [root@centos ~]# sudo docker pull node
     Using default tag: latest
     latest: Pulling from library/node
     ........
     Digest: sha256:3f8047ded7bb8e217a879e2d7aabe23d40ed7f975939a384a0f111cc041ea2ed
     Status: Downloaded newer image for node:latest
     ```
  2. **进入node镜像**
     
      1. 查看镜像ID，并创建容器
      2. 同时将本地目录/root/blog/hexo-blog（先创建），映射到容器目录/usr/hexo-blog
      3. 安装hexo-cli
      
        ```sh
        [root@centos ~]#docker images -a
        REPOSITORY   TAG       IMAGE ID       CREATED      SIZE
        node         latest    8778d77035e2   2 days ago   991MB
      
        [root@centos ~]#docker run -itd -v /root/blog:/root/blog node
      
        [root@centos ~]#docker ps -a
        CONTAINER ID    IMAGE     ...     STATUS     PORTS     NAMES
        fccf0c8c6ad1     node     ...       Up                hexo-blog
        [root@centos ~]#docker exec -it fcc bash            ##进入容器
        root@fccf0c8c6a:/#npm install hexo-cli -g			##安装hexo-cli
        root@fccf0c8c6a:/#hexo init /root/blog/hexo-blog    ##在指定目录初始化hexo
        root@fccf0c8c6a:/#cd /root/blog/hexo-blog && npm install
        root@fccf0c8c6a:/#hexo clean && hexo g              ##清理缓存&生成新的静态目录./public
        root@fccf0c8c6a:/#exit			                    ##退出容器回到宿主机
        ```



- ## **Nginx**
  1. **拉取Nginx镜像**
  ```sh 
  [root@centos ~]# sudo docker pull nginx
  Using default tag: latest
  latest: Pulling from library/nginx
  ........
  Status: Downloaded newer image for nginx:latest
  docker.io/library/nginx:latest
  ```
  2. **进入Nginx容器**
     1. 查看镜像ID，并创建容器
     ```sh 
     [root@centos ~]# docker images -a
     REPOSITORY   TAG       IMAGE ID       CREATED       SIZE
     node         latest    8778d77035e2   2 days ago    991MB
     nginx        latest    12766a6745ee   2 weeks ago   142MB
     ```
     2. 映射对应端口和目录: -p , -v
        - 看个人需求，我这里直接映射80端口
        - 静态文件目录：/root/blog/hexo-blog/public，映射到nginx容器目录：/usr/share/nginx/html

        ```sh 
        [root@centos ~]# docker run -itd -p 80:80 -v /root/blog/hexo-blog/public:/usr/share/nginx/html nginx
        [root@VM-4-8-centos hexo-blog]# docker ps -a		##查看容器列表，up状态即可
        CONTAINER ID     IMAGE    ...   STATUS               PORTS                     ...
        2106814a64de     nginx    ...     up      0.0.0.0:81->80/tcp, :::81->80/tcp    ...
        fccf0c8c6ad1     node     ...     up                                           ...
        ```

  3. **搭建完成**
      - 如果完成后无法访问，这时候就要检查下主机防火墙是否对端口未放行，还有像腾讯云主机，它在官网控制台有自己的安全组规则，需要手动设置放行。
      - 浏览器访问 http://x.x.x.x:port，成功如图：
      ![](/images/搭建个人博客Docker+Nginx+Hexo1.png)



- ## **文章更新、自定义...**
  1. 发表一篇文章，容器内执行
  ```sh
  root@fccf0c8c6ad1:# cd /root/blog/hexo-blog
  root@fccf0c8c6ad1:# hexo new "hello"
  INFO  Validating config
  INFO  Created: /blog/source/_posts/hello.md		##显示成功创建一篇名为"hello.md"的文章
  ```
  2. 增加文章等涉及静态资源的改动时，需要让Hexo重新生成一份，以便更新页面显示
  ```sh 
  root@fccf0c8c6ad1:# cd /root/blog/hexo-blog
  root@b1b0d1107917:# hexo clean && hexo g
  [root@centos ~]# docker restart 21		##需要重启Nginx容器进行资源同步
  ```

  3. 更加详细方法、教程，如更换主题等，请移步[Hexo作者主页]((https://hexo.io/zh-cn/))



- ## **GitHub（忙，待补充..）**





## 尾声：整理不易，欢迎静心敲之；同时希望自己坚持学习、持续输出，与博客一同成长！！

- 待有时间再研究吧步骤整理成自动化脚本。