### 初始化git和创建版本库

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

​	git add readme.txt 

###### 将文件提交到仓库

​	git commit -m "备注"  //-m是本次提交说明

### 时光机穿梭

###### 查看仓库当前状态

​	git status

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

​	git diff readme.txt

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

​	git log

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

​	git reset --hard HEAD^  //HEAD表示当前版本，HEAD^表示上一个版本，HEAD^^表示上上一个版本，HEAD~100表示上100个版本

```
$ git reset --hard HEAD^
HEAD is now at e475afc add distributed
```