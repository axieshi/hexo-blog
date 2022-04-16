---
title: ESXI提示：失败 – “scsi00”的磁盘类型 2 不受支持或无效。请确保磁盘已导入
date: 2019-05-10 15:08:03
categories: 
  - 百思其解
---

**解决方法：**

需要用VMware的工具"vmkfstools"转换后才能让ESXi识别使用

1. 打开ESXi的命令行（shell），用root登录

2. 进入对应的数据存储库：cd /vmfs/volumes/[数据存储名称]

   例如我的：cd /vmfs/volumes/datastore1

   系统会自动转化为此数据存储库的UUID，因此当前文件夹变为：

   > /vmfs/volumes/5908d542-ce73335d-1aea-6c0b84a530d0/

3. 进入对应的虚拟机文件夹，若不确定，就用列出命令：ls ，然后：cd xx

<!-- more -->

4. ls 列出所有文件，其中".vmdk"后缀的就是虚拟磁盘文件，看着文件名打命令：

   vmkfstools -i [原磁盘文件全称] [新磁盘文件全称]

   例如我的：

   > vmkfstools -i VM-Svr2008R2x64.vmdk VM-Svr2008R2x64-new.vmdk -d thin

   其中【 -i 】作用是转换，【 -d thin 】作用是将新磁盘文件使用“精简置备模式”。

   然后经过漫长的等待，看着进度 Clone: 100% done. 才算完成转换。

   

**附上官方的命令说明：**

https://www.vmware.com/support/developer/vcli/vcli41/doc/reference/vmkfstools.html