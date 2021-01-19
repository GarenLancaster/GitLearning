# GitLearning
学git用的

## 配置
先安装git，然后创建一个用来管理的文件夹，至少放3个文档

## git入门管理

1 在要管理的文件夹右键->git.bash

2 `git init` //进行初始化

3 `git status` //看文件夹的状态

4 `git add .` 或者 `git add 文件名` //加入暂存区，不算进版本好做回滚

5 `git commit -m 版本名`  // 加入版本库  |这里可能报错，要求输入邮箱和名字，看报错内容自行输入

6 `git status` //看不到之前的文件名了

7 `git log` //看各版本

## git进阶管理
### git版本切换

1 `git log` //找到要回滚的版本号 （commit后面接的一串乱码一样的东西）

2 `git reset --hard 上面找到的版本号` //回到该版本

//回到没回溯时的版本

3 `git reflog` //查看版本

4 `git reset --hard 版本号` //这个是比较短的 版本名前面那串短一点的乱码

//其他的切换用法跟上面大差不差，自己找吧

### branch操作
**dev分支开发时，模拟bug修复**

1 `git branch` 查看当前分支，标\*号的是处于的分支

2 `git branch 分支名` 创建一个分支

3 `git checkout 分支名` 进入这个分支

//在这完成一系列操作后，再checkout就能去别的分支，创建并去bug分支，修复bug

4 `git checkout master` 回到master分支

5 `git merge bug` 合并master和bug分支

6 `git branch -d bug` 删掉多余的bug分支

//去dev分支完成剩余开发,回到master

7 `git merge dev` 合并dev和已修复bug的master，*此时可能产生冲突，这时要手动修改冲突部分*

## git工作流
至少用两个branch，master和dev

只能在dev开发，master保存正式发行版

## git与GitHub

在本地已经init的时候

1 `git remote add origin(别名) github的http` 链接到github，*此时可能需要输入邮箱密码*

2 `git push -u origin 分支名` 想远程推送这个分支

从远程获取，先进入克隆的文件夹

3 `git clone github的http` 所有分支都被拉下来了，虽然只显示了master
