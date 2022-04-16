---
title: Git的使用与碰壁
date: 2022-2-10 21:08:03
categories: 
  - 日常捣鼓
---
![](/images/Git的使用与碰壁1.png)
- 图出自网络
- 图解：（暂时抛去官方解释不看，先以个人理解作为出发点，有助于探索学习）
#### (个人)工作区：项目代码本地存放、个人代码维护修改更新等相关操作存放区；
#### Index索引：看作缓存作用，存放着工作区所有的临时操作记录信息；
#### 存储仓库：同样视为缓存作用，但它存放的是具体修改后的文件，而不是操作记录；
#### 远程仓储：最终项目代码公示、长期曝光区，代码托管，无人值守，存取便利。

>流程1：**工作区**修改后，用`git add`将临时的操作记录存放于**Index索引区**，这时通过`git commit`可将Index区存放的操作记录对应的文件提交到**存储仓库**（如果中途反悔，可使用`checkout`退回提交的代码）；接着中间存储仓库将完整/改变的代码文件整体push到**远程公共仓储**上托管。
>
>`将流程1看作是同事甲的操作，当在团队协作时，同事乙也是维护人之一，此时乙想要同步得到甲更新后的文件。`
>
>流程2：同事乙可通过pull / clone将文件传到他个人工作区，也就是本机电脑上；
>- pull：直接复制回工作区，加入的内容和现存内容之间会进行对比记录更新；
>- clone：将整个项目文件复制一份到本机，包含.git目录，相当于项目是在本地主机从无到有。


### 工作区、Index、仓库区三者间
  **git add 绿字会变红字(撤销add)**
    `git reset HEAD` 		后面什么都不跟的，就是上一次add 里面的内容全部撤销
    `git reset HEAD XXX` 	后面跟文件名，就是对某个文件进行撤销

  **git commit 撤销操作**
    `git reset --soft HEAD`       仅是撤回commit操作，您写的代码仍然保留。

  **git reset 其他参数说明：**
    `--mixed`        不删除工作空间改动代码，撤销commit，并且撤销git add . 这个为默认参数,
    `git reset --mixed HEAD^` 和 `git reset HEAD^` 效果是一样的。
    `--soft`         不删除工作空间改动代码，撤销commit，不撤销git add . 
    `--hard`         删除工作空间改动代码，撤销commit，撤销git add . 注意完成这个操作后，就恢复到了上一次的commit状态。 
    `git commit --amend`      改commit注释，此时会进入默认vim编辑器，修改注释完毕后保存就好了。

  **红字变无 (撤销没add修改)**
    `git checkout -- 文件`

  **要删除指定的commit**
    `git log`     查找到要删除提交记录的上一条提交的commit的id，执行
    `git rebase -i (commit的id)`
    弹出框中上方有很多pick记录，对应的commit提交，将需要删除的commit的pick改为drop，保存
    `git log`可以看到所删记录没有了

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
  `git remote -v`                                   查看远程仓库详细信息，可以看到仓库名称
  `git remote remove orign`             删除orign仓库（如果把origin拼写成orign，删除错误名称仓库）
  `git remote add origin 仓库地址`       重新添加远程仓库地址
  `gti push -u origin master`             提交到远程仓库的master主干

**持续...**

