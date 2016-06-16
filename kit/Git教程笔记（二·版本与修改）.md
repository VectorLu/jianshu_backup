**主要内容：**
1. 工作区和暂存区
2. 管理修改

**命令概览**
```
$ git checkout -- filename
$ git reset filename
$ git rm filename
```
***
####工作区和暂存区
Git和其他版本控制系统如SVN的一个不同之处就是有暂存区的概念。
先来看名词解释。
**工作区（Working Directory）**
就是你在电脑里能看到的目录，比如我的learngit
文件夹就是一个工作区：
![working-dir](http://upload-images.jianshu.io/upload_images/1213502-2a596a34225058fe?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
**版本库（Repository）**
工作区有一个隐藏目录.git，这个不算工作区，而是Git的版本库。
Git的版本库其中最重要的就是称为stage（或者叫index）的暂存区，还有Git为我们自动创建的第一个分支master，以及指向master的一个指针叫HEAD。
![git-repo](http://upload-images.jianshu.io/upload_images/1213502-5ba88810f237ff26?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
我们把文件往Git版本库里添加的时候，是分两步执行的：

第一步是用git add把文件添加进去，实际上就是把文件修改添加到暂存区；

第二步是用git commit提交更改，实际上就是把暂存区的所有内容提交到当前分支。

####管理修改
git跟踪的是修改而非文件，每次修改，如果不add到暂存区，那就不会加入到commit中。
```$ git checkout -- file```可以丢弃工作区的修改。命令中的--
很重要，没有--，就变成了“切换到另一个分支”的命令。
命令```git checkout -- README.md```意思就是，把README.md文件在工作区的修改全部撤销，这里有两种情况：
1. readme.txt自修改后还没有被放到暂存区，现在，撤销修改就回和版本库一模一样的状态；
2. 是readme.txt已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。
总之，就是让这个文件回到最近一次git commit或git add时的状态。

用命令```git reset HEAD file```可以把暂存区的修改撤销掉（unstage），重新放回工作区。```git reset```命令既可以回退版本，也可以把暂存区的修改回退到工作区。当我们用HEAD时，表示最新的版本。
场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令git checkout -- file
。
场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令git reset HEAD file
，就回到了场景1，第二步按场景1操作。
场景3：已经提交了不合适的修改到版本库时，想要撤销本次提交，参考[版本回退](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/0013744142037508cf42e51debf49668810645e02887691000)一节，不过前提是没有推送到远程库。

####删除文件
```$ git checkout```
**其实是用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以“一键还原”。**
一般情况下，你通常直接在文件管理器中把没用的文件删了，或者用rm命令删了
1. 是确实要从版本库中删除该文件，那就用命令```$ git rm filename```删掉，并且```$ git commit```
比如：
```
$ git rm test.txt
rm 'test.txt'
$ git commit -m "remove test.txt"
[master d17efd8] remove test.txt 
1 file changed, 1 deletion(-) 
delete mode 100644 test.txt
```
2. 另一种情况是删错了，因为版本库里还有呢，所以可以很轻松地把误删的文件恢复到最新版本：
```$ git checkout -- test.txt```

***
**参考来源：**
[廖雪峰的git教程](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)
