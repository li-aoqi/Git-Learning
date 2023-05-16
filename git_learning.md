# git 学习笔记——

学习参考廖雪峰https://www.liaoxuefeng.com/wiki/896043488029600

## 创作版本库

1.选择一个合适的地方，创建空目录

```
$ mkdir learngit
$ cd learngit
$ pwd
/Users/michael/learngit
```

2.通过`git init`把这个目录变成git可以管理的仓库

```
$ git init
Initialized empty Git repository in /Users/michael/learngit/.git/
```

3.添加文件并提交到仓库

```
$ git add readme.txt
$ git commit -m "wrote a readme file"
```

## 回退版本

```
$ git reset --hard HEAD^
```

在Git中，我们用`git log`命令查看改了什么内容

如果嫌输出信息太多，看得眼花缭乱的，可以试试加上`--pretty=oneline`参数：

命令`git reflog`用来记录你的每一次命令

```
$ git log --pretty=oneline
```

- `HEAD`指向的版本就是当前版本，因此，Git允许我们在版本的历史之间穿梭，使用命令`git reset --hard commit_id`。
- 穿梭前，用`git log`可以查看提交历史，以便确定要回退到哪个版本。
- 要重返未来，用`git reflog`查看命令历史，以便确定要回到未来的哪个版本。

## 工作区和暂存区

用`git status`查看一下状态

`git add`命令实际上就是把要提交的所有修改放到暂存区（Stage），然后，执行`git commit`就可以一次性把暂存区的所有修改提交到分支。

## 管理修改

每次修改，如果不用`git add`到暂存区，那就不会加入到`commit`中。

## 撤销修改

没有add---------`git checkout -- file`可以丢弃工作区的修改

add了------------用命令`git reset HEAD <file>`可以把暂存区的修改撤销掉（unstage），重新放回工作区

## 删除文件

一是确实要从版本库中删除该文件，那就用命令`git rm`删掉，并且`git commit`：

```
$ git rm test.txt
rm 'test.txt'

$ git commit -m "remove test.txt"
[master d46f35e] remove test.txt
 1 file changed, 1 deletion(-)
 delete mode 100644 test.txt
```

另一种情况是删错了，因为版本库里还有呢，所以可以很轻松地把误删的文件恢复到最新版本：

```
git checkout -- test.txt
```

命令`git rm`用于删除一个文件。如果一个文件已经被提交到版本库，那么永远不用担心误删，但是要小心，只能恢复文件到最新版本，会丢失**最近一次提交后你修改的内容**。

## 远程仓库

GitHub告诉我们，可以从这个仓库克隆出新的仓库，也可以把一个已有的本地仓库与之关联，然后，把本地仓库的内容推送到GitHub仓库。

```
git remote add origin git@github.com:michaelliao/learngit.git
```

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

```
$ git push origin master
```

要关联一个远程库，使用命令`git remote add origin git@server-name:path/repo-name.git`；

关联一个远程库时必须给远程库指定一个名字，`origin`是默认习惯命名；

关联后，使用命令`git push -u origin master`第一次推送master分支的所有内容；

此后，每次本地提交后，只要有必要，就可以使用命令`git push origin master`推送最新修改；