# Git

## Commit message

### type

1. feat：新功能（feature）
2. fix：修补bug
3. docs：文档（documentation）
4. style： 格式（不影响代码运行的变动）
5. refactor：重构（即不是新增功能，也不是修改bug的代码变动）
6. test：增加测试
7. chore：构建过程或辅助工具的变动

### scope

用于说明 commit 影响的范围，比如数据层、控制层、视图层等等，视项目不同而不同

### subject

Subject是 commit 目的的简短描述，不超过50个字符

## 工作区、暂存区、本地仓库、远程仓库

![Git存储分布](/Git%E5%AD%98%E5%82%A8%E5%88%86%E5%B8%83.png)

***版本库包含stage和master***

```
git diff 比较的是工作区和暂存区的差别
git diff --cached 比较的是暂存区和版本库的差别
git diff HEAD 可以查看工作区和版本库的差别
```

1. 工作区：本地磁盘修改文件（如IDEA中的CRUD）
2. 暂存区：stage，是一个包含文件索引的目录树，不保存文件内容（**文件内容保存在.git/objects，一般是隐藏文件夹**），文件索引建立了文件和对象库中对象实体  之间的对应，如果当前仓库有文件更新，并使用了git add命令，则这些更新就会出现在stage
3. 本地仓库：stage中的文件经过git commit命令，提交到本地仓库（这也是为什么Git在没有网络的情况下也能提交代码）
4. 远程仓库：git push命令，本地master或其他分支被推送到远程仓库（远程分支master如果不存在，则会被创建）



## Git pull和Git fetch

**git pull**：从远程获取最新版本并merger到本地（和本地分支merger，如果本地不存在，则新创建分支）；

```
git pull = git fetch + git merger
```

**git fetch**：从远程获取最新分支到本地，不会自动merger，具体应用为：

```
## 从远程master获取分支到本地分支
git fetch origin master
## 比较本地分支和远程的差别
git log -p master..origin/master
## 合并到本地分支
git merger origin/master
```

上述命令可以表示为：

```
git fetch origin master:tmp
git diff tmp 
git merge tmp
```

*推荐使用git fetch，如果是新的分支，则两者效果相同*

## 切分支

1. 直接在GitHub或者GitLab上新建分支，本地pull到本地分支，最后checkout到工作区
2. 本地new branches

```
git branch feat1
git checkout feat1
git push -u origin feat1
```

## GitHub删除文件夹

想要在GitHub上删除，但又需要保留在本地的文件夹的删除方法，以.idea为例：

```
git rm -r —cached .idea
git commit -m 'delete .dea dir'
# push之前最好通过pull或fetch命令更新本地仓库
git pull URL
git push -u origin master
```

## Git上传项目到GitHub

```
# 进入到当前项目目录
git init
git add . 	#（.代表全部文件上传）
git commit -m "first commit"
# GitHub创建仓库
git remote add origin 仓库URL 	#为远程Git更名为origin
git push -u origin master	#推送此次修改

参数“-u”，Git不但会把本地的master分支内容推送的远程新的master分支，还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令
```

## Git中文乱码

```
git config --global gui.encoding utf-8
```

## Git获取指定分支

***注：有时当前用户没有权限创建文件夹，使用root权限sudo***

```
#获取master
git clone master地址
git branch -r 或 git branch -a查看所有分支
#切到指定分支
git checkout -b 本地分支名XXX origin/远程分支名XX
#切换到master
git checkout master
```

## Git远程代码回退到某一次提交

```
#查看提交日志
git log
#撤销到某个版本之前，之前的修改回到stage，soft会保留修改记录，hard不会保留 
git reset --soft commit-id
#为了安全起见，暂存
git stash
#修改文件，puggsh到remote，-f强制覆盖
git push --f
#释放暂存
git stash pop
```

## Git不同项目设置不同的用户名和邮箱

```

```





# SVN

## 基本命令

```
svn co(checkout) url	#从地址检出代码，以后就可以直接提交到此地址
svn up(update) -r m path
```

例：svn update如果后面没有目录，默认将当前目录以及子目录下的所有文件都更新到最新版本。

​	svn update -r 200 test.php(将版本库中的文件test.php还原到版本200)

​	svn update test.php(更新，于版本库同步。如果在提交的时候提示过期的话，是因为冲突，需要先update，修改文件，然后清除svn resolved，最后再提交commit)

```
svn info	#查看当前分支
svn di(diff) path	#将修改的文件与基础版本比较
```

例：svn diff test.php

​	 svn diff -r m:n path(对版本m和版本n比较差异)

```
svn log		#用来展示svn 的版本作者、日期、路径等等
svn st(status)	#查看编辑的文件
```

​	**?：不在svn的控制中；M：内容被修改；C：发生冲突；A：预定加入到版本库；K：被锁定**

```
svn ci(commit) -m 'msg'		#提交到svn
svn revert		#版本回退
svn resolve --accepting working test.php
```

注：svn resolve 是解决冲突的命令，解决的方法由--accepting 选项决定

​	base  恢复到冲突前的一个版本 

​	mine-full  恢复到以我的修改为主的版本 

​	their-full  恢复到库中最新版本 

​	working  手动解决

```
svn delete test.php		#从版本库中删除
svn add test.php	#添加到版本库
```

## 文件的状态

```
A：add，新增
C：conflict，冲突
D：delete，删除
M：modify，本地已经修改
G：modify and merGed，本地文件修改并且和服务器的进行合并
U：update，从服务器更新
R：replace，从服务器替换
I：ignored，忽略
```





# Code Review

### 安装Arcanist和依赖库libphutil

```
# 推荐安装到/Applications/arcanist/下
cd /Applications
mkdir arcanist
cd arcanist
 
# 开始下载clone这两个文件夹
# 注意：要放在同一个文件夹里
git clone git://github.com/facebook/arcanist.git
git clone git://github.com/facebook/libphutil.git
```

### 设置环境变量

```
# 先cd到自己的用户目录下
cd ~
 
# 然后编辑.bash_profile文件
# 如果没有这个文件，则创建一个.bash_profile文件
vi .bash_profile
  
# 增加如下一行
export PATH="$PATH:/Applications/arcanist/arcanist/bin"  
 
# 最后让文件生效
source .bash_profile
```

### 测试结果

```
arc help
```

### 设置arc编辑器

```
# 默认vi做编辑器
arc set-config editor "vim"
```

### 设置配置文件

***这一步必须在SVN或Git目录下进行，即当前project目录下（如果是从Git或SVN上clone下来的话）***

1. **进入你想diff的svn项目或者git项目，ssi工程或者avatar什么的都行，只要进入根目录下**
2. 在项目的根目录下新增**.arcconfig**配置文件，mac下vi .arcconfig就能新增文件，文件内容如下
3. 文件名必须是**.arcconfig**

```
# 文件名必须是.arcconfig
# 内容为以下两行即可
{
  "project_id" : "avatar",
  "conduit_uri" : "http://review.haodf.net/"
}
```

### 配置权限认证

1. **接着在刚刚的那个有.arcconfig的文件夹里进行如下操作**
2. **必须要有这一步，不然你是没有权限往review.haodf.net提交文件的（换句话说，就是一种变相的登录操作）**

```
# 否则你会没有权限向服务器提交diff代码
arc install-certificate
```

从浏览器中粘贴刚生成的token，进行验证

### diff操作

```
arc diff
## arc diff = git add + git commit
```

填写message，reviewers等信息，保存退出，代码已经到了本地仓库

### 常用命令

```
$ arc help                      # 获得arc中包装的可用指令/工具
$ arc diff                      # 提交代码去审核
$ arc diff --update Dxxx        # 审核未通过，修改后，再次提交审核，D为上次的版本号，如D19450
$ arc diff --create             # 创建一个新的提交审核
$ arc land                      # 审核通过后提交，已包括git push的动作，所以无需再push了
$ arc amend                     # 审核Git更新提交后的信息
$ arc list                      # 显示未提交修改的代码信息
$ arc lint                      # 检查代码的语法
$ arc get-config                # 查看已设置过的配置
$ arc set-config <key> <value>  # 修改配置，使用--local参数为全局配置
```









# 