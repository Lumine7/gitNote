## 这里用于记录使用git和github出现的问题和解决方法

#### 1.使用https在push、pull和clone经常失败

强烈建议使用ssh协议。我之前已经添加过ssh key，但是在关联远程仓库时用来https的地址，如果要使用ssh，应当将地址改为如下形式

`https://github.com/Lumine7/DBNote`--------------->>>>>` git@github.com:Lumine7/DBNote.git` 

使用ssh进行推送或者拉取时，正常情况下每次要输入密码

#### 2.git pull一个分支

#### fatal: refusing to merge unrelated histories

原因是两个分支的里是没有取得关联，我这次发生于new一个远程仓库后又把已有东西的本地仓库推过去的之后再拉取的时候；说白了就是在本地历史里面找不到远程那个branch对应的指针

加一个参数`--allow-unrelated-histories`，可以强制pull过来并合并，详见https://blog.csdn.net/qq_39400546/article/details/100150320、https://blog.csdn.net/wd2014610/article/details/80854807