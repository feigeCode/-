# 1、安装git

## 在Linux上安装Git

首先，你可以试着输入`git`，看看系统有没有安装Git：

```bash
$ git
The program 'git' is currently not installed. You can install it by typing:\n
sudo apt-get install git
```

像上面的命令，有很多Linux会友好地告诉你Git没有安装，还会告诉你如何安装Git。

如果你碰巧用Debian或Ubuntu Linux，通过一条`sudo apt-get install git`就可以直接完成Git的安装，非常简单。

老一点的Debian或Ubuntu Linux，要把命令改为`sudo apt-get install git-core`，因为以前有个软件也叫GIT（GNU Interactive Tools），结果Git就只能叫`git-core`了。由于Git名气实在太大，后来就把GNU Interactive Tools改成`gnuit`，`git-core`正式改为`git`。

如果是其他Linux版本，可以直接通过源码安装。先从Git官网下载源码，然后解压，依次输入：`./config`，`make`，`sudo make install`这几个命令安装就好了。

## 在Mac OS X上安装Git

如果你正在使用Mac做开发，有两种安装Git的方法。

一是安装homebrew，然后通过homebrew安装Git，具体方法请参考homebrew的文档：http://brew.sh/。

第二种方法更简单，也是推荐的方法，就是直接从AppStore安装Xcode，Xcode集成了Git，不过默认没有安装，你需要运行Xcode，选择菜单“Xcode”->“Preferences”，在弹出窗口中找到“Downloads”，选择“Command Line Tools”，点“Install”就可以完成安装了。

![install-git-by-xcode](https://gitee.com/feigeCode/picture/raw/master/img/0.jpg)

Xcode是Apple官方IDE，功能非常强大，是开发Mac和iOS App的必选装备，而且是免费的！

## 在Windows上安装Git

在Windows上使用Git，可以从Git官网直接[下载安装程序](https://git-scm.com/downloads)，然后按默认选项安装即可。

安装完成后，在开始菜单里找到“Git”->“Git Bash”，蹦出一个类似命令行窗口的东西，就说明Git安装成功

安装完成后，还需要最后一步设置，在命令行输入：

```bash
$ git config --global user.name "Your Name"
$ git config --global user.email "email@example.com"
```

因为Git是分布式版本控制系统，所以，每个机器都必须自报家门：你的名字和Email地址

注意`git config`命令的`--global`参数，用了这个参数，表示你这台机器上所有的Git仓库都会使用这个配置，当然也可以对某个仓库指定不同的用户名和Email地址。

# 2、创建版本库

```bash
$ mkdir learngit
$ cd learngit
$ pwd
/Users/michael/learngit

$ git init
Initialized empty Git repository in /Users/michael/learnGit笔记/.Git笔记/
```

![image-20200212133917146](https://gitee.com/feigeCode/picture/raw/master/img/image-20200212133917146-1583835639479.png)

Git就创建一个空的仓库（empty Git repository），目录下多了一个`.git`的目录，这个目录是Git来跟踪管理版本库的，不要手动修改这个目录里面的文件，不然改乱了，就把Git仓库给破坏了。

如果你没有看到`.git`目录，那是因为这个目录默认是隐藏的，windows下可以进入该目录，然后点击查看，勾选隐藏目录那个框

![image-20200212134738875](https://gitee.com/feigeCode/picture/raw/master/img/image-20200212134738875.png)

mac用`ls -ah`命令就可以看见。

# 3、把文件添加到版本库

第一步，用命令`git add`告诉Git，把文件添加到仓库：

```bash
$ git add feige.txt
```

执行上面的命令，没有任何显示，这就对了，Unix的哲学是“没有消息就是好消息”，说明添加成功。

第二步，用命令`git commit`告诉Git，把文件提交到仓库：

```bash
$ git commit -m 'first use git'
[master (root-commit) 72c881b] first use git
 1 file changed, 1 insertion(+)
 create mode 100644 feige.txt
```

![image-20200212135617934](https://gitee.com/feigeCode/picture/raw/master/img/image-20200212135617934.png)

简单解释一下`git commit`命令，`-m`后面输入的是本次提交的说明，可以输入任意内容，当然最好是有意义的，这样你就能从历史记录里方便地找到改动记录。

`git commit`命令执行成功后会告诉你，`1 file changed`：1个文件被改动（我们新添加的readme.txt文件）；`2 insertions`：插入了两行内容（feige.txt有两行内容）。

注意：可以先执行多个add，再commit

**小结**：

初始化一个Git仓库，使用`git init`命令。

添加文件到Git仓库，分两步：

1. 使用命令`git add `，注意，可反复多次使用，添加多个文件；
2. 使用命令`git commit -m `，完成。

# 5、使用git statu和git diff命令

~~~bash
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   feige.txt

no changes added to commit (use "git add" and/or "git commit -a")

~~~



`git status`命令可以让我们时刻掌握仓库当前的状态，上面的命令输出告诉我们，feige.txt改过了，但还没有准备提交的修改。

虽然Git告诉我们feige.txt了，但如果能看看具体修改了什么内容，自然是很好的。比如你休假两周从国外回来，第一天上班时，已经记不清上次怎么修改的feige.txt`，所以，需要用`git diff`这个命令看看：

~~~bash
$ git diff
diff --git a/feige.txt b/feige.txt
index 37dd99a..970cf48 100644
--- a/feige.txt
+++ b/feige.txt
@@ -1 +1,2 @@
-第一次使用git
\ No newline at end of file
+第一次使用git
+使用 git status
\ No newline at end of file

~~~

`git diff`顾名思义就是查看difference，显示的格式正是Unix通用的diff格式

再次提交

~~~bash
$ git add feige.txt

love。com@LAPTOP-Q6F3A4OJ MINGW64 /e/Git笔记/learngit (master)
$ git commit -m 'update'
[master 3f03828] update
 1 file changed, 2 insertions(+), 1 deletion(-)

love。com@LAPTOP-Q6F3A4OJ MINGW64 /e/Git笔记/learngit (master)

~~~

提交后，我们再用`git status`命令看看仓库的当前状态：

```bash
$ git statusOn branch masternothing to commit, working tree clean
```

Git告诉我们当前没有需要提交的修改，而且，工作目录是干净（working tree clean）的。

**小结**：

- 要随时掌握工作区的状态，使用`git status`命令。
- 如果`git status`告诉你有文件被修改过，用`git diff`可以查看修改内容。

# 6、使用`git log`

`git log`命令显示从最近到最远的提交日志

嫌输出的信息太多可以用

```bash
git log --pretty=oneline
```

# 7、版本退回和还原

首先，Git必须知道当前版本是哪个版本，在Git中，用`HEAD`表示当前版本，也就是最新的提交`1094adb...`（注意我的提交ID和你的肯定不一样），上一个版本就是`HEAD^`，上上一个版本就是`HEAD^^`，当然往上100个版本写100个`^`比较容易数不过来，所以写成`HEAD~100`。

现在，我们要把当前版本`append GPL`回退到上一个版本`add distributed`，就可以使用`git reset`命令：

~~~bash
love。com@LAPTOP-Q6F3A4OJ MINGW64 /e/Git笔记/learngit (master)$ git reset --hard HEAD^HEAD is now at 72c881b first use gitlove。com@LAPTOP-Q6F3A4OJ MINGW64 /e/Git笔记/learngit (master)$ cat feige.txt第一次使用git
~~~

~~~bash
$ git reset --hard 3f038286caHEAD is now at 3f03828 updatelove。com@LAPTOP-Q6F3A4OJ MINGW64 /e/Git笔记/learngit (master)$ cat feige.txt第一次使用git使用 git status
~~~

只要命令行窗口没关，你可以找到你要回到的版本的版本号`commit id是`1094adb...`，就可以指定回到未来的某个版本

版本号没必要写全，前几位就可以了，Git会自动去找。当然也不能只写前一两位，因为Git可能会找到多个版本号，就无法确定是哪一个了。

当你用`$ git reset --hard HEAD^`回退到`add distributed`版本时，再想恢复到`append GPL`，就必须找到`append GPL`的commit id。Git提供了一个命令`git reflog`用来记录你的每一次命令：

~~~bash
$ git reflog3f03828 (HEAD -> master) HEAD@{0}: reset: moving to 3f038286ca72c881b HEAD@{1}: reset: moving to HEAD^3f03828 (HEAD -> master) HEAD@{2}: commit: update72c881b HEAD@{3}: commit (initial): first use git
~~~

**小结**

现在总结一下：

- `HEAD`指向的版本就是当前版本，因此，Git允许我们在版本的历史之间穿梭，使用命令`git reset --hard commit_id`。
- 穿梭前，用`git log`可以查看提交历史，以便确定要回退到哪个版本。
- 要重返未来，用`git reflog`查看命令历史，以便确定要回到未来的哪个版本。

# 8、工作区和暂存区

#### 工作区（Working Directory）

就是你在电脑里能看到的目录，比如我的`learngit`文件夹就是一个工作区

#### 版本库（Repository）

工作区有一个隐藏目录`.git`，这个不算工作区，而是Git的版本库。

Git的版本库里存了很多东西，其中最重要的就是称为stage（或者叫index）的暂存区，还有Git为我们自动创建的第一个分支`master`，以及指向`master`的一个指针叫`HEAD`。

前面我们把文件往Git版本库里添加的时候，是分两步执行的：

第一步是用`git add`把文件添加进去，实际上就是把文件修改添加到暂存区；

第二步是用`git commit`提交更改，实际上就是把暂存区的所有内容提交到当前分支。

`git add`命令实际上就是把要提交的所有修改放到暂存区（Stage），然后，执行`git commit`就可以一次性把暂存区的所有修改提交到分支。

~~~bash
$ git statusOn branch masterUntracked files:  (use "git add <file>..." to include in what will be committed)        dage.txtnothing added to commit but untracked files present (use "git add" to track)love。com@LAPTOP-Q6F3A4OJ MINGW64 /e/Git笔记/learngit (master)$ git add dage.txtlove。com@LAPTOP-Q6F3A4OJ MINGW64 /e/Git笔记/learngit (master)$ statusbash: status: command not foundlove。com@LAPTOP-Q6F3A4OJ MINGW64 /e/Git笔记/learngit (master)$ git statusOn branch masterChanges to be committed:  (use "git restore --staged <file>..." to unstage)        new file:   dage.txtlove。com@LAPTOP-Q6F3A4OJ MINGW64 /e/Git笔记/learngit (master)$ git add feige.txtlove。com@LAPTOP-Q6F3A4OJ MINGW64 /e/Git笔记/learngit (master)$ git statusOn branch masterChanges to be committed:  (use "git restore --staged <file>..." to unstage)        new file:   dage.txtlove。com@LAPTOP-Q6F3A4OJ MINGW64 /e/Git笔记/learngit (master)$ git commit -m 'dage'[master bd8a360] dage 1 file changed, 1 insertion(+) create mode 100644 dage.txtlove。com@LAPTOP-Q6F3A4OJ MINGW64 /e/Git笔记/learngit (master)$ git statusOn branch masternothing to commit, working tree clean
~~~

![image-20200212144043690](https://gitee.com/feigeCode/picture/raw/master/img/image-20200212144043690.png)

# 8、管理修改

用`git diff HEAD -- feige.txt`命令可以查看工作区和版本库里面最新版本的区别

每次修改，如果不用`git add`到暂存区，那就不会加入到`commit`中

~~~bash
$ git diff HEAD -- feige.txtdiff --git a/feige.txt b/feige.txtindex c6c9b52..3db5b1e 100644--- a/feige.txt+++ b/feige.txt@@ -1,4 +1,5 @@ 第一次使用git 使用 git status 暂存区-撤销修改\ No newline at end of file+撤销修改+feige\ No newline at end of filelove。com@LAPTOP-Q6F3A4OJ MINGW64 /e/Git笔记/learngit (master)$ git statusOn branch masterChanges not staged for commit:  (use "git add <file>..." to update what will be committed)  (use "git restore <file>..." to discard changes in working directory)        modified:   feige.txtno changes added to commit (use "git add" and/or "git commit -a")love。com@LAPTOP-Q6F3A4OJ MINGW64 /e/Git笔记/learngit (master)$ git checkout -- feige.txtlove。com@LAPTOP-Q6F3A4OJ MINGW64 /e/Git笔记/learngit (master)$ cat feige.txt第一次使用git使用 git status暂存区撤销修改love。com@LAPTOP-Q6F3A4OJ MINGW64 /e/Git笔记/learngit (master)$ git statusOn branch masternothing to commit, working tree clean
~~~



# 9、撤销修改

```bash
$ git checkout -- feige.txt
```

**该命令可以撤销还未add和commit的文件**

命令`git checkout -- feige.txt`意思就是，把`feige.txt`文件在工作区的修改全部撤销，这里有两种情况：

一种是`readme.txt`自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；

一种是`readme.txt`已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。

总之，就是让这个文件回到最近一次`git commit`或`git add`时的状态。



**命令`git reset HEAD `可以把暂存区的修改撤销掉（unstage），重新放回工作区**

~~~bash
$ git reset HEAD feige.txtUnstaged changes after reset:M       feige.txtlove。com@LAPTOP-Q6F3A4OJ MINGW64 /e/Git笔记/learngit (master)$ git statusOn branch masterChanges not staged for commit:  (use "git add <file>..." to update what will be committed)  (use "git restore <file>..." to discard changes in working directory)        modified:   feige.txtno changes added to commit (use "git add" and/or "git commit -a")
~~~

**小结**

场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令`git checkout -- file`。

场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令`git reset HEAD `，就回到了场景1，第二步按场景1操作。

场景3：已经提交了不合适的修改到版本库时，想要撤销本次提交，参考[版本回退](https://www.liaoxuefeng.com/wiki/896043488029600/897013573512192)一节，不过前提是没有推送到远程库。



# 10、删除文件

手动删除一个文件或rm删除一个文件，Git知道你删除了文件，因此，工作区和版本库就不一致了，`git status`可以知道哪些文件被删除了，如果确实要从版本库中删除该文件，那就用命令`git rm`删掉，并且`git commit`：

~~~bash
$ rm dage.txtlove。com@LAPTOP-Q6F3A4OJ MINGW64 /e/Git笔记/learngit (master)$ git statusOn branch masterChanges not staged for commit:  (use "git add/rm <file>..." to update what will be committed)  (use "git restore <file>..." to discard changes in working directory)        deleted:    dage.txtno changes added to commit (use "git add" and/or "git commit -a")love。com@LAPTOP-Q6F3A4OJ MINGW64 /e/Git笔记/learngit (master)$ git rm dage.txtrm 'dage.txt'love。com@LAPTOP-Q6F3A4OJ MINGW64 /e/Git笔记/learngit (master)$ git statusOn branch masterChanges to be committed:  (use "git restore --staged <file>..." to unstage)        deleted:    dage.txtlove。com@LAPTOP-Q6F3A4OJ MINGW64 /e/Git笔记/learngit (master)$ git commit -m 'remove dage.txt'[master 25fe051] remove dage.txt 1 file changed, 1 deletion(-) delete mode 100644 dage.txt
~~~



手动删除一个文件或rm删除一个文件时可以通过以下命令撤销

~~~bash
love。com@LAPTOP-Q6F3A4OJ MINGW64 /e/Git笔记/learngit (master)$ git statusOn branch masterChanges not staged for commit:  (use "git add/rm <file>..." to update what will be committed)  (use "git restore <file>..." to discard changes in working directory)        deleted:    dage.txtno changes added to commit (use "git add" and/or "git commit -a")love。com@LAPTOP-Q6F3A4OJ MINGW64 /e/Git笔记/learngit (master)$ git checkout -- dage.txt
~~~

`git checkout`其实是用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以“一键还原”。

 注意：从来没有被添加到版本库就被删除的文件，是无法恢复的！

**小结**

命令`git rm`用于删除一个文件。如果一个文件已经被提交到版本库，那么你永远不用担心误删，但是要小心，你只能恢复文件到最新版本，你会丢失**最近一次提交后你修改的内容**。





# 11、远程仓库

第1步：创建SSH Key。在用户主目录下，看看有没有.ssh目录，如果有，再看看这个目录下有没有`id_rsa`和`id_rsa.pub`这两个文件，如果已经有了，可直接跳到下一步。如果没有，打开Shell（Windows下打开Git Bash），创建SSH Key：

~~~bash
ssh-keygen -t rsa -C "1835698775@qq.com"
~~~

你需要把邮件地址换成你自己的邮件地址，然后一路回车，使用默认值即可，由于这个Key也不是用于军事目的，所以也无需设置密码。

如果一切顺利的话，可以在用户主目录里找到`.ssh`目录，里面有`id_rsa`和`id_rsa.pub`两个文件，这两个就是SSH Key的秘钥对，`id_rsa`是私钥，不能泄露出去，`id_rsa.pub`是公钥，可以放心地告诉任何人。

第2步：登陆GitHub，打开“Account settings”，“SSH Keys”页面：

然后，点“Add SSH Key”，填上任意Title，在Key文本框里粘贴`id_rsa.pub`文件的内容：

![image-20200212213850910](https://gitee.com/feigeCode/picture/raw/master/img/image-20200212213850910.png)

为什么GitHub需要SSH Key呢？因为GitHub需要识别出你推送的提交确实是你推送的，而不是别人冒充的，而Git支持SSH协议，所以，GitHub只要知道了你的公钥，就可以确认只有你自己才能推送。

# 11、创建远程仓库

~~~bash
love。com@LAPTOP-Q6F3A4OJ MINGW64 /e/Git笔记/learngit (master)$ echo "# java" >> README.mdlove。com@LAPTOP-Q6F3A4OJ MINGW64 /e/Git笔记/learngit (master)$ git add README.mdwarning: LF will be replaced by CRLF in README.md.The file will have its original line endings in your working directorylove。com@LAPTOP-Q6F3A4OJ MINGW64 /e/Git笔记/learngit (master)$ git commit -m "first commit"[master 0dfe941] first commit 1 file changed, 1 insertion(+) create mode 100644 README.mdlove。com@LAPTOP-Q6F3A4OJ MINGW64 /e/Git笔记/learngit (master)$ git remote add origin git@github.com:feigeCode/java.git
~~~

添加后，远程库的名字就是`origin`，这是Git默认的叫法，也可以改成别的，但是`origin`这个名字一看就知道是远程库。

下一步，就可以把本地库的所有内容推送到远程库上：

~~~bash
love。com@LAPTOP-Q6F3A4OJ MINGW64 /e/Git笔记/learngit (master)$ git push -u origin masterCounting objects: 100% (23/23), done.Delta compression using up to 8 threadsCompressing objects: 100% (13/13), done.Writing objects: 100% (23/23), 1.90 KiB | 277.00 KiB/s, done.Total 23 (delta 0), reused 0 (delta 0)To github.com:feigeCode/java.git * [new branch]      master -> masterBranch 'master' set up to track remote branch 'master' from 'origin'.
~~~

把本地库的内容推送到远程，用`git push`命令，实际上是把当前分支`master`推送到远程。

由于远程库是空的，我们第一次推送`master`分支时，加上了`-u`参数，Git不但会把本地的`master`分支内容推送的远程新的`master`分支，还会把本地的`master`分支和远程的`master`分支关联起来，在以后的推送或者拉取时就可以简化命令。

从现在起，只要本地作了提交，就可以通过命令：

```bash
$ git push origin master
```

把本地`master`分支的最新修改推送至GitHub，现在，你就拥有了真正的分布式版本库！

**SSH警告**

当你第一次使用Git的`clone`或者`push`命令连接GitHub时，会得到一个警告：

```bash
The authenticity of host 'github.com (xx.xx.xx.xx)' can't be established.RSA key fingerprint is xx.xx.xx.xx.xx.Are you sure you want to continue connecting (yes/no)?
```

这是因为Git使用SSH连接，而SSH连接在第一次验证GitHub服务器的Key时，需要你确认GitHub的Key的指纹信息是否真的来自GitHub的服务器，输入`yes`回车即可。

Git会输出一个警告，告诉你已经把GitHub的Key添加到本机的一个信任列表里了：

```bash
Warning: Permanently added 'github.com' (RSA) to the list of known hosts.
```

这个警告只会出现一次，后面的操作就不会有任何警告了。

如果你实在担心有人冒充GitHub服务器，输入`yes`前可以对照[GitHub的RSA Key的指纹信息](https://help.github.com/articles/what-are-github-s-ssh-key-fingerprints/)是否与SSH连接给出的一致。

**小结**

要关联一个远程库，使用命令`git remote add origin git@server-name:path/repo-name.git`；

关联后，使用命令`git push -u origin master`第一次推送master分支的所有内容；

此后，每次本地提交后，只要有必要，就可以使用命令`git push origin master`推送最新修改；

分布式版本系统的最大好处之一是在本地工作完全不需要考虑远程库的存在，也就是有没有联网都可以正常工作，而SVN在没有联网的时候是拒绝干活的！当有网络的时候，再把本地提交推送一下就完成了同步，真是太方便了！

# 12、从远程库克隆

![image-20200212220313374](https://gitee.com/feigeCode/picture/raw/master/img/image-20200212220313374.png)

命令`git clone`克隆一个本地库：

~~~bash
git clone git@github.com:feigeCode/Vue.git
~~~

如果有多个人协作开发，那么每个人各自从远程克隆一份就可以了。

你也许还注意到，GitHub给出的地址不止一个，还可以用`https://github.com/michaelliao/gitskills.git`这样的地址。实际上，Git支持多种协议，默认的`git://`使用ssh，但也可以使用`https`等其他协议。

使用`https`除了速度慢以外，还有个最大的麻烦是每次推送都必须输入口令，但是在某些只开放http端口的公司内部就无法使用`ssh`协议而只能用`https`。

**小结**

要克隆一个仓库，首先必须知道仓库的地址，然后使用`git clone`命令克隆。

Git支持多种协议，包括`https`，但通过`ssh`支持的原生`git`协议速度最快。

# 13、创建与合并分支

创建`dev`分支，然后切换到`dev`分支

`git checkout`命令加上`-b`参数表示创建并切换，相当于以下两条命令

用`git branch`命令查看当前分支

~~~bash
$ git checkout -b devSwitched to a new branch 'dev'love。com@LAPTOP-Q6F3A4OJ MINGW64 /e/Git笔记/learngit (dev)$ git branch devfatal: A branch named 'dev' already exists.love。com@LAPTOP-Q6F3A4OJ MINGW64 /e/Git笔记/learngit (dev)$ git checkout devAlready on 'dev'
~~~

`git branch`命令会列出所有分支，当前分支前面会标一个`*`号。

~~~bash
git branch* dev  master
~~~

我们就可以在`dev`分支上正常提交

然后提交：

```bash
$ git add feige.txt $ git commit -m "branch test"[dev b17d20e] branch test 1 file changed, 1 insertion(+)
```

现在，`dev`分支的工作完成，我们就可以切换回`master`分支：

```bash
$ git checkout masterSwitched to branch 'master'
```

切换回`master`分支后，再查看一个`feige.txt`文件，刚才添加的内容不见了！因为那个提交是在`dev`分支上，而`master`分支此刻的提交点并没有变

现在，我们把`dev`分支的工作成果合并到`master`分支上：

```bash
$ git merge devUpdating d46f35e..b17d20eFast-forward readme.txt | 1 + 1 file changed, 1 insertion(+)
```

`git merge`命令用于合并指定分支到当前分支。合并后，再查看`readme.txt`的内容，就可以看到，和`dev`分支的最新提交是完全一样的。

注意到上面的`Fast-forward`信息，Git告诉我们，这次合并是“快进模式”，也就是直接把`master`指向`dev`的当前提交，所以合并速度非常快。

当然，也不是每次合并都能`Fast-forward`，我们后面会讲其他方式的合并。

合并完成后，就可以放心地删除`dev`分支了：

```bash
$ git branch -d devDeleted branch dev (was b17d20e).
```

删除后，查看`branch`，就只剩下`master`分支了：

```bash
$ git branch* master
```

因为创建、合并和删除分支非常快，所以Git鼓励你使用分支完成某个任务，合并后再删掉分支，这和直接在`master`分支上工作效果是一样的，但过程更安全。

~~~bash
$ git statusOn branch devChanges not staged for commit:  (use "git add <file>..." to update what will be committed)  (use "git restore <file>..." to discard changes in working directory)        modified:   feige.txtno changes added to commit (use "git add" and/or "git commit -a")love。com@LAPTOP-Q6F3A4OJ MINGW64 /e/Git笔记/learngit (dev)$ git add feige.txtlove。com@LAPTOP-Q6F3A4OJ MINGW64 /e/Git笔记/learngit (dev)$ git commit -m 'branch test'[dev 85aa617] branch test 1 file changed, 2 insertions(+), 1 deletion(-)love。com@LAPTOP-Q6F3A4OJ MINGW64 /e/Git笔记/learngit (dev)$ git checkout masterSwitched to branch 'master'Your branch is up to date with 'origin/master'.love。com@LAPTOP-Q6F3A4OJ MINGW64 /e/Git笔记/learngit (master)$ git merge devUpdating 0dfe941..85aa617Fast-forward feige.txt | 3 ++- 1 file changed, 2 insertions(+), 1 deletion(-)
~~~

**switch**

我们注意到切换分支使用`git checkout `，而前面的撤销修改是`git checkout -- `，同一个命令，有两种作用，确实有点令人迷惑。

最新版本的Git提供了新的`git switch`命令来切换分支：

创建并切换到新的`dev`分支，可以使用：

```bash
$ git switch -c dev
```

直接切换到已有的`master`分支，可以使用：

```bash
$ git switch master
```

使用新的`git switch`命令，比`git checkout`要更容易理解。

**小结**

Git鼓励大量使用分支：

查看分支：`git branch`

创建分支：`git branch `

切换分支：`git checkout `或者`git switch `

创建+切换分支：`git checkout -b `或者`git switch -c `

合并某分支到当前分支：`git merge `

删除分支：`git branch -d `

# 14、解决冲突

当Git无法自动合并分支时，就必须首先解决冲突。解决冲突后，再提交，合并完成。

解决冲突就是把Git合并失败的文件手动编辑为我们希望的内容，再提交。

用`git log --graph`命令可以看到分支合并图。

# 15、分支策略

在实际开发中，我们应该按照几个基本原则进行分支管理：

首先，`master`分支应该是非常稳定的，也就是仅用来发布新版本，平时不能在上面干活；

那在哪干活呢？干活都在`dev`分支上，也就是说，`dev`分支是不稳定的，到某个时候，比如1.0版本发布时，再把`dev`分支合并到`master`上，在`master`分支发布1.0版本；

你和你的小伙伴们每个人都在`dev`分支上干活，每个人都有自己的分支，时不时地往`dev`分支上合并就可以了。

所以，团队合作的分支看起来就像这样：

![git-br-policy](https://gitee.com/feigeCode/picture/raw/master/img/0)

**小结**

Git分支十分强大，在团队开发中应该充分应用。

合并分支时，加上`--no-ff`参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而`fast forward`合并就看不出来曾经做过合并。

# 16、Bug分支

Git提供了一个`stash`功能，可以把当前工作现场“储藏”起来，等以后恢复现场后继续工作：

```bash
$ git stashSaved working directory and index state WIP on dev: f52c633 add merge
```

一是用`git stash apply`恢复，但是恢复后，stash内容并不删除，你需要用`git stash drop`来删除；

另一种方式是用`git stash pop`，恢复的同时把stash内容也删了：

```bash
$ git stash popOn branch devChanges to be committed:  (use "git reset HEAD <file>..." to unstage)	new file:   hello.pyChanges not staged for commit:  (use "git add <file>..." to update what will be committed)  (use "git checkout -- <file>..." to discard changes in working directory)	modified:   readme.txtDropped refs/stash@{0} (5d677e2ee266f39ea296182fb2354265b91b3b2a)
```

再用`git stash list`查看，就看不到任何stash内容了：

```bash
$ git stash list
```

你可以多次stash，恢复的时候，先用`git stash list`查看，然后恢复指定的stash，用命令：

```bash
$ git stash apply stash@{0}
```

**小结**

修复bug时，我们会通过创建新的bug分支进行修复，然后合并，最后删除；

当手头工作没有完成时，先把工作现场`git stash`一下，然后去修复bug，修复后，再`git stash pop`，回到工作现场；

在master分支上修复的bug，想要合并到当前dev分支，可以用`git cherry-pick `命令，把bug提交的修改“复制”到当前分支，避免重复劳动。





- `master`分支是主分支，因此要时刻与远程同步；
- `dev`分支是开发分支，团队所有成员都需要在上面工作，所以也需要与远程同步；
- bug分支只用于在本地修复bug，就没必要推到远程了，除非老板要看看你每周到底修复了几个bug；
- feature分支是否推到远程，取决于你是否和你的小伙伴合作在上面开发。

总之，就是在Git中，分支完全可以在本地自己藏着玩，是否推送，视你的心情而定！





因此，多人协作的工作模式通常是这样：

1. 首先，可以试图用`git push origin `推送自己的修改；
2. 如果推送失败，则因为远程分支比你的本地更新，需要先用`git pull`试图合并；
3. 如果合并有冲突，则解决冲突，并在本地提交；
4. 没有冲突或者解决掉冲突后，再用`git push origin `推送就能成功！

如果`git pull`提示`no tracking information`，则说明本地分支和远程分支的链接关系没有创建，用命令`git branch --set-upstream-to  origin/`。

这就是多人协作的工作模式，一旦熟悉了，就非常简单。

**小结**

- 查看远程库信息，使用`git remote -v`；
- 本地新建的分支如果不推送到远程，对其他人就是不可见的；
- 从本地推送分支，使用`git push origin branch-name`，如果推送失败，先用`git pull`抓取远程的新提交；
- 在本地创建和远程分支对应的分支，使用`git checkout -b branch-name origin/branch-name`，本地和远程分支的名称最好一致；
- 建立本地分支和远程分支的关联，使用`git branch --set-upstream branch-name origin/branch-name`；
- 从远程抓取分支，使用`git pull`，如果有冲突，要先处理冲突。

- 命令`git tag `用于新建一个标签，默认为`HEAD`，也可以指定一个commit id；
- 命令`git tag -a  -m "blablabla..."`可以指定标签信息；
- 命令`git tag`可以查看所有标签。