git——分布式版本控制软件
**注意：<>内为可替换内容，敲命令行时不需要敲<>**


**主要内容**：创建git repository，将文件添加到版本库，git的状态和文件的变化，版本&回退。
**命令概览**：
```
$ git init #使当前目录变成可以管理的版本仓库（git repository)
$ git add filename #将文件添加到版本仓库
$ git commit -m "description" #把文件提交到仓库
$ git status #查看repository的状态
$ git diff #查看修改了哪些内容
$ git log #查看提交日志
$ git log --pretty=oneline #简洁地显示提交日志
$ git reset --hard HEAD~<3> #回退到某个版本，比如这里回退到第前3个版本
$ git reset --hard <commit ID> #回退到特定ID的版本
$ git reflog #记录了每个命令，可以用来查看每个操作的编号

```
***
####创建git repository
1. 找合适的地方，创建一个空目录，比如在```/Users/Vector/github```路径下，创建并进入，一般不要用中文的目录名，容易出现问题
```
$ mkdir learnGit
$ cd learnGit
$ pwd
/Users/Vector/github/learnGit
```
2. ```git init``` 命令将这个目录变成可以管理的版本仓库（git repository）
```
VectorLu:learnGit Vector$ git init
Initialized empty Git repository in /Users/Vector/github/learnGit/.git/
```
可以发现当前目录下多了一个```.git```目录，这个目录就是git用来跟踪管理版本仓库的，不要去手动修改。```ls```命令不会显示带.的目录，用```ls -a```就可以显示了
```
VectorLu:learnGit Vector$ ls
VectorLu:learnGit Vector$ ls -a
.	..	.git
```
***
####将文件添加到版本库
> **注意**
首先这里再明确一下，所有的版本控制系统，其实只能跟踪文本文件的改动，比如TXT文件，网页，所有的程序代码等等，Git也不例外。版本控制系统可以告诉你每次的改动，比如在第5行加了一个单词“Linux”，在第8行删了一个单词“Windows”。而图片、视频这些二进制文件，虽然也能由版本控制系统管理，但没法跟踪文件的变化，只能把二进制文件每次改动串起来，也就是只知道图片从100KB改成了120KB，但到底改了啥，版本控制系统不知道，也没法知道。
不幸的是，Microsoft的Word格式是二进制格式，因此，版本控制系统是没法跟踪Word文件的改动的，前面我们举的例子只是为了演示，如果要真正使用版本控制系统，就要以纯文本方式编写文件。
因为文本是有编码的，比如中文有常用的GBK编码，日文有Shift_JIS编码，如果没有历史遗留问题，强烈建议使用标准的UTF-8编码，所有语言使用同一种编码，既没有冲突，又被所有平台所支持。
使用Windows的童鞋要特别注意：
千万不要使用Windows自带的**记事本**编辑任何文本文件。原因是Microsoft开发记事本的团队使用了一个非常弱智的行为来保存UTF-8编码的文件，他们自作聪明地在每个文件开头添加了0xefbbbf（十六进制）的字符，你会遇到很多不可思议的问题，比如，网页第一行可能会显示一个“?”，明明正确的程序一编译就报语法错误，等等，都是由记事本的弱智行为带来的。

**个人很推荐github官方出品的atom文本编辑器，简直好看漂亮好用，当然老牌的sublime也不错，但是要付费（虽然无限期试用），但是atom是开源免费的，而且跟git配合天衣无缝。**
![用Atom写文本文件](http://upload-images.jianshu.io/upload_images/1213502-e6582cd268d85f54.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
**如图中所示，刚刚创建未提交的文件名是绿色的，已经提交过的文件但是经过修改后，没有提交修改后的文件名是黄色的“。要注意Atom在编辑文件时，点文件编辑窗口的关闭键，如果当前的修改没有保存，会弹出提示，但是如果点的是Atom的关闭键。。。它就直接退出了。。。**不会提示没有保存，这是个bug，改天看它更新版本的时候会不会修正把，或者什么时候去官网说一下。
一定要放到learngit目录下（子目录也行），因为这是一个Git仓库。
把一个文件放到Git仓库只需要两步：
1. 把文件添加到仓库，没有任何消息就是表示添加成功
```
$ git add README.md
```
2. 把文件提交到仓库
```
VectorLu:learnGit Vector$ git commit -m "wrote a readme file"
[master (root-commit) 3be5479] wrote a readme file
 1 file changed, 2 insertions(+)
 create mode 100644 README.md
```

-m之后是本次提交的说明，最好能够简要概括这次提交了什么内容，或者做了什么修改。为什么Git添加文件需要add，commit一共两步呢？因为commit可以一次提交很多文件，所以你可以多次add不同的文件，比如：
```
$ git add file1.txt
$ git add file2.txt 
$ git add file3.txt
$ git commit -m "add 3 files."
```
***
####git的状态和文件的变化
 ```$ git status```查看repository的状态，```$ git diff```查看修改了哪些内容
***
####版本&回退
```$ git log```由近到远显示提交日志，可以加上参数``` --pretty=oneline```，使显示的信息更加简洁
```
VectorLu:learnGit Vector$ git log --pretty=oneline
8512e8bcdda603a6184128be151174f962221dd2 append GPL
f53fa0eae5a3ad9689473f01ef9b38cab366cfd5 distributed
3be547948169bfa0fcff7d4755949859628f3312 wrote a readme file
```

![桌面版的github上显示详细的信息](http://upload-images.jianshu.io/upload_images/1213502-4f241982718c6c21.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
**```HEAD```表示当前版本，```HEAD~100```第前100个版本**。```git reset --hard HEAD```回退到上一个版本。
```
VectorLu:SE Vector$ git reset --hard HEAD~3
HEAD is now at 547e18f 迭代计算梯度
```
回退到以前的版本A之后，用git log命令就找不到A之后的版本了，如果想回到之后的版本，需要```git reset --hard commitID```，但是如果不记得commitID（谁没事会记这个）,可以用```$ git reflog```，它记录了每一个命令
```
VectorLu:SE Vector$ git reflog
547e18f HEAD@{0}: reset: moving to HEAD~3
da1401a HEAD@{1}: commit: 修改了精度，使之更加严谨
697c680 HEAD@{2}: commit: 精度有问题，重新编写函数测算是否重合
5afa969 HEAD@{3}: commit: 测算是否有圆重合
547e18f HEAD@{4}: commit: 迭代计算梯度
9c80a1b HEAD@{5}: commit (initial): start code the function

VectorLu:SE Vector$ git reset --hard 547e18f
HEAD is now at 547e18f 迭代计算梯度
```
>1.  HEAD指向的版本就是当前版本，因此，Git允许我们在版本的历史之间穿梭，使用命令git reset --hard commit_id。
2. 穿梭前，用git log可以查看提交历史，以便确定要回退到哪个版本。
3. 要重返未来，用git reflog查看命令历史，以便确定要回到未来的哪个版本。

参考来源：[廖雪峰的git教程](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)
