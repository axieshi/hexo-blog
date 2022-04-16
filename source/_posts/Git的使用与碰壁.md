---
title: Git的使用与碰壁
date: 2022-2-10 21:08:03
categories: 
  - 日常捣鼓
---

`git status` 查看改变的文件列表
`git status -s` 查看改变的文件列表简化版
`git diff`或者`git diff filename`查看不在缓冲区的文件发生的改变
`git diff --cached`或者`git diff --staged`查看缓冲区的文件发生的改变
`git diff HEAD`是`git diff`和`git diff --cached`的合并

### 报错信息“fatal: 'origin' does not appear to be a git repository...”
$ git push -u origin master
fatal: 'origin' does not appear to be a git repository
fatal: Could not read from remote repository.

是因为远程不存在origin这个仓库名称，可以使用如下操作方法，查看远程仓库名称以及路径相关信息，可以删除错误的远程仓库名称，重新添加新的远程仓库；
`git remote -v`                                   ： 查看远程仓库详细信息，可以看到仓库名称
`git remote remove orign`                  ：删除orign仓库（如果把origin拼写成orign，删除错误名称仓库）
`git remote add origin 仓库地址`       ： 重新添加远程仓库地址**
`gti push -u origin master`               ： 提交到远程仓库的master主干**

**持续...**