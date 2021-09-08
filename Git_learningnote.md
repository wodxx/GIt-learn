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

场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令git reset --staged <file>，就回到了场景1，第二步按场景1操作。

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

### 6.从远程仓库克隆 ###

```C
$ git clone 2286721671@qq.com:wodxx/learning_git.git
# GitHub 支持多种协议,上面是 ssh 协议,还有 https 协议
```
