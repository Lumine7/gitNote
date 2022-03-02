### repo repository 仓库 版本库

是一个意思



命令 `pwd`可以显示当前目录

命令`ls`或者`dir`可以查看当前目录下的文件



##### 所有版本控制系统只能跟踪文本文件的改动

## 将文件添加到git仓库/提交修改：两步

0.创建一个文件rrr.txt

1.`git add rrr.txt`;告诉git这个东西准备提交，**正确执行后应该不出现任何信息**，相当于提交到暂存区

tips：如果文件名含有空格，需要将名字用**双引号**括起来，单引号不行！

tips2:可以在提交前多次向暂存区add一个东西，commit时只会提交最新的

2.`git commit -m 'xxx'`将文件添加到仓库；完成后会告诉你几个文件变了，几处插入、删除等；'xxx'是关于本次提交的信息

一次可以提交很多更改！



## 仓库的状态

#### `git status`用来查看仓库当前的状态

**如果文件没有change，所有add都已经提交（刚刚提交完的情况）,会显示** 

> nothing to commit, working tree clean

**如果你更改了但是没add，会告诉你**

> Changes not staged for commit:
>   (use "git add <file>..." to update what will be committed)
>   (use "git restore <file>..." to discard changes in working directory)

**如果已经add了更改但是还没有commit，会告诉你**

> Changes to be committed:
>   (use "git restore --staged <file>..." to unstage)
>         modified:   git note1.md

#### `git diff`查看差异

在有修改还没add的时候，可以通过git diff查看修改了什么地方

如果什么都没有显示说明add过了

#### `git log` 查看由近到远的提交记录

注意那些一大串字符是版本号，很长很乱是hash过了防止冲突

使用`git log --pretty=oneline`更看简洁的查看



## git仓库的结构

分为工作区（你能看到和直接修改的），暂存区和本地仓库

## 版本回退和撤销修改

首先，git中，`HEAD`表示当前版本，`HEAD^`表示上一个版本，`HEAD^^`表示上两个版本，`HEAD~100`表示上100个版本

#### `git reset --hard HEAD^`回到上一个版本：时光穿梭

更广义的，通过`git reset --hard commit_id`回到版本号为commit_id 的版本

穿梭前，最好先用`git log`查看历史

穿梭后，如果想回到未来，需要先知道未来那个版本号

如果之前有用过git log，可以直接复制版本号

如果没有，可以使用`git reflog`查看命令历史，找到那个版本号

tips：git中的版本穿梭不会对之前提交过的commit做出修改，只是修改HEAD指针

说白了，版本之间的穿梭是平行时空里的，可以随便在任何版本间乱跳！



git管理的是修改，不是文件；只有add到暂存区的修改才会被commit

#### 丢弃修改

当你对工作区的文件进行修改但是**还没有add时**，可以使用`git checkout -- file`来丢弃工作区的修改

回退到的版本即你上一次add或者commit后的状态

注意一但你add之后就没法回退了

那么问题来了，如果你已经将修改提交到了暂存区了，这时候怎么回退呢？

#### `git reset HEAD filename` 将暂存区的修改回退到工作区，即清空暂存区

好像没什么需要解释的



#### 总结：

* 工作区回退：`git checkout -- filename`
* 暂存区回退：`git reset HEAD filename`回退到工作区，然后再运行上面一条彻底删掉
* 本地仓库回退：`git reset --hard HEAD^`
* 如果你已经推送到了远程仓库：对不起



## 删除文件

在工作区删除一个仓库中有的文件后，

如果（你想从本地仓库中删掉它），运行`git rm filename`

如果你误删了，直接撤销工作区的修改即可

如果一个文件已经被提交到版本库，那你永远都不需要担心误删，只要版本库在它就存储着历史上的所有版本！
