**主要内容：**
1. 添加远程仓库
2. 从远程仓库克隆到本地

**命令概览（括号里是需要替换的内容）**
```
ssh-keygen -t rsa -C ("youremail@example.com")
#创建ssh key

git remote add origin (repository address on github)
#关联远程仓库

git remote -help
 #用来查看帮助

git push -u origin master
#第一次推送本地内容到远程仓库要加 -u参数

git push origin master
#以后就可以直接推送内容了

git clone (repository address on github)
#将repository克隆到本地
```
***
####添加远程仓库
> SSH 为建立在应用层和传输层基础上的安全协议。SSH 是目前较可靠，专为[远程登录](http://baike.baidu.com/view/59099.htm)会话和其他网络服务提供安全性的协议。利用 SSH 协议可以有效防止远程管理过程中的信息泄露问题。

由于你的本地Git仓库和GitHub仓库之间的传输是通过SSH加密的，所以，需要一点设置：

第1步：创建SSH Key。在用户主目录下，看看有没有.ssh目录，如果有，再看看这个目录下有没有id_rsa和id_rsa.pub这两个文件，如果已经有了，可直接跳到下一步。如果没有，打开Shell（Windows下打开Git Bash），创建SSH Key：
```$ ssh-keygen -t rsa -C "youremail@example.com"```
需要把邮件地址换成你自己的邮件地址，然后一路回车，使用默认值即可。如果一切顺利的话，可以在用户主目录里找到.ssh
目录，里面有id_rsa和id_rsa.pub两个文件，这两个就是SSH Key的秘钥对，id_rsa是私钥，不能泄露出去，id_rsa.pub
是公钥，可以放心地告诉任何人。

**教程中没有说明怎样打开id_rsa.pub文件，个人的方法是用文本编辑器打开，比如atom（如果安装了atom），vi等**

![进入.ssh文件夹](http://upload-images.jianshu.io/upload_images/1213502-016f722d6347c428.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![用atom打开id_rsa.pub](http://upload-images.jianshu.io/upload_images/1213502-366f1a75f62e82ee.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
**也可以用这样方法打开id_rsa文件**
如果没有安装atom，可以用vi打开
```$ vi id_rsa```
![用vi打开](http://upload-images.jianshu.io/upload_images/1213502-f91109674c831ec1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
第2步：登陆GitHub，打开“Account settings”，“SSH Keys”页面：
然后，点“Add SSH Key”，填上任意Title，在Key文本框里粘贴id_rsa.pub文件的内容：
![github-addkey-1](http://upload-images.jianshu.io/upload_images/1213502-27e90f7af2fb6124?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
点“Add Key”，你就应该看到已经添加的Key：
![github-addkey-2](http://upload-images.jianshu.io/upload_images/1213502-aea5c2f6d6d1a09b?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

####在GitHub上免费托管的Git仓库，任何人都可以看到喔（但只有你自己才能改）。所以，不要把敏感信息放进去。####

***
####添加远程库
首先，登陆GitHub，然后，在右上角找到“Create a new repo”按钮，创建一个新的仓库，目前，在GitHub上的这个learngit仓库还是空的，GitHub告诉我们，可以从这个仓库克隆出新的仓库，也可以把一个已有的本地仓库与之关联，然后，把本地仓库的内容推送到GitHub仓库。

**在添加新的repository时，只填repository的名字，其他的保留默认设置确认就会出现下图的界面。其实不用非要把那个命令行背下来或者照着教程敲。看这个界面，github给了提示的命令行，复制一下就好了。其实如果在图形界面（github Desktop里更加简单，下次详细写写那个怎么用）。**

![添加新的repository后，复制提示的命令行就好了](http://upload-images.jianshu.io/upload_images/1213502-36d0f4629c26a751.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
将复制的命令行贴到终端就好了。
> 1. 当你第一次使用Git的clone或者push命令连接GitHub时，会得到一个警告,这是因为Git使用SSH连接，而SSH连接在第一次验证GitHub服务器的Key时，需要你确认GitHub的Key的指纹信息是否真的来自GitHub的服务器，输入yes回车即可。
Git会输出一个警告，告诉你已经把GitHub的Key添加到本机的一个信任列表里了。
2. 将本地GIT版本库PUSH到一个GITHUB上一个空的版本库时可能会出现如下错误error:src refspec master does not match any原因: 本地版本库为空, 空目录不能提交 (只进行了init, 没有add和commit)
如果出现这种情况，那么就是你在工作去的文件还从来没有放到本地的仓库中。git add 和 git commit一下就好了


![push到github上](http://upload-images.jianshu.io/upload_images/1213502-ac962b6b2682a082.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![push到github上](http://upload-images.jianshu.io/upload_images/1213502-2e13048182eff39d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![成功了，开酒庆祝](http://upload-images.jianshu.io/upload_images/1213502-ba25fa765db8f91e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
之后，在本地提交，就只需要
``` 
git push origin master
```
推送最新的修改就好了

***
####从远程库克隆到本地
可以用教程中的类似```git clone git@github.com:michaelliao/gitskills.git```的命令，但是我不理解这种表达（好像是基于ssh协议，不过现在有https了？），要是有人知道请留言告诉我orz，我比较习惯用的方式是类似于```
git clone https://github.com/VectorLu/HelloC
```
即git clone后面直接加网址的方法，这种方法还不需要你有账号，想clone任何public的repository都是可以的。其实打也很容易，就是https://github.com/(Username)/(RepositoryName)，
括号里是需要替换的内容。教程上好像说https会比较慢，而且每次都需要输入口令，可能是private会吧，这个我倒是不是很清楚。

**参考来源：**
[廖雪峰的git教程](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)
