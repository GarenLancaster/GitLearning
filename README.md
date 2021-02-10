# GitLearning
学git用的

有序号的按照序号做，没序号的看看、自取

## 配置
1 先安装git，win下安装时一直下一步就ok了，其他系统的用命令行

2 创建一个用来管理的文件夹，至少放3个文档

## Git入门管理
管理一个文件并实现做出第一个版本

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

#### git工作流
至少用两个branch，master和dev

只能在dev开发，master保存正式发行版

## Git与GitHub

在本地已经init的时候

1 `git remote add origin(别名) github的http` 链接到github，*此时可能需要输入邮箱密码*

2 `git push -u origin 分支名` 想远程推送这个分支

从远程获取，先进入克隆的文件夹

3 `git clone github的http` 所有分支都被拉下来了，虽然只显示了master

## Rebase，变基
### 基本操作
进行4次commit，产生4个版本，rebase将后面3个版本合成为一个版本

`git rebase  -i HEAD~3` 完成合并，此后将出现要修改的文件，将文件中出第二版以外的pick改成s，保存退出,接着出现commit信息文件，修改commit内容接着退出。

### rebase还可以将branch并入，简化流程

在dev分支执行 `git reabse main` 就能将dev接入main

接着转到main `git merge dev` 完成合并，此时main和dev在一条分支上

### 其他应用
- 先fetch到库，再将远程的rebase到本地就嫩避免pull产生的分支合并 *感觉没啥软用*
- 如果产生冲突，手动解决后执行`git add .`接着`git rebase --continue`完成rebase

## 冲突解决
使用beyond compare工具（外部工具），自动定位冲突快速解决冲突

安装，并在git配置**只在当期repo有效**
```
git config --local merge.tool bc3
git config --local mergetool.path  '安装路径'
git config --local mergetool.keepBackup false //取消备份，可选
```

出现冲突时 `git mergetool` ，解决之

## 多人合作
1 在dev进行创建分支并切换`git checkout -b branchname`，完成开发

2 开发完成后，进行review，使用Pull/Merge Requested

- 首先，管理员进行配置，进入Github页面，找到Setting的Branches界面，选好dev，按条件选其他，提交即可。
- 接着，开发人员在Code界面提交Pull Requested，选好将哪个分支并入dev并填好其他信息。
- 最后，管理员在网站就能看到，在add-review检查，完成后返回并允许合并。（可以用bash命令形式拉到本地进行，详细看网站那提供的命令）

3 完成后进行测试，最后再发布到main，一次开发完成
+ 首次测试`git checkout -b release`，在release分支进行测试
+ 用Pull Requested合并到main或者Merge到main

## Fork，为其他开源软件做贡献
1 在其他团队项目中进行fork，就在自己仓库得到这份克隆

2 在自己仓库对改代码改进，完成后提交到自己的仓库（二次开发）

3 给原作者提交改进申请，在自己库主页提交一个Pull Requested，等待原作者回复

## 其他关于Git的知识
### 配置文件
git找配置先找local再找global最后找system

+ 项目配置文件 对它的配置只作用于当前的项目,/.git/config `git config --local 配置内容`
+ 全局配置文件 对它的配置会作用于所有项目,~/.git/config  `git config --global 配置内容`
+ 系统配置文件（要root权限）,/etc/.gitconfig            `git config --system 配置内容`
### 免密登录
- git自动管理，不用管，个人用最方便
- URL，修改地址https://用户名:密码@github.com/其他.../项目.git （主要过去每次push都要输用户和密码，实现与现在push相同功能）
- SSH，生成公钥（企业用的最多）

   1 在PC生成公钥和私钥`ssh-keygen `，在~/.ssh 目录下id_rsa.pub公钥、id_rsa私钥

   2 在Github页面，在个人的setting里面的SSH添加那段码

   3在项目中配置ssh地址,`git remote add origin ssh的地址git@...`
### gitignore
让git忽略一些文件

创建一个叫.gitignore文件，在里面输入文件名，该文件就会被忽略，文件名可以bash的语法（父目录作用于子目录，子目录不作用于父：file/*.h时，./的\*.h不会忽略）
### 放置issues、wiki
用于团队提问题、接问题

### LFS 大文件提交和管理
管理大文件可以使用LFS，被管理文件记录在.git/.gitattributes

`git lfs track [文件名|文件夹]`，使LFS追踪这个文件或文件夹

`git lfs track`展示以追踪的文件，或`git lfs ls-file`显示特定文件是否被追踪
