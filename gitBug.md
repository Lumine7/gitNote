## 这里用于记录使用git和github出现的问题和解决方法

#### 1.使用https在push、pull和clone经常失败

可能出现的保存有443端口超时、connection was reset、等一大堆不同的花式报错，对于github网站，改用ssh会解决至少90%这样的问题

强烈建议使用ssh协议。我之前已经添加过ssh key，但是在关联远程仓库时用来https的地址，如果要使用ssh，应当将地址改为如下形式

`https://github.com/Lumine7/DBNote`--------------->>>>>` git@github.com:Lumine7/DBNote.git` 

使用ssh进行推送或者拉取时，正常情况下每次要输入密码

#### 2.git pull一个分支

#### fatal: refusing to merge unrelated histories

原因是两个分支的里是没有取得关联，我这次发生于new一个远程仓库后又把已有东西的本地仓库推过去的之后再拉取的时候；说白了就是在本地历史里面找不到远程那个branch对应的指针

加一个参数`--allow-unrelated-histories`，可以强制pull过来并合并，详见https://blog.csdn.net/qq_39400546/article/details/100150320、https://blog.csdn.net/wd2014610/article/details/80854807

#### 3. git push报

> ! [rejected]        master -> master (non-fast-forward)
> error: failed to push some refs to 'github.com:Lumine7/gitNote.git'
> hint: Updates were rejected because the tip of your current branch is behind
> hint: its remote counterpart. Integrate the remote changes (e.g.
> hint: 'git pull ...') before pushing again.

也出现在建立一个远程repo后将本地已存在的仓库推过去的时候

原因是远程库和本地库不一致，需要先pull一下；

另外普通的pull似乎不起作用，应该在pull后面加上`--rebase`参数

菜鸡的rebase那块没学过，不太懂是什么原理，只好按照网上的照做了https://blog.csdn.net/qq_30152625/article/details/90404727



##### 4.关于默认分支

github在某个版本之后默认分支变成了main，不过本地的git默认的分支还是master，建议将github那里也改为master