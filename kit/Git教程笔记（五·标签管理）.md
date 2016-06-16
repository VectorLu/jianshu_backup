发布一个版本时，我们通常先在版本库中打一个标签，这样，就唯一确定了打标签时刻的版本。将来无论什么时候，取某个标签的版本，就是把那个打标签的时刻的历史版本取出来。其实它就是指向某个commit的指针（跟分支很像对不对？但是分支可以移动，标签不能移动。

**内容和命令行概览：**
```
git tag #查看所有标签

git tag <tag name> #加标签

git log --pretty=oneline --abbrev-commit #找到历史提交的commit ID

git tag <tag name> <commit ID> #对某个commit打标签

git tag -a v0.1 -m "version 0.1 released" 3628164
#还可以创建带有说明的标签，用-a指定标签名，-m指定说明文字

git tag -d <tag name> #删除某个标签

git push origin <tag name> #将某个标签推送到远程

git push origin --tags #一次性推送全部的标签到远程

git push origin :refs/tags/<tag name> #在远程删除
```

***
####创建标签
1. 切换到要打标签的分支上
2. ```git tag <tag name>```加标签，然后可以用```git tag```查看所有标签，如下：
```
VectorLu:Diary Vector$ git branch
 * dev
  master
VectorLu:Diary Vector$ git checkout master
Switched to branch 'master'
Your branch is up-to-date with 'origin/master'.
VectorLu:Diary Vector$ git tag v1.1
VectorLu:Diary Vector$ git tag
v1.0
v1.1
```

**默认标签打在最新提交的commit上**。如果忘了打标签，比如，现在已经是周五了，但应该在周一打的标签没有打，怎么办？

方法是找到历史提交的commit id，然后打上就可以了，比如：
```
$ git log --pretty=oneline --abbrev-commit
6a5819e merged bug fix 101
cc17032 fix bug 101
7825a50 merge with no-ff
6224937 add merge
59bc1cb conflict fixed
400b400 & simple
75a857c AND simple
fec145a branch testd
17efd8 remove test.txt
...
```
如果要对add merge 这次commit打标签，它对应的commit ID是6224937，所以
```
git tag v0.9 6224937
```
再用```git tag```查看tag信息
```
$ git tag
v0.9
v1.0
```
**tag不是按时间顺序，而是按字母排序。**可以用```git show <tagname>```来查看标签信息

![查看标签信息](http://upload-images.jianshu.io/upload_images/1213502-2ccd07019f4efe8d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
**还可以创建带有说明的标签**用-a指定标签名，-m指定说明文字
```
git tag -a v0.1 -m "version 0.1 released" 3628164
```
暂时对gpg不了解，跳过用-s私钥签名一个标签

小结
1. 命令git tag <name>用于新建一个标签，默认为HEAD，也可以指定一个commit id；
2. git tag -a <tagname> -m "blablabla..."可以指定标签信息；
3. git tag -s <tagname> -m "blablabla..."可以用PGP签名标签；
4. 命令git tag可以查看所有标签。

***
####标签操作除
创建的标签不会自动推送到远程，故打错的标签可以只用在本地删除。例如：
```
git tag -d v1.0
```
推送某个标签到远程
```
git push origin v1.0
```
一次性推送全部的标签到远程
```
git push origin --tags
```
如果标签已经推送到远程，需要删除就相较麻烦一些。先在本地删除tag，然后，在远程删除，命令也是push，格式如下：
```
git push origin :refs/tags/v0.9
```

**小结：**
1. 命令git push origin <tagname>可以推送一个本地标签；
2. 命令git push origin --tags可以推送全部未推送过的本地标签；
3. 命令git tag -d <tagname>可以删除一个本地标签；
4. 命令git push origin :refs/tags/<tagname>可以删除一个远程标签
