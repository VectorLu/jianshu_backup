###命令行及内容概览
```
git branch #查看分支
 
git branch <name>  #创建分支

git checkout <name> #切换分支

git checkout -b <new branch name> #创建加切换分支

git merge <name>  #合并某分支到当前分支

git branch -d <name> #删除分支

git log --graph #查看分支合并的情况

git log --graph --pretty=oneline --abbrev-commit #用一行呈现log信息

#如何利用分支进行合作开发

git stash #隐藏工作区和暂存区的内容

git stash apply #将隐藏的内容恢复，但不从stash list中删除

git stash drop #删除stash list中的内容

git stash pop #恢复的同时也删除

git remote #查看远程库的信息

git remote -v #查看更详细的信息

git push origin <remote branch name> #推送到远程仓库的指定分支

git branch --set-upstream <local branch name> origin/<remote branch name>
 #建立local branch和remote branch的链接

git pull 
#抓取分支到本地，尝试合并，若有conflict，解决冲突，在本地提交后再push
```
***
####创建与合并分支
每次提交，Git都把它们串成一条时间线，这条时间线就是一个分支。截止到目前，只有一条时间线，在Git里，这个分支叫主分支，即master分支。HEAD
严格来说不是指向提交，而是指向master，master才是指向提交的，所以，HEAD指向的就是当前分支。
一开始的时候，master分支是一条线，Git用master指向最新的提交，再用HEAD指向master，就能确定当前分支，以及当前分支的提交点：
![git-br-initial](http://upload-images.jianshu.io/upload_images/1213502-4d94e18191dcfd1a?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
每次提交，master分支都会向前移动一步，这样，随着你不断提交，master分支的线也越来越长

当我们创建新的分支，例如dev时，Git新建了一个指针叫dev，指向master相同的提交，再把HEAD指向dev，就表示当前分支在dev上：
![git-br-create](http://upload-images.jianshu.io/upload_images/1213502-34e0db78d30eced7?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
Git创建一个分支很快，因为除了增加一个dev指针，改改HEAD的指向，工作区的文件都没有任何变化！不过，从现在开始，对工作区的修改和提交就是针对dev分支了，比如新提交一次后，dev指针往前移动一步，而master指针不变：
![git-br-dev-fd](http://upload-images.jianshu.io/upload_images/1213502-34c1f903fb94ad68?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
假如我们在dev上的工作完成了，就可以把dev合并到master上。Git怎么合并呢？最简单的方法，就是直接把master指向dev的当前提交，就完成了合并：![git-br-ff-merge](http://upload-images.jianshu.io/upload_images/1213502-c5553275c75693a2?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)所以Git合并分支也很快！就改改指针，工作区内容也不变！合并完分支后，甚至可以删除dev分支。删除dev分支就是把dev指针给删掉，删掉后，我们就剩下了一条master分支：
![git-br-rm](http://upload-images.jianshu.io/upload_images/1213502-d3d7dd0fe7b302e6?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

***
**练习**
```
git checkout -b dev
```
加上-b参数表示创建并切换，相当于：
```
git branch dev
git checkout dev
```
然后，用```git branch```命令查看当前分支

![查看当前分支](http://upload-images.jianshu.io/upload_images/1213502-d0b48bc5081abaea.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
当前分支前会标```*```号。
同样的方法可以在这个分支上进行add和commit。(要推送到github上是要注意将master换成新的branch名)

![比如这里应该换成dev](http://upload-images.jianshu.io/upload_images/1213502-fa457986c5c3225b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
在新创建的分支上的工作完成（add和commit）之后，就可以切回到master。

![切换到新创建的分支test](http://upload-images.jianshu.io/upload_images/1213502-aa1ee6575cfcbca4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
比如这里可以将```test```分支的工作成果合并到```master```分支上。

![合并工作](http://upload-images.jianshu.io/upload_images/1213502-7d9c0cb97655e0b3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

test分支上的工作完成之后，切换到其他的分支。用```git branch -d branchName```删除分支。

***
####解决冲突
当两个分支分别有新的提交
现在，master分支和feature1分支各自都分别有新的提交，变成了这样：
![分别有新的提交](http://upload-images.jianshu.io/upload_images/1213502-01341ddc72ebd1ca?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
这种情况下，就不能快速合并，只能将各自的修改尝试合并起来，但是这样的合并可能会有冲突。可以用```git status```查看发生冲突的文件。
![发生冲突](http://upload-images.jianshu.io/upload_images/1213502-760b153ba9249cfc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![查看发生冲突的文件](http://upload-images.jianshu.io/upload_images/1213502-1f759a27efb410ca.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

修改发生冲突的文件，add、commit，master分支和新分支的关系就变成：
![git-br-conflict-merged](http://upload-images.jianshu.io/upload_images/1213502-798dec9b5d6a78cf?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
用带参数的git log也可以看到分支的合并情况：
```git log --graph --pretty=oneline --abbrev-commit```

![分支的合并情况](http://upload-images.jianshu.io/upload_images/1213502-6968d121c4e3c9fb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

当Git无法自动合并分支时，就必须首先解决冲突。解决冲突后，再提交，合并完成。

用```git log --graph```命令可以看到分支合并图。

***
###分支管理策略
通常合并分支的时候，git会默认使用```fast forward```模式。但是，这种模式，在删除分支后，会丢失分支信息。如果强制禁止这种模式，Git就会在merge时生成一个新的commit，这样，从分支历史上就可以看出分支信息。
加上```--no-ff```参数，表示禁用```fast forward```：
下面我们实战一下--no-ff方式的git merge：
首先，仍然创建并切换dev分支：
```
$ git checkout -b dev
Switched to a new branch 'dev'
```

修改readme.txt文件，并提交一个新的commit：
```
$ git add readme.txt 
$ git commit -m "add merge"
[dev 6224937] add merge 
1 file changed, 1 insertion(+)
```
现在，我们切换回master：
```
$ git checkout master
Switched to branch 'master'
```
准备合并dev分支，请注意--no-ff参数，表示禁用Fast forward：
```
$ git merge --no-ff -m "merge with no-ff" dev
Merge made by the 'recursive' strategy.
 readme.txt | 1 + 
1 file changed, 1 insertion(+)
```
因为本次合并要创建一个新的commit，所以加上-m参数，把commit描述写进去。

合并后，我们用git log看看分支历史：
```
$ git log --graph --pretty=oneline --abbrev-commit
* 7825a50 merge with no-ff
|\
| * 6224937 add merge
|/* 59bc1cb conflict fixed
```
可以看到，不使用Fast forward
模式，merge后就像这样：
![git-no-ff-mode](http://upload-images.jianshu.io/upload_images/1213502-5de480fc36aecccd?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
***
####分支策略
在实际开发中，我们应该按照几个基本原则进行分支管理：
首先，master分支应该是非常稳定的，也就是仅用来发布新版本，平时不能在上面干活；
那在哪干活呢？干活都在dev分支上，也就是说，dev分支是不稳定的，到某个时候，比如1.0版本发布时，再把dev分支合并到master上，在master分支发布1.0版本：
你和你的小伙伴们每个人都在dev分支上干活，每个人都有自己的分支，时不时地往dev分支上合并就可以了。
所以，团队合作的分支看起来就像这样：
![git-br-policy](http://upload-images.jianshu.io/upload_images/1213502-5cafd4fde0a29a71?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
小结
Git分支十分强大，在团队开发中应该充分应用。
合并分支时，加上--no-ff参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而fast forward合并就看不出来曾经做过合并。

***
####Bug分支
每个bug都可以通过一个新的临时分支来修复，修复后，合并分支，然后将临时分支删除。
当在dev上干活时，突然接到要修复代号101的bug，想新建一个分支issue-101来修复。可是手头的工作还没完成,dev上的工作还没有提交。可是现在还没有办法提交手头的工作，bug又需要立即修复。
Git提供了一个stash功能，可以把当前工作现场“储藏”起来，等以后恢复现场后继续工作（未add和commit的工作都可以隐藏）：

![有未add和未commit的工作](http://upload-images.jianshu.io/upload_images/1213502-c91bc0a1519e52a0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![stash后工作区就变得干净了](http://upload-images.jianshu.io/upload_images/1213502-2938a8ca51cd5e4e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#####修复bug
1. 首先确定在哪个分支上修复bug，假定要在master上修复，就从master创建临时分支，例如:
![从选定的分支上创建临时分支](http://upload-images.jianshu.io/upload_images/1213502-bec41215ff8f6a2b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
2. 修复bug，提交后切换到master分支，用```git merge --no-ff -m "merge bug fix 101" ```合并分支，然后删除issue-101分支。
3. 回到dev继续干活。刚才被stash的工作用```git stash list```查看。有两种方法恢复：
  1. 用```git stash apply ```恢复，恢复后，stash内容并不删除，需要用```git stash drop```来删除。
  2. 用``` git stash pop```来删除，恢复的同时也删除了里的内容，再用```git stash list ```来查看就没有了。可以多次stash，恢复的时候，先用```git stash list```查看，再用类似于```git stash apply stash@{0}```来恢复到想要的内容。
**看到pop命令就大概明白，stash主要就是将需要放在一边的内容先放在一个堆栈里面啦**

***
####Feature分支
**每添加一个新功能，最好新建一个feature分支（在取名的时候，要让新的分支名能够概括新的功能），在上面开发，完成后，合并，最后，删除该feature分支。**
一切顺利的话，feature分支与bug分支一样，完成后提交到dev并删除分支。但是新功能被取消了，要求删除feature分支。
先切换回dev分支，用```git branch -d feature```命令删除分支，但是，由于feature分支没有merge。会显示失败，根据提示用```git branch -D feature```就可以强行删除了。

![过程示例](http://upload-images.jianshu.io/upload_images/1213502-5c8ce528025dc98e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

***
####多人协作
当从远程仓库把项目clone到本地的时候，git自动把本地的master分支和远程的master分支对应起来了。远程仓库的默认名为origin。

要查看远程库的信息，用
```
git remote
```
或者用查看更详细的信息
```
git remote -v
```
比如：
```
VectorLu:Diary Vector$ git remote
origin
VectorLu:Diary Vector$ git remote -v
origin	https://github.com/VectorLu/Diary.git (fetch)
origin	https://github.com/VectorLu/Diary.git (push)
```
上面显示了可以抓取和推送的origin的地址，如果没有推送的权限，就看不到push的地址。

**推送分支**就是把这个分支上的所有提交推送到远程库。推送时，要指定本地分支。git就会把指定的分支推送到远程仓库对应的指定分支上。比如：
```
git push origin master
```
或者要推送到dev上，就是
```
git push origin dev
```

**哪些分支需要推送，哪些不需要呢？**
1. master分支是主分支，因此要时刻与远程同步；
2. dev分支是开发分支，团队所有成员都需要在上面工作，所以也需要与远程同步；
3. bug分支只用于在本地修复bug，就没必要推到远程了，除非老板要看看你每周到底修复了几个bug；
4. feature分支是否推到远程，取决于你是否和你的小伙伴合作在上面开发。

然而可能会有推送失败的情况出现。
![未设定分支间的连接](http://upload-images.jianshu.io/upload_images/1213502-7097e589aec15b82.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
原因是没有指定本地```dev```分支与远程```origin/dev```分支的链接，根据提示，设置dev和origin/dev的链接：
```
$ git branch --set-upstream dev origin/dev
Branch dev set up to track remote branch dev from origin.
```
![合并有冲突](http://upload-images.jianshu.io/upload_images/1213502-376fc5b46de95a5c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
修改文件以解决冲突，别忘了add和commit之后，再push。

![解决冲突，add和commit之后再push](http://upload-images.jianshu.io/upload_images/1213502-8ec7b22d7092c760.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

多人协作的工作模式通常是这样：
1. 首先，可以试图用```git push origin branch-name```推送自己的修改；
2. 如果推送失败，则因为远程分支比你的本地更新，需要先用```git pull```试图合并；
3. 如果合并有冲突，则解决冲突，并在本地提交；
4. 没有冲突或者解决掉冲突后，再用```git push origin branch-name```推送就能成功!
**如果提示"no tracking information"，则说明本地分支与远程分支之间没有创建链接关系，用命令
```
git branch --set-upstream <local branch name> origin/<remote branch name>

**小结:**
1. 查看远程库信息，使用git remote -v；
2. 本地新建的分支如果不推送到远程，对其他人就是不可见的；
3. 从本地推送分支，使用git push origin branch-name，如果推送失败，先用git pull抓取远程的新提交；
4. 在本地创建和远程分支对应的分支，使用git checkout -b branch-name origin/branch-name，本地和远程分支的名称最好一致；
5. 建立本地分支和远程分支的关联，使用git branch --set-upstream branch-name origin/branch-name；
6. 从远程抓取分支，使用git pull，如果有冲突，要先处理冲突，并且add和commit修改再push。
