#### git 作用域
[TOC]

**如果缺省等同于 local**
``` git
$ git config --local
local 只对某个仓库有效
$ git config --global
global 对当前用户所有仓库有效
$ git config --system
system 对系统所有登陆的用户有效
```

### 初始化


#### git 作用域
[TOC]

**如果缺省等同于 local**
``` git
$ git config --local
local 只对某个仓库有效
$ git config --global
global 对当前用户所有仓库有效
$ git config --system
system 对系统所有登陆的用户有效
```

***

如若需要显示 config 的配置，加上 --list
``` git
$ git config --list --loacl
$ git config --list --global
$ git config --list --system
```

***
**git 登录设置**
```git 
$ git config --global user.name ‘'laborbird'
$ git config --global user.email '18226568070@139.com'
```
![e7868d16fb5112bd06bd631202e530e6.png](en-resource://database/482:1)
![20268ec22ed6381376aad15c0cf63690.png](en-resource://database/483:1)


***

##### 在仓库中添加文件 

1、再工作目录中编辑好代码，通过命令把代码添加到 git 暂存区
```git
git status 查看当前状态，看是否有需要添加的文件

git add [文件名] 
git add  -u  %把需要添加的全部添加
```

2、把暂存区的文件提交到服务器中
```git
git status 查看状态，有没有把文件提交到暂存区成功
git commit -m'本次提交的记录备注'
```
3、查看版本历史
```git 
在提交好文件后，通过命令能够查看版本状态
$ git log
```
* **git 文件重命名的简单方法**
如果上传需要重新命名时，有两种方法，一种事在工作目录中修改文件名，然后上传到暂存区，再进行后续的操作，另一种是直接通过命令把文件名修改成需要修改的名字
```git
 $ git mv readme.txt readme.md

```
![15a7bc8e69588c08753bb93823306978.png](en-resource://database/484:1)




* **git 查看版本信息**
```git
 $ git log 

# --oneline  简洁化查看
# -nx   x 为任意参数，表示为最近的 x 条
# --all 产看所有的，包括其它分支的版本信息
# --graph 对不同的分支结构使用不同标记
 
```
* **git 分支**

本地分支信息查看
```git
$ git  checkout -b xxx
Switch to a new branch ''xxx''
# 另外两种写的形式
$ git branch xxx
$ git chechout xxx
```
切换到原来的（master）分支
```git
$ git checkout master
```


##### commit tree blob 三个对象之间的关系
在不连接数据库时 .git 本身也有一个本地库，中间的目录以 HEAD 查看到分支
config 文件能够查看到对应的配置，在文件中修改和通过命令修改的效果是一样的。
![eed08c698b6e3299d9160fd61a658abb.png](en-resource://database/485:1)



##### 分离头指针的注意事项

在使用分离头指针的时候如果经过了 commit 但是此时的工作环境是这个头指针所在的环境，相当于计算机的存储机制在内存中，在头指针中的 commit 没有依赖，这时就会有危险发生，在 git checkout 到别的分支时头指针中的 commit 就没有被保存。

![image.png](data:image/png;base64,

这时如果需要保存可以根据 git 的命令提示使用 
git branch <new-branch-name> xxxx(对应的 commit 提交号)

理解 git 中的 Head 和 branch
不同版本中的对比
```shell
$ git diff xxxx xxxxx
$ git dif head head^^
$ git diff head head~2
```

删除分支：
```shell
$ git branch -d xxx
$ git branch -D xxx
```


##### 对commit 的 messasge 做变更

**对最新的 commit 做变更**
首先查看当前仓库的分支，然后在查看最新的 commit 

```shell
$ git branch -av 查看当前分支

$ git log -1 查看最新的一次 commit

$ git commit --amend 修改 commit 的message

```
![a6ab804800c52863e491a8c3ba1546f7.png](en-resource://database/486:1)

 **对老旧的 commit 的 message 做变更**
 同样时产看需要做变更的是哪一个 commit 做变更，找到这个对应的 commit 的前一个父的 commit  id 号
 
```shell
$ git rebase -i xxxxxxx ( commit 的 id 号)
```
 修改后，本省的 commit id 号会有改变
 
```shell
$ git log -n3 --graph  可以使用这个命令查看当前的 commit 状态

```
 ![da9c797c0a44a9d841ee9b44d2acc074.png](en-resource://database/487:1)
 
 
 ##### 
 
 
 ##### 命令统计
 
| 常用命令 | 命令参数 | 命令含义 |
| --- | --- | --- | 
| git branch | -a -v | 查看当前仓库的分支|
| git commit | -amend | 修改 commit 的 message |
| git log | -n n 为数字  | 查看当前日志的最新的 n 条|
| git log | -n3 --graph | 查看当前 3 条日志记录并显示节点关系 |
| git rebase | -i xxxx(commit id 号) | 修改 commit 的 message |



