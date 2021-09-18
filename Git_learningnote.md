# Git使用笔记 #

## 一. Git和SVN的区别 ##

|   |类型|描述|
| -| - | - |
Git|分布式版本控制系统|本地有镜像,无网络时也可以提交到本地镜像,等到有网络时再push到服务器
SVN|集中式版本控制系统|无网络不可以提交, 和 Git 的主要区别是历史版本维护的位置

## 二. 安装Git ##

本文的git安装在ubuntu系统下

## 三. 创建版本库 ##

**版本库又名仓库，英文名repository，可以简单理解成一个目录，这个目录里面的所有文件都可以被Git管理起来，每个文件的修改、删除，Git都能跟踪，以便任何时刻都可以追踪历史，或者在将来某个时刻可以“还原"。**

### 1.相关命令 ###

```C

(所有命令都在 Git Bash 中运行)
$ git                           查看 git 的相关命令 (git --help)
$ git --version                 查看 git 的版本
$ git config                    查看 git config 的相关命令
$ git pull origin develop       从远程(origin) 的 develop 分支拉取代码
```

### 2.初始化本地仓库 ###

```c

$ mkdir learn_git
$ cd learn_git
$ pwd

# cd: change directory 改变目录
# mkdir  创建目录
# pwd    用于显示当前目录
```

```c

# 不想要 git 管理跟踪的文件,可以在仓库根目录添加 .gitignore 文件,在里面写对应的规则
$ git init              把当前目录初始化为 git 仓库
$ ls -ah                查看当前目录下的文件,包含隐藏文件 (不带 -ah 看不了隐藏文件)
```

### 3.添加文件到仓库 ###

随便写一个文件放到 ***git_learing*** 目录下，因为这是一个Git仓库，放到其他地方Git找不到这个文件.
这里我们写了一个 ***readme.txt*** 文件，把一个文件放到Git仓库需要两步：

第一步：用命令 git add 告诉Git，把文件添到仓库  

```C

git add readme.txt
```

第二步，用命令git commit告诉Git，把文件提交到仓库：

```C

$ git commit -m "wrote a readme file"
[master (root-commit) eaadf4e] wrote a readme file
1 file changed, 2 insertions(+)
create mode 100644 readme.txt
```

**注意**:

1. -m 后面输入的是本次提交的说明，可以输入任意内容，当然最好是有意义的,这样你就能从历史记录里方便地找到改动记录。

2. git commit命令执行成功后会告诉你， ***1 file changed***  1个文件被改动（我们新添加的readme.txt文件）； ***2 insertions*** 插入了两行内容（readme.txt有两行内容）。

3. 为什么Git添加文件需要add，commit一共两步呢？因为commit可以一次提交很多文件，所以你可以多次add不同的文件，比如：

```C
git add file1.txt
git add file2.txt file3.txt
git commit -m "add 3 files."
```

### 4.查看仓库当前状态(项目是否有修改，添加，未追踪文件等) ###

```C
git status 
```

### 5.查看修改内容，查看文件不同 ###

```C
$ git diff 
$ git diff <file>                
$ git diff --cached
$ git diff HEAD -- <file>
# git diff 查看工作区(work dict)和暂存区(stage)的区别
# git diff --cached 查看暂存区(stage)和分支(master)的区别
# git diff HEAD -- <file> 查看工作区和版本库里面最新版本的区别
如: git diff readme.txt  表示查看 readme.txt 修改了什么,有什么不同
```

### 6.查看提交日志 ###

每修改一次算一个版本，如何查看版本？用日志命令！

```C
$ git log
$ git log --oneline     #美化输出信息,每个记录显示为一行,显示 commit_id 前几位数
$ git log --pretty=oneline     #美化输出信息,每个记录显示为一行,显示完整的 commit_id
$ git log --graph --pretty=format:'%h -%d %s (%cr)' --abbrev-commit --
$ git log --graph --pretty=oneline --abbrev-commit

# 显示从最近到最远的提交日志
# 日志输出一大串类似 3628164...882e1e0 的是commit_id (版本号),和 SVN 不一样，Git 的commit_id 不是 1，2，3…… 递增的数字，而是一个 SHA1 计算出来的一个非常大的数字，用十六进制表示, 因为 Git 是分布式的版本控制系统，当多人在同一个版本库里工作，如果大家都用 1，2，3……作为版本号，那肯定就冲突了
# 最后一个会打印出提交的时间等, (HEAD -> master)指向的是当前的版本
# 退出查看 log 日志,输入字母 q (英文状态)
```

### 7.版本回退 ###

为什么要版本回退？
答：当文件修改到一定程度出现问题的时候，比如把文件改乱了或者误删了文件，就可以选择一个最近保存过的版本回退。

```C

$ git reset --hard HEAD^
$ git reset --hard <commit_id>

# HEAD    表示当前版本，也就是最新的提交
# HEAD^   上一个版本
# HEAD^^  上上一个版本
# HEAD~100   往上100个版本

# 回退到 commit_id 对应的那个版本,commit_id 为版本号,只需要前几位就行
```

此时如果使用 git log 查询日志时，会发现没有最近一次的日志显示，如果想前进到回退前的版本，找到以前未来版本的提交日志显示的版本号，用以下命令回去：

```C
git reset --hard 1094a （1094a是未来版本的版本号前几位）

```

查看某一文件的内容命令

```C
cat <file_name>
```

#### 小结 ####

1. HEAD指向的版本就是当前版本，因此，Git允许我们在版本的历史之间穿梭，使用命令git reset --hard commit_id。

2. 穿梭前，用git log可以查看提交历史，以便确定要回退到哪个版本。

3. 要重返未来，用git reflog查看命令历史，以便确定要回到未来的哪个版本。

### 8.工作区和暂存区 ###

**工作区**：在电脑里能看到的目录，比如learn_git文件夹就是一个工作区;
**版本库**：工作区有一个隐藏目录 .git ,这不算工作区，而是git的版本库。
Git的版本库里存了很多东西，其中最重要的就是称为stage（或者叫index）的暂存区，还有Git为我们自动创建的第一个分支master，以及指向master的一个指针叫HEAD。

前面把文件往Git版本库里添加的时候，是分两步执行的：

第一步是用git add把文件添加进去，实际上就是把文件修改添加到暂存区；

第二步是用git commit提交更改，实际上就是把暂存区的所有内容提交到当前分支。

#### 小 结 ####

1. git add命令实际上就是把要提交的所有修改放到暂存区（Stage），然后，执行git commit就可以一次性把暂存区的所有修改提交到分支；
2. 每次修改，如果不用git add到暂存区，那就不会加入到commit中。

### 9.撤销和修改 ###

***- 丢弃工作区 (Working Directory) 的修改***

```C

git restore <file>  (建议使用) (如: git restore readme.txt)
git checkout -- <file>
# 命令中 -- 很重要，没有就变成 “切换到另一个分支” 的命令

```

***- 丢弃暂存区 (stage/index) 的修改***

```C

# 第一步: 把暂存区的修改撤销掉(unstage)，重新放回工作区
$ git restore --staged <file>

# 第二步: 撤销工作区的修改
$ git restore <file>

```

#### 小  结 ####

场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令git checkout -- file。

场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令git reset --staged < file >，就回到了场景1，第二步按场景1操作。

场景3：已经提交了不合适的修改到版本库时，想要撤销本次提交，参考版本回退一节，不过前提是没有推送到远程库。

### 10.删除文件 ###

```C

$ git rm <file>

# git rm <file> 相当于执行
- rm <file>
- git add <file>
# 命令git rm用于删除一个文件。如果一个文件已经被提交到版本库，那么你永远不用担心误删，但是要小心，你只能恢复文件到最新版本，你会丢失最近一次提交后你修改的内容。

```

## 四. 相关名词理解 ##

**1. 工作区 (Working Directory): 自己电脑里能看到的目录
2. 版本库 (Repository): 工作区有一个隐藏目录 .git，这个不算工作区，而是 Git 的版本库**

>Git 的版本库里存了很多东西，其中最重要的就是称为 stage（或者叫index）的暂存区，还有 Git 为我们自动创建的第一个分支 master，以及指向 master 的一个指针叫 HEAD

## 五. 远程库创建 ##

### 1. 创建 SSH Key ###

```C

ssh-keygen -t rsa -C "youremail@example.com"
# 邮件地址换成你自己的邮件地址，然后一路回车，使用默认值即可，由于这个Key也不是用于军事目的，所以也无需设置密码。
```

>在用户主目录下(ubuntu下时home目录下.ssh作为隐藏文件），看看有没有.ssh目录，如果有，再看看这个目录下有没有id_rsa和id_rsa.pub这两个文件，如果已经有了，可直接跳到下一步。如果没有，打开Shell（Windows下打开Git Bash），创建SSH Key。
>如果一切顺利的话，可以在用户主目录里找到.ssh目录，里面有 id_rsa 和 id_rsa.pub 两个文件，这两个就是 SSH Key 的秘钥对，id_rsa 是私钥，不能泄露出去，id_rsa.pub 是公钥，可以放心地告诉任何人。

### 2.在GitHub中添加new ssh ###

>登录 GitHub ,在 Settings 中找到 SSH 设置项中添加新的 SSH Key,设置任意 title,在 Key 文本框里粘贴 id_rsa.pub 文件的内容。

### 3.关联远程仓库（先有本地仓库） ###

>登陆GitHub，然后，在右上角找到“Create a new repo”按钮，创建一个新的仓库
>目前，在GitHub上的这个新仓库还是空的，GitHub告诉我们，可以从这个仓库克隆出新的仓库，也可以把一个已有的本地仓库与之关联，然后，把本地仓库的内容推送到GitHub仓库

```C
git remote add origin git@github.com:wodxx/learning_git.git
# 后面的地址换成自己的 GitHub 仓库地址
```

### 4.关联远程仓库分支 ###

```C
git branch -M main
```

### 5.推送到远程仓库 ###

```C
$ git push -u origin master   第一次推送
$ git push origin master      推送本地 master 分支到远程库
$ git push origin dev         推送本地 dev 分支到远程库
# 除了第一次推送,不需要添加 -u 参数
```

>把本地库的内容推送到远程，用git push命令，实际上是把当前分支master推送到远程。
>由于远程库是空的，我们第一次推送master分支时，加上了-u参数，Git不但会把本地的master分支内容推送的远程新的master分支，还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令。

```C
相关命令
$ git remote       查看远程库信息
$ git remote -v    查看远程库详细信息
$ git remote rm origin  删除已关联的远程库 origin

# 一个本地库关联多个远程库,例如同时关联 GitHub 和 Gitee:
# 1. 先关联GitHub的远程库：(注意:远程库的名称叫 github，不叫 origin)
$ git remote add github git@github.com:renyun/learngit1.git
# 2. 再关联Gitee的远程库：(注意:远程库的名称叫 gitee，不叫 origin)
$ git remote add gitee git@gitee.com:renyun/learngit1.git
# 3. 推送到远程库
$ git push github master
$ git push gitee master
```

**本地仓库和远程仓库关联可能出现的问题:**
1.如果在github上建立新仓库时创建了一个readme文件，可能出现本地仓库无法关联远程仓库的主分支的问题；
2.此时就要把远程仓库的文件pull到本地仓库再重新关联，用如下命令：
>git pull --rebase origin main

### 6.从远程仓库克隆 ###

```C
$ git clone 2286721671@qq.com:wodxx/learning_git.git
# GitHub 支持多种协议,上面是 ssh 协议,还有 https 协议
```

## 六. 分支管理 ##

***分支的功能***
>创建一个属于自己的分支，把工作区的内容提交到自己的分支里面，防止进度丢失；并且自己的分支别人是看不到的。

### 1.创建与合并分支 ###

```C
git checkout -b dev   #创建并切换到新的dev分支
git checkout main     #直接切换到已有的main分支
git branch            #查看已有的分支
git merge <name>      #合并某分支到当前分支
git branch -d<name>   #删除分支
```

### 2.解决冲突 ###

***发生冲突的原因***
>一个文件在新的分支上提交的内容（或修改的内容）与在主分支上提交的内容不一致，这时将新分支合并到主分支时发生冲突

***解决冲突***
>将两个分支上修改的内容改一致，然后合并。最后删除新分支

### 3.分支管理 ###

>通常，合并分支时，如果可能，Git(默认）会用Fast forward模式，但这种模式下，删除分支后，会丢掉分支信息。
如果要强制禁用Fast forward模式，Git就会在merge时生成一个新的commit，这样，从分支历史上就可以看出分支信息。

```C

# 使用递归合并
# --no-ff参数，表示禁用Fast forward
git merge <new branch> --no-ff -m "merge with no -ff"

$ git log --graph  查看分支合并图
$ git log --graph --pretty=oneline --abbrev-commit
```

合并后，可以使用git log 查看历史分支！

***使用新分支的原因:***

在实际开发中，我们应该按照几个基本原则进行分支管理：
1.首先，master分支应该是非常稳定的，也就是仅用来发布新版本，平时不能在上面干活；
2.**干活都在dev分支上**，也就是说，dev分支是不稳定的，到某个时候，比如1.0版本发布时，再把dev分支合并到master上，在master分支发布1.0版本；
3.每个人都在dev分支上干活，每个人都有自己的分支，时不时地往dev分支上合并就可以了;
4.合并分支时，加上--no-ff参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而fast forward合并就看不出来曾经做过合并。

### 4. bug分支 ###

```C
  git stash  保存当前工作区和暂存区的修改状态,切换到其他分支修复 bug 等工作然后在回来继续工作
$ git stash list  查看保存现场的列表
$ git stash pop   恢复的同时把 stash 内容也删除
$ git stash apply  恢复现场，stash内容并不删除
$ git stash drop   删除 stash 内容
$ git stash apply stash@{0}  多次stash，恢复的时候，先用git stash list查看，然后恢复指定的stash

通常在 dev 分支开发时,需要有紧急 bug 需要马上处理,保存现在修改的文件等,先修复 bug 后再回来继续工作的情况

$ git cherry-pick <commit> 复制一个特定的提交到当前分支(当前分支的内容需要先 commit,然后冲突的文件需要解决冲突,然后 commit)

$ git rebase  把本地未push的分叉提交历史整理成直线(使得我们在查看历史提交的变化时更容易，因为分叉的提交需要三方对比)
```

### 5. feature分支 ###

>开发一个新feature，最好新建一个分支；
如果要丢弃一个没有被合并过的分支，可以通过git branch -D < name > 强行删除。

### 6. 多人协作 ###

>当你从远程仓库克隆时，实际上Git自动把本地的master分支和远程的master分支对应起来了，并且，远程仓库的默认名称是origin
> 1.查看远程库信息，使用**git remote -v**；
2.本地新建的分支如果不推送到远程，对其他人就是不可见的；
3.从本地推送分支，使用git push origin branch-name，如果推送失败，先用git pull抓取远程的新提交；
4.在本地创建和远程分支对应的分支，使用git checkout -b branch-name origin/branch-name，本地和远程分支的名称最好一致；
5.建立本地分支和远程分支的关联，使用git branch --set-upstream branch-name origin/branch-name；
6.从远程抓取分支，使用git pull，如果有冲突，要先处理冲突。

## 七 .标签管理 ##

**原因** 为什么要使用标签管理？
答：给版本库打一个标签，确定打标签时的某个版本！

### 1. 创建标签 ###

```C
打标签的过程：
git branch                   #查看当前分支
git checkout < branch >      #切换到需要打标签的分支上
git tag < name >             #name是标签的名字，如 v1.0
git tag                      #查看所有标签
```

**问题1** 如何给以前的commit打标签？

```C

git log --pretty=oneline --abbrev-commit   #找以前的commit id
git tag v0.9 < commit id >                 #给以前提交的版本打上标签
git tag                                    #再次查看标签
```

**问题2** 如何查看标签信息？

```C
git show < tag name >     #因为标签不是按时间顺序列出，而是按字母顺序排列的。因此可以用该命令来查看具体的标签信息。
git tag -a v0.1 -m “version 0.1 released” 1094adb  #[创建标签的另一种方法]带说明的标签，用-a指定标签名，-m指定说明文字。
```

***注意***
>1.默认的标签时打在最新提交的commit上的
2.标签总是和某个commit挂钩。如果这个commit既出现在master分支，又出现在dev分支，那么在这两个分支上都可以看到这个标签。

### 2. 操作标签 ###

```C
操作1：删除标签     
git tag -d v0.1

操作2：标签推远程   
git push origin < tag name > 

或者一次性推送全部尚未推送到远程的本地标签  
git push origin --tags

操作3:标签已推送的情况下删除标签
git tag -d v0.9         #先从本地删除
git push origin : refs/tags/v0.9    #再从远程删除
```

## 八. 使用github ##

### 1.使用github ###

***做什么?***
参与一个开源项目!比如人气极高的bootstrap项目，这是一个非常强大的CSS框架，访问它的项目主页,点“Fork”就在自己的账号下克隆了一个bootstrap仓库，然后，从自己的账号下clone：
>git clone git@github.com:michaelliao/bootstrap.git
在github上，可以任意Fork开源仓库;
自己拥有Fork后的仓库的读写权；
可以推送pull request 给官方仓库来贡献代码。

### 2.使用Gitee ###

### 3.建立gitignore文件 ###

>在本地仓库建立一个gitignore文件，旨在把不想推送的文件夹或者文件在一次推送的过程中忽略掉。
touch .gitignore           #在本地仓库下建立gitignore文件
然后打开.gitignore文件，把不想推送的文件夹或者某类型的文件后缀写上去。

注意:
新建的.gitignore文件也要先推送到远程仓库，才能对里面忽略的内容在下次推送的时候产生效果！

### 4. 配置别名 ###

***用途***
将一些命令的英文以缩写的形式生成指令！

```C
git congfig --global alias.st status
git config --global alias.co checkout
git config --global alias.ci commit
git config --global alias.br branch
告诉git，以后用st表示status,用co代替checkout，用ci代替commit...
```

> --global参数是全局参数，也就是这些命令在这台电脑的所有Git仓库下都有用。

### 5.搭建服务器 ###

>自己搭建一台Git服务器作为私有仓库使用。

### 九. 使用SourceTree ###

使用SourceTree可以以图形界面操作Git，省去了敲命令的过程，对于常用的提交、分支、推送等操作来说非常方便。

SourceTree使用Git命令执行操作，出错时，仍然需要阅读Git命令返回的错误信息。

