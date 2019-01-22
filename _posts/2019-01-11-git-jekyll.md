---
layout: post
title:  "使用Github Pages建立本博客"
date:   2019-01-11
categories: git jekyll
---
## 安装Git
参考[廖雪峰Git教程](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)，  
下载安装[Git](https://git-scm.com/downloads)，按默认选项安装。之后打开“Git Bash”，在命令行输入：
    
    $ git config --global user.name "Your name"
    $ git config --global user.email "email@example.com"
注册GitHub账号。由于本地Git仓库和GitHub仓库之间的传输是通过SSH加密的，所以还需要一点设置：  
1. 创建SSH Key。

    ssh-keygen -t rsa -C "youremail@example.com"
一路回车，使用默认值即可。然后在C盘用户主目录下找到`.ssh`目录，里边有`id_rsa`和`id_rsa.pub`两个文件，这两个就是SSH Key的密钥对，`id_rsa`是私钥，不能泄露出去，`id_rsa.pub`是公钥，可以放心地告诉任何人。  
2. 登陆GitHub，在右上角个人头像处找到“Settings”->“SSH and GPG keys”，点击“New SSH key”,填上Title，在Key文本框里粘贴`id_rsa.pub`文件的内容，点击“Add Key”，就能看到已经添加的Key。  
然后可以测试一下ssh（`git@github.com`不要修改)：

    $ ssh -T git@github.com
后边会有一个（yes/no）选项，输入yes回车就好，之后就会看到一个包括sucessfully的回应。  
## 使用GitHub Pages建立博客
与GitHub建立好链接之后，就可以方便的使用它提供的Pages服务，GitHub Pages分两种，一种是你的GitHub用户名建立的`username.github.io`这样的用户&组织页（站），另一种是依附项目的Pages即对于某一个`repository`，你可以在其中创建一个小网站，向人们展示你的项目。  
想建立个人博客用的是第一种`User & Organization Pages`，形如`username.github.io`这样的可访问站，每个用户名下面只能建立一个。  
我们在GitHub上创建一个`new repository`，命名为`username.github.io`。
## 购买、绑定域名
本来一开始在腾讯云购买了一个域名，而在国内购买域名需要备案，但是备案又需要购买腾讯云服务器，构建这个个人博客不需要购买服务器，所以我就去[Godaddy](https://sg.godaddy.com/zh)买了一个新域名，过了几天听说其他域名网站会赠送一个个人信息加密服务，而Godaddy不赠送，所以准备明年换一家。  
购买域名后选择[DNSPod](https://www.dnspod.cn/)服务：  
1. 添加域名
2. 在DNSPod自己的域名下添加一条记录，主机记录按自己域名格式选择，会有提示，记录类型选择A记录，线路类型默认，记录值需要在“cmd”或者“Git Bash”上输入命令：

        ping github.io
    剩余的就默认就可以。（这个记录值，也就是地址，会有变动，[这里](https://help.github.com/articles/troubleshooting-custom-domains/)查看，我的A记录绑定的地址为185.199.109.153，但是我现在输入上边的命令出来的地址为185.199.110.153，但是域名访问没有问题，应该是因为GitHub总共提供了四个地址：
    + 185.199.108.153
    + 185.199.109.153
    + 185.199.110.153
    + 185.199.111.153）
3. 在域名注册商处修改DNS服务：去[Godaddy](https://sg.godaddy.com/zh)修改域名服务器为自定义服务器，然后添加域名服务器地址：`f1g1ns1.dnspod.net`和`f1g1ns2.dnspod.net`。（这一步在[DNSPod](https://www.dnspod.cn/)应该会有提示）
4. 等待域名解析生效。
## CNAME
之后要回到`username.github.io`项目根目录，添加一个名为`CNAME`的文件，有两点要求：
1. `CNAME`文件名必须为大写。
2. 文件内容只应列出一级域名或二级域名，例如，`example.com`或`blogexample.com`而不是`https://example.com`。

`CNAME`文件只能包含一个域。要将多个域指向同一个页面，需要通过DNS设置重定向。  
需要提醒的一点是，如果你使用形如`example.com`这样的一级域名的话，需要在DNS处设置A记录到207.97.227.245（这个地址会有变动，[这里](https://help.github.com/articles/troubleshooting-custom-domains/)查看），而不是在DNS处设置为CNAME的形式，否则可能会对其他服务（比如email）造成影响。（这一段是参考[beiyuu博客](http://beiyuu.com/github-pages)中看到的，但是目前没有遇到问题，不清楚作者给的地址是哪儿来的，并没有按照作者的改）  
也可以在我们repository里面的setting里面，在github page里面的custom domain里填写自己的买的域名。（这一段是参考[pkuduo博客](http://pkuduo.cn/blog/2018/01/05/first-blog/)看到的）
## Jekyll模板系统
GitHub Pages为了提供对HTML内容的支持，选择了[Jekyll](https://jekyllcn.com/)作为模板系统，Jekyll是一个强大的静态模板系统，作为个人博客使用，基本上可以满足要求，也能保持管理的方便。  
### 安装Jekyll
1. Jekyll是一款基于Ruby的插件，需要先安装Ruby。  
下载[rubyinstaller](https://rubyinstaller.org/downloads/)，选择WITH DEVKIT的最新就可以，我目前是2.6.0-1版本。按照默认安装即可，当然路径可以改，但是安装目录不允许包含空格，“Add Ruby executables to your PATH”已经被自动选中了。  
安装完成会有MSYS64的安装提示，继续按默认安装即可（好像安装时需要改成国内的下载源，但是我不需要，所以没有改，需要可以自己查一下）。  
完成后进入“cmd”输入`ruby -v`显示版本则安装成功。
2. 判断gem是否安装。  
在“cmd”输入`gem -v`显示版本号则安装成功。（gem好像是跟着Ruby自动安装的）
3. 安装Jekyll。  
在“cmd”输入`gem install jekyll`，同样`jekyll -v`显示版本号则安装成功。
### 创建Jekyll模板
在“cmd”进入自己想要创建博客文件夹的地方，然后输入：

    jekyll new myblog
    cd myblog
    jekyll serve
在浏览器`localhost:4000`浏览。  
## 部署Jekyll模板
1. 从远程库克隆：
    + 在GitHub上`username.github.io`项目页点“Clone or download”，复制SSH链接，形如`git@github.com:example/example.github.io.git`。
    + 在想要放个人博客项目的文件夹内右键“Git Bash”，输入`git clone git@github.com:example/example.github.io.git`
2. 将Jekyll模板推送到GitHub:   
将myblog文件夹里的内容复制到example.github.io文件夹中，在后者文件夹中右键“Git Bash”，输入

        git status
        git add .
        git commit -m "sth"
        git push origin master
    `git add .` 监控工作区的状态树，将所有变化提交到缓存区，包括文件的修改和新文件。  
    `git add -u` 监控已被add的文件，将修改文件提交的暂存区，包括文件的修改和删除。  
    `git add -a` 是上边两个功能的合集。
3. 等一会儿浏览器`example.github.io`浏览。
## 推送文章
在example.github.io文件夹中的_posts文件夹中添加`.md`文件，之后推送到GitHub上就行了。
## Markdown
+ [Markdown中文文档](https://markdown-zh.readthedocs.io/en/latest/)
+ Markdown编辑器：
    + Windows上我用的VScode，是一个窗口编辑一个窗口预览的。
    + ipad上有Bear、锤子便签，这几个免费的Markdown编辑器，但是Bear是输入完成后直接显示预览，
    + [Typora](https://typora.io/)软件有OS X、Windows、Linux三个版本，和Bear一样是输入完成后直接预览，试用过还不错。
+ `.md`和`.markdown`后缀是一样的。
## 参考博客
+ [pkuduo博客](http://pkuduo.cn/blog/2018/01/06/install-ruby&jekyll-on-windows/)
+ [beiyuu博客](http://beiyuu.com/github-pages)