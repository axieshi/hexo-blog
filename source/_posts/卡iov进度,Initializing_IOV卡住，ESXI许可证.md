---
title: 卡iov进度,InitializingIOV卡住,ESXI许可证
date: 2019-05-10 13:28:21
categories: 
  - 百思其解
---

VMware vSphere 6 Enterprise Plus(6.7许可证)
0A65P-00HD0-3Z5M1-M097M-22P7H

**安装**

**Initializing IOV卡住**

如果你是4代酷睿, 你会遇到第一个bug. 卡在Initializing IOV. 解决方案就是进入之前按住shift + O. 然后输入noIOMMU, 注意最前面有个空格的. 回车.
![](/images/卡iov进度,InitializingIOV卡住,ESXI许可证.png)

之后安装好了, 要启动之前, 还需要shift + O. 然后输入

<!-- more -->

> noIOMMU

最后进入系统后, 管理界面 > F2 进入配置 > 登录 > 故障解决选项 > 启用Esxi Shell > alt + F1 > 登录 > 执行

> esxcli system settings kernel set --setting=noIOMMU -v TRUE 

alt + F2退出, 然后可以再禁用ESXI shell.