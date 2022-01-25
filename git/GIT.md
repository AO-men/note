# GIT

## 1. git介绍

`git是一个免费的、开源的分布式版本控制工具`

> 版本控制工具：可以用来记录文件的历史修改记录。

## 2. 安装 

```markdown
略
```

### 3. git 常用命令



### 3.1 简单的常用命令

`git config --global user.name`：设置用户名

`git config --global user.email`：设置邮箱

> 可在电脑本地c:/user/.gitconfig文件查看设置的用户名以及邮箱信息

`git init`：初始化git本地库

`git status`：查看本地库状态

`git add 文件名 `：将文件提交的暂存区

> 也可以使用git add . 提交所有文件

`git commit -m '提交注释'`：将暂存区文件提交到本地库中

`git log`：查看版本详细信息

> 也可以使用git reflog 查看版本精简版的详细信息

### 3.2 版本穿梭命令

`使用git reflog 查看版本号`

`git reset -hard 版本号`：版本穿梭

> git 控制版本的原理是底层的指针指向不同的版本。



### 3.2 分支 

`在版本控制过程中，同时推进多个任务，为每个任务，我们就可以创建每个任务的单独分支。使用分支意味着程序员可以把自己的工作从开发主线上分离开来，开发自己的分支的时候，不会影响主线分支的运行。`

> 分支底层也是指针的使用

`git branch -v`：查看分支

`git branch 分支名`：创建分支

`git checkout 分支名`：切换分支

`git merge 分支名`：合并分支（将命令中的分支合并到当前分支）

```markdown
# 合并冲突
	当两个分支修改了同一份代码时，将会产生冲突。需手动合并
```



## 3. 团队协作



### 3.1 github

`git remote add 别名 远程地址`：创造别名

`git remote -v ` ：查看当前所有远程地址别名

<img src="D:\软件\typora\笔记资源\图片资源\image-20220124125339777.png" width="80%">

  `git push  别名 分支名`