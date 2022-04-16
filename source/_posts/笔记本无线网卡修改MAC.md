---
title: 笔记本无线网卡修改MAC
date: 2021-02-08 20:32:05
categories: 
  - 日常捣鼓
---

#### 思路：对于有线网卡，我们可以直接通过修改属性NetworkAddress的值达到目的；但鉴于无线网卡有些是没有这个值的，于是我们可以通过注册表为该无线网卡添加NetworkAddress值达到目的；

![](/images/笔记本无线网卡修改MAC1.png)

### 开始操作：

1. “WIN+R”打开运行 - regedit

2. 打开该路径：

   > 计算机\HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Class\{4d36e972-e325-11ce-bfc1-08002be10318}

<!-- more -->

   我们可以看到好多00xx开头的文件夹，这些表示电脑安装的各个网卡，我的有线网卡是0002，下有一个值DriverDesc对应我的有线网卡名称：ASIX AX88179 USB 3.0 to Gigabit Ethernet Adapter；按照对应关系我们找到无线网卡的目录为：0001

3. 这时候我们对照有线网卡0002为无线网卡0001添加对应值；

4. 有线网卡NetworkAddress的值在路径：~\0002\Ndi\params

5. 同样，我们给无线网卡~\0001\Ndi\params路径下也添加NetworkAddress这个值；

6. 这是0002有线网卡的属性值路径。右侧的6个属性值同样添动加到无线网卡的NetworkAddress项里![](/images/笔记本无线网卡修改MAC2.png)

7. 最后在无线网卡的~\0001主目录下右键再另添加一个项NetworkAddress值为你需要修改的MAC



最后补下无线网卡修改后的图：

![](/images/笔记本无线网卡修改MAC3.png)

![](/images/笔记本无线网卡修改MAC4.png)

![](/images/笔记本无线网卡修改MAC5.png)

PS：修改的MAC只能是以下段(2、6、A、E)

02xx-xxxx-xxxx-xxxx

06xx-xxxx-xxxx-xxxx

0Axx-xxxx-xxxx-xxxx

0Exx-xxxx-xxxx-xxxx

最后重启生效。