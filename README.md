[toc]

> [Git教程 - 廖雪峰](https://www.liaoxuefeng.com/wiki/896043488029600)

> [Git远程操作详解 - 阮一峰](http://www.ruanyifeng.com/blog/2014/06/git_remote.html)

## Git是什么？

Git是目前世界上最先进的分布式版本控制系统

## 安装Git

[在Windows上安装Git - 廖雪峰](https://www.liaoxuefeng.com/wiki/896043488029600/896067074338496)

官网下载地址：https://git-scm.com/download/win

## 入门

### 1. 创建版本库

> [创建版本库 - 廖雪峰](https://www.liaoxuefeng.com/wiki/896043488029600/896827951938304)

* 版本库又名仓库，英文名repository
* 步骤
  * 切换到合适的目录
    * 命令 `cd` 可以切换文件夹，命令 `mkdir` 可以新建文件夹
  * 初始化一个Git仓库。使用 `git init` 命令
  * 添加文件到Git仓库，分两步：
    * 使用命令 `git add <file>`。注意，可反复多次使用，添加多个文件
    * 使用命令 `git commit -m <message>`，完成
    * 例：添加并提交一个readme.txt文件
      * 命令 `ls` 可以查看当前文件夹中的文件
  
### 2. 修改文件后会发生什么？

> [时空机穿梭 - 廖雪峰](https://www.liaoxuefeng.com/wiki/896043488029600/896954074659008)

* 使用 `git status` 命令，可以发现文件状态有变化
* 如果 `git status` 告诉你有文件被修改过，用 `git diff` 可以查看修改内容
* 可以更新版本。执行命令 `git add <file>` 并  `git commit -m <message>`

### 3. 版本回退

> [版本回退](https://www.liaoxuefeng.com/wiki/896043488029600/897013573512192)

#### 3.1 本地回退

* 先看一下提交记录，命令 `git log`：

```git
  $ git log
  commit a0c5fe209704e56a7e00e79f3de7b5d67b33c31a (HEAD -> master)
  Author: ------- <----------@gmail.com>
  Date:   Mon Aug 24 20:11:47 2020 +0800
  
    append GPL
  
  commit fab9c50d48b54085c54211d33737f4662a2a4488
  Author: ------- <----------@gmail.com>
  Date:   Mon Aug 24 20:04:35 2020 +0800
  
    add a word distributed
  
  commit 415ac66de426d66b906ad06afd7d4dec0ec7ea10
  Author: ------- <----------@gmail.com>
  Date:   Mon Aug 24 19:54:13 2020 +0800
```
* 注意 commit 后面那一串东西是 commit id（版本号）

* `HEAD` 是指针，指向的版本就是**当前版本**。因此，Git允许我们在版本的历史之间穿梭，使用命令 `git reset --hard <commit_id>`
  * **先查看提交历史**，命令 `git log` ，以便确定要**回退**到哪个版本。
    * 可以用 `<commit_id>` 指定版本，也可以用如下符号：
      * `HEAD^` 上一个版本
      * `HEAD^^` 上上个版本
      * `HEAD~100` 往上100个版本
    * 注意：会 “丢失” 后续版本
  * 要**重返未来**，用 `git reflog` 查看**命令历史**，以便确定要回到未来的哪个版本

* 命令 `cat <file>` 可以看看该文件内容

#### 3.2 远程回滚

* 如果你的错误提交已经推送到自己的远程分支了，那么就需要回滚远程分支了
  * 先本地回退
    * `git reset --hard <commit_id>`
  * 再强制推送到远程分支
    * `git push -f origin master ## 这里假设只有一个master分支`

### 4. Git的基本工作原理

> [工作区和暂存区 - 廖雪峰](https://www.liaoxuefeng.com/wiki/896043488029600/897271968352576)

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/5a99cd7e5e0b4ae58746887a2e4c7b84~tplv-k3u1fbpfcp-zoom-1.image)

* **说明**
  * **工作区**（Working Directory）：就是我们在电脑里能看到的目录，比如刚刚创建的 `learngit` 文件夹就是一个工作区
  * **版本库**（隐藏文件夹`.git`）
    * 暂存区（stage，或者叫index）
    * master 分支（唯一）
* **命令**
  * `git add <file> `：把文件从工作区添加到暂存区
  * `git commit -m <message>`：把文件从暂存区移动到分支

### 5. Git管理的是修改，而不是文件

> [管理修改 - 廖雪峰](https://www.liaoxuefeng.com/wiki/896043488029600/897884457270432)

* Git是如何跟踪修改的？
  * 每次修改，如果不用 `git add` 到暂存区，那么 `git commit` 时就不会提交到分支中

### 6. 撤销修改

> [撤销修改 - 廖雪峰](https://www.liaoxuefeng.com/wiki/896043488029600/897889638509536)

* **场景 1**：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令 `git checkout -- <file>`。
  * 把该文件在工作区的修改全部撤销：会回到最近一次 `git commit` 或 `git add` 时的状态
  * 本质：用版本库里的版本替换工作区的版本 √
* **场景 2**：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步：
  * 第一步：用命令 `git reset HEAD <file>`，就回到了场景1
  	* 也就是说，`git reset` 命令既可以回退版本，也可以把暂存区的修改回退到工作区 √
  * 第二步：按场景1操作【注意别漏了】
* **场景 3**：已经提交了不合适的修改到版本库时，想要撤销本次提交，参考版本回退一节，不过**前提**是没有推送到远程库

### 7. 删除

>[删除文件 - 廖雪峰](https://www.liaoxuefeng.com/wiki/896043488029600/900002180232448)

* (1) 如果你用的 `rm <file>` 删除文件，那就相当于只删除了工作区的文件
  * 如果想要**恢复**，直接用`git checkout -- <file>` 就可以把版本库的版本恢复到工作区
  * 但是要小心，你只能恢复文件到最新版本，你会丢失最近一次提交后你修改的内容
* (2) 如果你用的是 `git rm <file>` 删除文件，那就相当于不仅删除了文件，而且还添加到了暂存区【√】
  * 如果想要**恢复**，需要先 `git reset HEAD <file>`，然后再 `git checkout -- <file>`
* (3) 如果你想**彻底**把版本库的删除掉，先 `git rm <file>` ，再 `git commit -m <message>`就OK了【√】

## 远程仓库：Github

### 1. 通过SSH连接远程仓库：

>[远程仓库 - 廖雪峰](https://www.liaoxuefeng.com/wiki/896043488029600/896954117292416)

* 步骤：
  * 本地电脑生成 ssh 密钥——公钥私钥
    * 打开 `git bash`，输入下面命令生成 email 的 ssh 密钥：`ssh-keygen -t rsa -C "youreamil"`，一路确认
    * 根据 `git bash` 的回复的秘钥目录找到私钥 `id_rsa` 和公钥 `id_rsa.pub`
  * 复制公钥 `id_rsa.pub` 内容，到第三方git仓库设置中绑定 `ssh key`

### 2. Github远程库配置

#### 场景1：先有本地库，后有远程库的时候，如何关联远程库？

>[添加远程库 - 廖雪峰](https://www.liaoxuefeng.com/wiki/896043488029600/898732864121440)

> 现在的情景是，你已经在本地创建了一个 `Git` 仓库后，又想在 `GitHub` 创建一个 `Git` 仓库，并且让这两个仓库进行远程同步，这样，`GitHub` 上的仓库既可以作为备份，又可以让其他人通过该仓库来协作，真是一举多得。

* 要关联一个远程库，使用命令 `git remote add origin git@server-name:path/repo-name.git`；
  
  * 例如：`$ git remote add origin git@github.com:bobo6668/learngit.git`
* 关联后
  * 使用命令 `git push -u origin master` 第一次推送 `master` 分支的所有内容；
    * 第一次使用 Git 的 `clone` 或者 `push` 命令连接 GitHub 时，会得到一个SSH警告，忽略即可
  * 此后，每次本地提交后，只要有必要，就可以使用命令 `git push origin master` 推送最新修改；
  
* Debug：push 的时候遇到了 Warning
  ```python
  Warning: Permanently added the RSA host key for IP address '13.229.188.59' to the list of known hosts.
  ```
  * 解决方法：在host文件中添加一句话即可
  ```python
  13.229.188.59 github.com
  ```

#### 场景2：我们从零开发，那么最好的方式是先创建远程库，然后，从远程库克隆

>[从远程库克隆 - 廖雪峰](https://www.liaoxuefeng.com/wiki/896043488029600/898732792973664)

* 在 `Github` 上先创建一个仓库，如 `gitSkills`，并得到地址
   ![仓库地址](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/34f9d7cb68d44e398cd58c32c12137ac~tplv-k3u1fbpfcp-zoom-1.image)
* 使用 `git clone` 命令克隆
  * `git clone git@github.com:bobo6668/gitSkills.git`
  * 注意 `Git` 执行克隆操作时，克隆到当前路径`pwd`那里去了
* `Git` 支持多种协议，包括 `https` ，但 `ssh` 协议速度最快

> 更多资料：可以看看 Github 的 [Hello World](https://guides.github.com/activities/hello-world/#intro)

## 分支管理

![分支管理示意图](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/e7e2a0ab6e6b401f8bd85d1b43ded01e~tplv-k3u1fbpfcp-zoom-1.image)

### 1. 创建与合并分支

> [创建与合并分支 - 廖雪峰](https://www.liaoxuefeng.com/wiki/896043488029600/900003767775424)

---
2020年8月26日21:10:10

* `Git` 鼓励大量使用分支：
  * 查看分支：`git branch`
  * 创建分支：`git branch <name>`
  * 切换分支：`git switch <name>`（不建议`git checkout <name>` ）
    * *创建+切换分支：`git switch -c <name>`（不建议`git checkout -b <name>`）
  * 合并某分支到当前分支：`git merge <name>`
  * 删除分支：`git branch -d <name>`
  
### 2. 解决冲突

> [解决冲突 - 廖雪峰](https://www.liaoxuefeng.com/wiki/896043488029600/900004111093344)

* 当 `Git` 无法自动合并分支时
  * 必须先解决冲突：把 `Git` 合并失败的文件手动编辑为我们希望的内容
  * 解决冲突后，再提交，合并完成
* 用 `git log --graph` 命令可以看到分支合并图

### 3. 分支管理策略

> [分支管理策略 - 廖雪峰](https://www.liaoxuefeng.com/wiki/896043488029600/900005860592480)

* `Git` 分支十分强大，在团队开发中应该充分应用
* 合并分支时
  * 默认是 `fast forward` 合并（如 `git merge dev`）。合并后会删除分支 `dev`，看不出来曾经做过合并
  * 加上 `--no-ff` 参数（即 `git merge --no-ff -m "merge with no-ff" dev`）就可以用普通模式合并。合并后的历史有分支，能看出来曾经做过合并【√】
  ![](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/7878b4bc89784dec8a03929593520f09~tplv-k3u1fbpfcp-zoom-1.image)
* 分支管理策略：
![分支管理策略](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/42755a76f21f42c2872565f0015a77a6~tplv-k3u1fbpfcp-zoom-1.image)

## 标签管理

Git的标签是版本库的快照，方便未来取出那个打标签的时刻的历史版本

### 1.创建标签

> [创建标签 - 廖雪峰](https://www.liaoxuefeng.com/wiki/896043488029600/902335212905824)

> [打标签 - Git](https://git-scm.com/book/zh/v2/Git-%E5%9F%BA%E7%A1%80-%E6%89%93%E6%A0%87%E7%AD%BE)

* 打标签
  * `git tag v0.1`
* 补打标签
  * 找到历史提交的commit id
  * `git tag v0.9 f52c633`
* 创建带有说明的标签，用`-a`指定标签名，`-m`指定说明文字
  * `git tag -a v0.1 -m "version 0.1 released" 1094adb`
* 查看所有标签
  * `git tag`
* 查看某标签的说明内容
  * `git show <tagname>`
* 同步到github
  * 某标签
    * `git push origin <tagname>`
  * 所有标签
    * `git push origin --tags`


## Git 常用命令速查表

> [打开Git的正确姿势](https://juejin.im/post/6844903749631098893)

![](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/fad2c17e4cc644729dd20b5dec7a5334~tplv-k3u1fbpfcp-zoom-1.image)

当然，可以用 `git help` 非常方便地查看所有命令

### `git mv` 命令

`git mv` 命令用于移动或重命名文件，目录或符号链接。

参考1：[git mv命令 - 易百教程](https://www.yiibai.com/git/git_mv.html)

官网：[git-mv - Move or rename a file, a directory, or a symlink](https://git-scm.com/docs/git-mv/en)

### Git 移动操作

参考：[Git移动操作 - 易百教程](https://www.yiibai.com/git/git_move_operation.html)

  (To be continued...)