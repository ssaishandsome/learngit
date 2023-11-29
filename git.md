### 一、初始化git和创建版本库

###### 设置名字和邮箱

```
$ git config --global user.name "Your Name"
$ git config --global user.email "email@example.com"
```

###### 创建版本库

```
$ git init
Initialized empty Git repository in /Users/michael/learngit/.git/
```

###### 将文件添加到仓库

​	**git add readme.txt** 

###### 将文件提交到仓库

​	**git commit -m "备注"**  //-m是本次提交说明

### 二、时光机穿梭

###### 查看仓库当前状态

​	**git status**

```
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   readme.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

上述结果表明readme.txt文件被修改了，但是还未提交

###### 查看具体修改内容

​	**git diff readme.txt**

```
$ git diff readme.txt 
diff --git a/readme.txt b/readme.txt
index 46d49bf..9247db6 100644
--- a/readme.txt
+++ b/readme.txt
@@ -1,2 +1,2 @@
-Git is a version control system.
+Git is a distributed version control system.
 Git is free software.
```

​	可以看出修改了语句

###### 查看修改的历史记录

​	**git log**

```
$ git log
commit 1094adb7b9b3807259d8cb349e7df1d4d6477073 (HEAD -> master)
Author: Michael Liao <askxuefeng@gmail.com>
Date:   Fri May 18 21:06:15 2018 +0800

    append GPL

commit e475afc93c209a690c39c13a46716e8fa000c366
Author: Michael Liao <askxuefeng@gmail.com>
Date:   Fri May 18 21:03:36 2018 +0800

    add distributed

commit eaadf4e385e865d25c48e7ca9c8395c3f7dfaef0
Author: Michael Liao <askxuefeng@gmail.com>
Date:   Fri May 18 20:59:18 2018 +0800

    wrote a readme file
```

这里的commit代表是快照，你可以通过该快照进行文件回溯

###### 回溯文件

​	**git reset --hard HEAD^**  //HEAD表示当前版本，HEAD^表示上一个版本，HEAD^^表示上上一个版本，HEAD~100表示上100个版本

```
$ git reset --hard HEAD^
HEAD is now at e475afc add distributed
```

###### 回到现在

​	git reset --hard 版本号（commit id）  //前提你要记住先前的commit id

```
$ git reset --hard 1094a  //写前几位就好，会自动查找
HEAD is now at 83b0afe append GPL
```

Git的版本回退速度非常快，因为Git在内部有个指向当前版本的`HEAD`指针，当你回退版本的时候，Git仅仅是把HEAD从指向`append GPL`：

```ascii
┌────┐
│HEAD│
└────┘
   │
   └──▶ ○ append GPL
        │
        ○ add distributed
        │
        ○ wrote a readme file
```

改为指向`add distributed`：

```ascii
┌────┐
│HEAD│
└────┘
   │
   │    ○ append GPL
   │    │
   └──▶ ○ add distributed
        │
        ○ wrote a readme file
```

如果你已经关闭电脑找不到之前的commit id了 ，你可以使用

​	**git reflog** //记录你的每一次命令

```
$ git reflog
e475afc HEAD@{1}: reset: moving to HEAD^
1094adb (HEAD -> master) HEAD@{2}: commit: append GPL
e475afc HEAD@{3}: commit: add distributed
eaadf4e HEAD@{4}: commit (initial): wrote a readme file
```

然后你就可以看到先前的commit id



### 三、工作区和暂存区

###### 工作区

​	电脑中能看到的目录，比如电脑中的learngit文件夹

###### 版本库

​	工作区中有个隐藏目录**.git** ，它就是git的版本库

![image-20231128151808745](C:\Users\1\AppData\Roaming\Typora\typora-user-images\image-20231128151808745.png)

这里的stage就是暂存区，以及Git为我们创建的第一个分支**master** 还有指向master的指针**HEAD** 

`git add`就是将文件先添加到暂存区；

`git commit` 就是提交更改，将暂存区内容提交到当前分支中去

###### 管理修改

​	第一次修改 -> `git add` -> 第二次修改 -> `git commit`

​	Git管理的是修改，当你用`git add`命令后，在工作区的第一次修改被放入暂存区，准备提交，但是，在工作区的第二次修改并没有放入暂存区，所以，`git commit`只负责把暂存区的修改提交了，也就是第一次的修改被提交了，第二次的修改不会被提交。

​	因此可以使用，`git diff HEAD -- readme.txt` 查看工作区和版本库中的区别

```
$ git diff HEAD -- readme.txt 
diff --git a/readme.txt b/readme.txt
index 76d770f..a9c5755 100644
--- a/readme.txt
+++ b/readme.txt
@@ -1,4 +1,4 @@
 Git is a distributed version control system.
 Git is free software distributed under the GPL.
 Git has a mutable index called stage.
-Git tracks changes.
+Git tracks changes of files.
```

###### 撤销修改

​	`git checkout --readme.txt`  //丢弃在工作区的修改

​	这里有两种情况：

一种是`readme.txt`自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；（版本库中依然要存在）

一种是`readme.txt`已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。



​	但是上述问题都是在你并没有把你发的疯 `git add` 到暂存区中；现在你又犯病了，你不仅改了文件，还放到了暂存区。

​	`git reset HEAD <file> `  //将暂存区中的修改撤销，放回到工作区

​	撤销之后，暂存区的修改没有了，工作区存在修改，然后再用 `git check `删除工作区的修改



###### 删除文件

​	`rm test.txt`       --在工作区把文件删除后

​	`git rm test.txt`  然后使用 `git commit`  就可以在版本库中删除文件

​	`git checkout -- test.txt ` 撤销对工作区的修改（删除也是一种修改） 

​    注意：从来没有被添加到版本库就被删除的文件，是无法恢复的！



### 四、远程仓库

###### 准备操作

第1步：创建SSH Key。在用户主目录下，看看有没有.ssh目录，如果有，再看看这个目录下有没有`id_rsa`和`id_rsa.pub`这两个文件，如果已经有了，可直接跳到下一步。如果没有，打开Shell（Windows下打开Git Bash），创建SSH Key：

```
$ ssh-keygen -t rsa -C "youremail@example.com"
```

第2步：登陆GitHub，打开“Account settings”，“SSH Keys”页面：

然后，点“Add SSH Key”，填上任意Title，在Key文本框里粘贴`id_rsa.pub`文件的内容

###### 添加远程仓库

```
$ git remote add origin     git@github.com:michaelliao/learngit.git
```

`michaelliao` 替换成自己的GitHub名称，关联到自己的远程库，添加后，远程库的名称就叫  `origin` 

```
$ git push -u origin master
Counting objects: 20, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (15/15), done.
Writing objects: 100% (20/20), 1.64 KiB | 560.00 KiB/s, done.
Total 20 (delta 5), reused 0 (delta 0)
remote: Resolving deltas: 100% (5/5), done.
To github.com:michaelliao/learngit.git
 * [new branch]      master -> master
Branch 'master' set up to track remote branch 'master' from 'origin'.
```

`git push` 命令将当前分支 `master` 推送到远程。

`-u` 操作不仅能把本地分支 `master` 推送到远程新的 `master` 分支中，还会把本地的 `master` 分支和远程 `master` 分支关联起来

从现在起，只要本地作了提交，就可以通过命令：

```
$ git push origin master
```



###### 删除远程库

```
$ git remote -v
origin  git@github.com:michaelliao/learn-git.git (fetch)
origin  git@github.com:michaelliao/learn-git.git (push)
```

`git remote -v` 可以查看远程库信息

```
$ git remote rm origin
```

再使用 `git remote rm <name>` 删除远程库



###### 从远程库克隆

```
$ git clone git@github.com:ssaishandsome/learngit.git
```

然后你就可以在该目录下看到远程库中的文件



### 五、分支管理

分支就是科幻电影里面的平行宇宙，当你正在电脑前努力学习Git的时候，另一个你正在另一个平行宇宙里努力学习SVN。

如果两个平行宇宙互不干扰，那对现在的你也没啥影响。不过，在某个时间点，两个平行宇宙合并了，结果，你既学会了Git又学会了SVN！

![learn-branches](https://www.liaoxuefeng.com/files/attachments/919021987875136/0)





###### 创建并合并分支

在回溯文件中，你已经知道，每次提交，Git都把它们串成一条时间线，这条时间线就是一个分支。截止到目前，只有一条时间线，在Git里，这个分支叫主分支，即`master`分支。`HEAD`严格来说不是指向提交，而是指向`master`，`master`才是指向提交的，所以，`HEAD`指向的就是当前分支。

一开始的时候，`master`分支是一条线，Git用`master`指向最新的提交，再用`HEAD`指向`master`，就能确定当前分支，以及当前分支的提交点：

```ascii
                  HEAD
                    │
                    │
                    ▼
                 master
                    │
                    │
                    ▼
┌───┐    ┌───┐    ┌───┐
│   │───▶│   │───▶│   │
└───┘    └───┘    └───┘
```



当我们创建新的分支，例如`dev`时，Git新建了一个指针叫`dev`，指向`master`相同的提交，再把`HEAD`指向`dev`，就表示当前分支在`dev`上：

```ascii
                 master
                    │
                    │
                    ▼
┌───┐    ┌───┐    ┌───┐
│   │───▶│   │───▶│   │
└───┘    └───┘    └───┘
                    ▲
                    │
                    │
                   dev
                    ▲
                    │
                    │
                  HEAD
```
