**也许这些仍然没有显示出git的强大之处，下面我们介绍git最屌的地方**

电脑的ssh公钥和私钥在用户 .ssh文件夹里



### 关联本地仓库和原远程仓库

如果你的github上已经建立了一个远程仓库，用以下命令将一个本地仓库与之关联

```
git remote add origin url
```

url形如以下形式`git@github.com:michaelliao/learngit.git`

远程库的名字一般写作origin，之后就可以用origin指代远程库

#### ` git push -u origin master` 将本地仓库推送到远程（第一次）

之后再推送时就可以用`git push origin master`

#### `git remote -v `用来查看远程库信息





#### `git remote rm origin`和远程库解除关联

注意远程库还是存在的，只是和本地仓库解除关联了，要真的从github服务器上删除远程库，需要登录github账号找到删除按钮手动操作



## 克隆已存在的远程仓库

`git clone git@github.com:michaelliao/gitskills.git`

如果你发现链接是https形式的网站，常常需要将https改为git，否则可能用不了







# :star: 分支管理 :star:

HEAD永远指向的是当前分支。当只有一个分支时，HEAD指向的就是master

master是原始分支

#### 创建分支`git branch name`

相当于创建了一个指向当前仓库的指针

#### 查看所有分支`git branch`

当前分支前面会带上一个星号

#### 切换分支`git checkout name` 或`git switch name`

实际上就是让HEAD指针指向name所示的分支

#### 创建分支同时切换到新创建的分支`git checkout -b name` 或`git switch -c name`



#### 合并某分支到当前分支`git merge name`

如果出现了`Fast-forward`说明进行了快进模式合并，即让master直接指向name，但也有一些时候不能快速合并

当合并完成后，我们可以删掉dev分支，即刚才的name

#### 删除分支 ` git branch -d name`

其实就是删除了一个指针



经过实验，发现：

1.切换分支后工作区的文件也会改变成对应分支下的内容

2.如果一个分支内有修改还没有提交commit，这时切换分支会报错；不要强制进行切换分支（我不会再这里写明方法）



### 当无法快进合并

如果分支后，master进行了修改并提交，dev也进行了修改并提交，这时两者就已经不在一个时间线上了，产生了平行时空（分叉）

此时就无法进行快进合并了

当然，如果这时候运行`git merge` ，会进行自动合并，提示你`Auto-merging`

如果两边的修改是不同的文件或者一个文件不同的两处，那么通常是可以自动合并的

如果两边的修改有重叠的部分，就会产生**冲突**

这时需要先手动解决冲突，即手动修改某一个分支中的文件，让两个分支可以自动合并

合并之后，两条世界线有变成一条啦

可以用`git log --graph`来查看分支合并图









问题：

1.如果master领先了dev会发生什么？

好像也不会发生什么，不过这时好像可以在dev分支上用`git merge master`，会将dev指针移到master处

如果master在dev后面，`git merge master`是不起作用的





problems:

1.切换分支之前，需要将修改add和commit，否则可能会将修改带到另一个分支（工作区内容不变）

最恶心的地方在于不是每次都会，有时会，然后会给文件名前面显示一个M，也有的显示U

有时就会报error，目前还不清楚什么时候会成功切换，什么时候会error

### 总之，切换分支之前一定记得add和commit，最好多查看查看status

当然，另外还有一种方法，后面会介绍

2.如果已经commit，这时切换分支，对应工作区会改变

3.如何手动解决冲突?

实验了一下，冲突的时候好像会将另一边的文件先添加过来，然后你可能需要将新来的add、commit，然后就告诉你解决了

如果两边都更改了一个文件的一个地方，打开那个文件，会看见git把文件修改了，它用了>>>>>   ====== <<<<<来区分不同的东西，

你自己手动保留一个或者怎么改一改就好



#### 禁用fast-forward

通常如果可能，git在merge时会使用fast-forward，但是这种方法在删除分支后会丢掉该分支的信息

如果不想，可以禁用，加上一个参数`--no-ff`,这时的merge到当前分支时会给当前分支提交一个commit，你可以写一些东西

代码格式如下e.g

```
git merge --no-ff -m "merge with no-ff" dev
```

实际开发中，通常master只用于发布最新的版本而并不会在上面开发，而dev（develop）分支才是大家开发的分支，但也不会直接在上面开发

每个人会有自己的分支，定期向dev提交即可



### 



其它要学的：working-tree

### `git stash`用于保存工作区

有时候我们不想立即提交，但是又需要切换分支，怎么办呢？

使用`git stash`可以保存当前进度，使工作区看上去是干净的

之后回来，可以使用`git stash list`命令查看stash里面存储的情况

注意，stash相当于一个堆栈

使用`git stash apply`来用stash的最上一个来恢复工作区，但是不会出栈

使用`git stash drop`来从stash中删除最上面的（只是删除，不会改变工作区）

使用`git stash pop`来删除并恢复工作区（即弹出）

即`git stash pop`相当于`git stash apply`+`git stash drop`



使用`git stash apply stash@[5]`来从特定位置恢复工作区



`git cherry-pick <commit>`可以将指定提交从一个分支拷贝到另一个分支，自行了解



#### `git branch -D <name>`强制删除分支



## 远程相关

`git push origin master`将master分支推送到远程仓库

`git pull`从远程拉取（不写参数为所有分支）合并到本地



如果push不成功，可能是冲突了，你需要先pull别人的修改，在本地解决冲突，再push



### rebase相关：将提交历史整理成直线

自己看





至此，基于廖雪峰的教程，我已经将git的基本操作学完了；其它很多可能用不太上，用的时候再学吧
