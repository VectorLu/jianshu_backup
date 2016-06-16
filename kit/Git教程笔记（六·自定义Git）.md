####命令行概览
```
git add -f <filename> #强行添加文件

git check-ignore #检查.gitignore规则

git config --global alias.<> <> #配置别名
```

***

####忽略特殊文件
有些时候，你必须把某些文件放到Git工作目录中，但又不能提交它们，比如保存了数据库密码的配置文件啦。
在Git工作区的根目录下创建一个特殊的.gitignore文件，然后把要忽略的文件名填进去，Git就会自动忽略这些文件。
不需要从头写.gitignore文件，GitHub已经为我们准备了各种配置文件，只需要组合一下就可以使用了。所有配置文件可以直接在线浏览：[https://github.com/github/gitignore](https://github.com/github/gitignore)

忽略文件的原则是：
1. 忽略操作系统自动生成的文件，比如缩略图等；
2. 忽略编译生成的中间文件、可执行文件等，也就是如果一个文件是通过另一个文件自动生成的，那自动生成的文件就没必要放进版本库，比如Java编译产生的.class文件；
3. 忽略你自己的带有敏感信息的配置文件，比如存放口令的配置文件。

举个例子：

假设你在Windows下进行Python开发，Windows会自动在有图片的目录下生成隐藏的缩略图文件，如果有自定义目录，目录下就会有Desktop.ini文件，因此你需要忽略Windows自动生成的垃圾文件：
```
# Windows:
Thumbs.db
ehthumbs.db
Desktop.ini
```
然后，继续忽略Python编译产生的.pyc、.pyo、dist等文件或目录：
```
# Python:
*.py[cod]
*.so
*.egg
*.egg-info
dist
build
```
加上你自己定义的文件，最终得到一个完整的.gitignore文件，内容如下：
```
# Windows:
Thumbs.db
ehthumbs.db
Desktop.ini

# Python:
*.py[cod]
*.so
*.egg
*.egg-info
dist
build

# My configurations:
db.ini
deploy_key_rsa
```
最后一步就是把.gitignore也提交到Git，就完成了！当然检验.gitignore的标准是git status命令是不是说working directory clean。

使用Windows的童鞋注意了，如果你在资源管理器里新建一个.gitignore文件，它会非常弱智地提示你必须输入文件名，但是在文本编辑器里“保存”或者“另存为”就可以把文件保存为.gitignore了。

有些时候，你想添加一个文件到Git，但发现添加不了，原因是这个文件被.gitignore忽略了：如果你确实想添加该文件，可以用-f强制添加到Git：
```
git add -f App.class
```
或者你发现，可能是.gitignore写得有问题，需要找出来到底哪个规则写错了，可以用git check-ignore命令检查：
```
$ git check-ignore -v App.class
.gitignore:3:*.class App.class
```

Git会告诉我们，.gitignore的第3行规则忽略了该文件，于是我们就可以知道应该修订哪个规则。

***
####配置别名
我们只需要敲一行命令，告诉Git，以后st就表示status：
```
git config --global alias.st status
```
当然还有别的命令可以简写，很多人都用co表示checkout，ci表示commit，br表示branch。以后提交就可以简写成：
```
$ git ci -m "bala bala bala..."
```

```--global``` 参数是全局参数，也就是这些命令在这台电脑的所有Git仓库下都有用

命令```git reset HEAD file```可以把暂存区的修改撤销掉（unstage），重新放回工作区。既然是一个unstage操作，就可以配置一个unstage别名：
``` 
git config --global alias.unstage 'reset HEAD'
```
配置一个git last，让其显示最后一次提交信息：
```
git config --global alias.last 'log -1'
```
这样，用git last就能显示最近一次的提交：

甚至还有人丧心病狂地把lg配置成了：
```
git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"
```
我觉得效果很好。

***
####配置文件
配置Git的时候，加上```--global```是针对当前用户起作用的，如果不加，那只针对当前的仓库起作用。

配置文件放哪了？每个仓库的Git配置文件都放在```.git/config```文件中。
而当前用户的Git配置文件放在用户主目录下的一个隐藏文件```.gitconfig```
中（必须在用户主目录下才能看到）。

后面配置git服务器很少用到，就不赘述了，要用的时候再查也很方便。

参考来源：[廖雪峰的git教程](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)
