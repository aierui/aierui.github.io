---
title: 如何使用10个小时搭建出个人域名而又Geek的独立博客？
date: 2016-01-03 16:47:44 
art: a
id: 16010316
categories: 教程
tags: 
- 个人域名独立博客
- 10小时轻松搞定
- 我的第一片新天空
description: 摘要：我在这里写下长篇大论，只希望小白们能跟快速入门。坚持10个小时坚持10个小时坚持10个小时（重要的事情说三遍！！！）搭建出属于自己的个人独立博客，我将会通过 安装流程主线+优质文章 作为参考。由于我本人是一名学生（非计算机专业），喜欢尝试和不愿意人与亦云想要不一样的人生打小喜欢折腾便开始了搭建自己独立博客的道路，这路上可谓是路途凶险和艰难费了我九牛二虎之力解决，还是不够完美，但我希望他完美，希望他可以记录我的时光。
---


摘要：我在这里写下长篇大论，只希望小白们能跟快速入门。**坚持10个小时** **坚持10个小时** **坚持10个小时**（重要的事情说三遍！！！）搭建出属于自己的个人独立博客，我将会通过 **安装流程主线+优质文章** 作为参考。由于我本人是一名学生（非计算机专业），喜欢尝试和不愿意人与亦云想要不一样的人生打小喜欢折腾便开始了搭建自己独立博客的道路，这路上可谓是路途凶险和艰难费了我九牛二虎之力解决，还是不够完美，但我希望他完美，希望他可以记录我的时光。作为一名技术小白，没有技术基础。看到网上教程更是玲琅满目直至眼花缭乱无从下手，讲真，我从开始接触到成功搭建花费了不低于100小时，走了很多弯路和误区， 希望通过本教程可以真心小白一个敢于尝试的机会。我会将这篇教程写仔细，会将我出现过的问题给予解决方法。（同时这也是我第二次，就在前几分钟，电脑过热，系统崩了，快写完的教程没有按Ctrl+S的情况不翼而飞了。我现在的内心是崩溃的）但是我知道坚持一会，就可以完成了。

# 前言

天生倔强不愿屈服的我，总受想做出一番成绩来，不愿意随波逐流。我为什么要在这个博客已经不盛行的时代去搭建属于自己的博客？可以看看我之前写的[《重新认识自己》](http://www.jianshu.com/p/726d08eda977)和[《我为什么那么懒？》](http://www.jianshu.com/p/b66e1322dbc8)。不去折腾怎么能知道自己不行？未知的东西太多，需要我们去学习和掌握的数不尽数，唯有时刻保持这一份对新事物的好奇心并真心有心去坚持下去。

# 疑问

 先给大家预览下我的博客目前最终版**[视己慎独](http://blog.shijinrong.cn/)**，很多人用 wordpress，你为什么要用 github pages 来搭建？为什么要搭建一个独立博客?独立博客与微信公众平台有什么区别？
- 1、 无需购置服务器，目前的blog挂载在Github Pages，免服务器费的同时还能做负载均衡；github pages有300M免费空间，资料自己管理，保存可靠；学着用 github，享受 github 的便利，上面有很多大牛，眼界会开阔很多；github 是趋势，像eleme这种互联网大公司都在github上完成自己的项目；顺便看看 github 工作原理，最好的团队协作流程；你不觉得一个文科生用 github 很 geek 吗？瞬间跻身技术界。
- 2、独立的才是自己的。在知乎上有这样一个话题[《GitHub 能作为衡量程序员能力的指标吗？》](http://www.zhihu.com/question/24972520)，在我看来独立博客是喜欢尝试新事物的人新一片天空，他们可以在这片天空中翱翔，他可以不太受拘束爱上些自由，他的内心因他的不羁和外表不屈，愿意潜心研究深钻其爱好，同时在这里他可以结实一大批有着共同的爱好的追梦人。对于小白，请保持记得那份好奇心，坚持尝试下去，继续折腾。
- 3、公众账号是对所有人开放的，简单申请即可使用，无需太多的挑战。他仅仅只是一个平台（对一般人来说）同时好好做运营也似乎不是一件简单的事情，没有足够的经历和精力是很难达到一个高度，也很难去传播你的文化价值观念。博客也只是一个平台，但是这里有你想要的，也是你的用武之地。公众账号是一个一对多的平台很难利于交流尽管现在越来越人性化，这点你的博客很轻松就可以做到。更多的区别在此不多分析。

# 成功方向
- 1、安装准备软件 Node.js、Git、GitHub DeskTop（前两个必须安装，后者可选）
- 2、本地搭建hexo框架、配置主题、修改参数、实现本地测试预览
- 3、链接GitHub、实现在线预览
- 4、购买域名并解析 （这里告诉大家一个方法，1元购买一个使用期限为一年.cn的域名 **仅高校学生可以** ）
- 5、日后站点的管理和运营


> 纸上得来终觉浅，要知此事须举行。世上无难事，就怕是懒人。以下以我的博客:blog.shijinrong.cn（shijinrong.cn是我一下行动）在windows下为例，教大家如何搭建一个独立博客。

# 安装流程

## 安装准备软件
- [Node.js](https://nodejs.org/en/)
- [Git](http://git-scm.com/downloads)
- [GitHub Desktop](https://desktop.github.com/) (可选)
以上几个软件均是英文版本，请小白不要害怕，敢于面对。安装简单，在此不做详细介绍。

## 本地搭建hexo框架、配置主题

![Hexo 静态博客搭建流程](https://static.shijinrong.cn/hexo.jpg)

#### 目录
##### I.Hexo简介
##### II.Hexo安装方法
##### III.Hexo配置方法
##### IV.Hexo主题修改
##### V.Hexo部署方法

### I.Hexo简介
Hexo 是一个轻量的静态博客框架。通过Hexo可以快速生成一个静态博客框架,仅需要几条命令就可以完成,相当方便。

而架设Hexo的环境更简单了 不需要 lnmp/lamp/XAMPP 这些繁琐复杂的环境 仅仅需要一个简单的http服务器即可使用 或者使用互联网上免费的页面托管服务
比如本人的这个博客 就是托管于 GitHub Pages服务上

### II.Hexo安装方法
参考官网[中文文档](https://hexo.io/zh-cn/docs/),请尝试者仔细读教程和官方文档。这步很简单，正如官方网站写的那样只需要一条命令即可自动安装hexo框架。

	$ npm install -g hexo-cli #使用 npm 安装 Hexo。

#### 初始化hexo
请参考hexo[官方文档](https://hexo.io/zh-cn/docs/setup.html),init命令中的`<folder>`就是文件夹aierui.github.io。初始化后，aierui.github.io里面就已经有完整的Hexo框架了,这里可以在任意地方新建立一个文件夹并命名为aierui.github.io【不要问为什么】打开该文件，点击鼠标右键你会看到一个Git bash here点击跳出git的黑窗口，输入命令`$ npm install`，完成后，指定文件夹的目录如下：
![hexo初始化的目录](https://static.shijinrong.cn/%E5%88%9D%E5%A7%8B%E5%8C%96hexo.jpg)

### III.Hexo配置方法

#### 熟悉hexo
为了让读者快速了解Hexo，我作几个简单介绍吧。当然，更多的还是需要仔细阅读文档才能了解更详细。
+ _config.yml 全局配置文件。要注意的是，该文件格式要求极为严格，缺少一个空格都会导致运行错误。小提示：不要用Tab缩进，两个空格符， **冒号：后面只用一个空格即可** 。
+ themes 存放主题的文件夹
+ source 博客资源文件夹
+ source/_drafts 草稿文件夹
+ source/_posts 文章文件夹
+ themes/landscape 默认皮肤文件夹
……
官方文档中教详细。

#### 配置hexo
做一些基础配置即可，请参考配置[官方文档](https://hexo.io/zh-cn/docs/configuration.html)，这里也可以省略，因为在后面配置主题NExt是也有提到这里的配置修改。

### IV.Hexo主题修改

Hexo主题非常多，可以参考丰富多彩的[Hexo主题](https://hexo.io/themes/)，本文选Next为主题，样式参考我的博客[视己慎独](http://blog.shijinrong.cn/)。

到这里我们还是采用参考[官方文档](http://theme-next.iissnan.com/),5 分钟快速安装。在本地修改完这一连串的配置，（包括：语言设置、财产、菜单设置、侧栏设置、头像设置、作者名称、站点描述、标签云页面、分类页面、统计系统、评论系统等等）现在是需要下面的一个命令即可在本地成功预览你的博客样式。

### V.Hexo部署方法

写完文章之后 就可以启动本地服务器测试了

	$ hexo s #启动本地服务器测试

这个时候在浏览器中输入http://localhost:4000端口 静态的网站架设完成

![效果如图](https://static.shijinrong.cn/%E6%9C%AC%E5%9C%B0%E9%A2%84%E8%A7%88hexo.jpg)

当你修改好你想要的样式，包括头像，favicon图标，标题样式，第三方平台链接等等等等你心中完美的页面。那就可以继续下一个阶段了。再提示一点，大家可以hexo主题修改一步就hexo s看下变化，初次接触对参数不清楚。只有hexo s后在可以在本地浏览到效果，Ctrl+C 停止服务器。


## 链接GitHub、实现在线预览

#### 目录
##### I.注册GitHub
##### II.配置和使用 Github
##### III.SSH Key 配置成功
##### IV.实现在线预览

现在已经来到第三部分了，请你在坚持一下。

### I.注册GitHub

访问：http://www.github.com/ 注册你的username和邮箱，邮箱十分重要，GitHub上很多通知都是通过邮箱的。注册过程比较简单,在此我不再啰嗦。界面任然是英文，请读者耐心一点。

### II.配置和使用 Github

#### 配置 SSH keys
我们如何让本地git项目与远程的github建立联系呢？用SSH keys。

**检查 SSH keys的设置**，首先我们需要检查你电脑上现有的ssh key：

	$ cd ~/.ssh 检查本机的ssh密钥

如果提示：No such file or directory 说明你是第一次使用git。

**生成新的SSH Key：**

	$ ssh-keygen -t rsa -C "邮件地址@youremail.com"
	Generating public/private rsa key pair.
	Enter file in which to save the key (/Users/your_user_directory/.ssh/id_rsa):<回车就好>

注意1: 此处的邮箱地址，你可以输入自己的邮箱地址；注意2: 此处的「-C」的是大写的「C」

然后系统会要你输入密码：

	Enter passphrase (empty for no passphrase):<输入加密串>
	Enter same passphrase again:<再次输入加密串>

在回车中会提示你输入一个密码，这个密码会在你提交项目时使用，如果为空的话提交项目时则不用输入。这个设置是防止别人往你的项目里提交内容。

注意：输入密码的时候没有*字样的，你直接输入就可以了。

最后看到这样的界面，就成功设置ssh key了：

![效果如图](https://static.shijinrong.cn/ssh%20key.jpg)

#### 添加 SSH Key 到 GitHub

在本机设置SSH Key之后，需要添加到GitHub上，以完成SSH链接的设置。

1、打开本地C:\Documents and Settings\Administrator.ssh\id_rsa.pub文件。此文件里面内容为刚才生成人密钥。如果看不到这个文件，你需要设置显示隐藏文件。准确的复制这个文件的内容，才能保证设置的成功。
2、登陆github系统。点击右上角的 Account Settings—>SSH Public keys —> add another public keys
3、把你本地生成的密钥复制到里面（key文本框中）， 点击 add key 就ok了

#### 测试

可以输入下面的命令，看看设置是否成功，git@github.com的部分不要修改：

	$ ssh -T git@github.com

如果是下面的反馈：

	The authenticity of host 'github.com (207.97.227.239)' can't be established.
	RSA key fingerprint is 16:27:ac:a5:76:28:2d:36:63:1b:56:4d:eb:df:a6:48.
	Are you sure you want to continue connecting (yes/no)?

不要紧张，输入yes就好，然后会看到：


Hi aierui! You've successfully authenticated, but GitHub does not provide shell access.

#### 设置用户信息

现在你已经可以通过 SSH 链接到 GitHub 了，还有一些个人信息需要完善的。

Git 会根据用户的名字和邮箱来记录提交。GitHub 也是用这些信息来做权限的处理，输入下面的代码进行个人信息的设置，把名称和邮箱替换成你自己的，名字必须是你的真名，而不是GitHub的昵称。

	$ git config --global user.name "aierui"//用户名
	$ git config --global user.email  "imland@outlook.com"//填写自己的邮箱

### III.SSH Key 配置成功

本机已成功连接到 github。若有问题，请重新设置。常见错误请参考：
[GitHub Help - Generating SSH Keys](https://help.github.com/articles/generating-ssh-keys/) 和 [GitHub Help - Error Permission denied (publickey)](http://help.github.com/articles/error-permission-denied-publickey)

### IV.实现在线预览

#### 创建仓库和本地远程到GitHub仓库

首先在GitHub上创建一个仓库repository，注意仓库名称必须为aierui.github.io，也是你之前在本地建立的文件夹名称 ![效果如图](https://static.shijinrong.cn/astoneprepository.jpg)，这里由于存在这个名称的仓库，无法重名。

	# 将当前的改动暂存在本地仓库
	$ git add .
	# 将暂存的改动提交到本地仓库，并写入本次提交的注释是”first post“
	$ git commit -m "first post"
	# 将远程仓库在本地添加一个引用：origin
	$ git remote add origin https://github.com/username/projectName.git
	# 向origin推送gh-pages分支，该命令将会将本地分支gh-pages推送到github的远程仓库，并在远程仓库创建一个同名的分支。该命令后会提示输入用户名和密码。
	$ git push origin gh-pages

在GitHub上将gh-pages merge 到msater上

#### 添加部署代码

在站点的-config.yml文件新增字段
**Deployment  站点部署到github要配置这里, 非常重要**

	deploy:
  	 type: git 部署类型若有问题，其他类型自行google之
  	 repository: https://github.com/Aierui/aierui.github.io.git
  	 branch: master
  	 plugins: -hexo-generator-feed

merge后就可以部署上去了，在Git命令黑窗口里输入
  
	$ hexo g #生成静态网页
	$ hexo d #开始部署

完成以上步骤，你算是成功了。在浏览器中输入aierui.github.io（自己对应即可）看到了你在本地搭建的博客主页一样，哇哇哇哇哇哇。开心死你了，不要忘了回来给我点赞呀~
Enjoy~


## 购买域名并解析

这一环节相对简单，可以参考[一步步在GitHub上创建博客主页(3)](http://www.pchou.info/web-build/2013/01/05/build-github-blog-page-03.html),

### 一元搞定域名（重头戏）

**仅限在校的高校学生，社会人士请自行绕开，老老实实花钱购买吧**

废话少说，直接上链接看我是怎么办到的[一元搞定域名还送服务器](http://bbs.qcloud.com/thread-9953-1-1.html)，全体咆哮。我们大家一起欢呼一起咆哮吧，哈哈哈。

云+校园计划是腾讯云为在读高校生量身打造的扶持计划，旨在为高校生提供先进的技术支持、资金扶持和经验分享。同时让更多高校生了解云计算及互联网知识，为后续职业、创业发展奠定基础。

**学生们请仔细研读腾讯云官方论坛领取的规则参与领取**

### 将独立域名与 GitHub Pages 的空间绑定

#### DNS 设置

领取到域名后进行解析，进入到我的域名管理，添加域名，如下图设置。我这里设置了一个三级域名blog，大家可以自行忽略。设置后访问的就是blog.shijinrong.cn了，不是shijinrong.cn哟~~~~
![](https://static.shijinrong.cn/astonepyuming.jpg)

其中A的两条记录指向的ip地址是github Pages的提供的ip

+ 192.30.252.153
+ 192.30.252.154

如博客不能登录，有可能是 github 更改了空间服务的 ip 地址，记得及时到在GitHub Pages查看最新的ip即可

www 指定的记录是你在 github 注册的仓库。

#### GitHub Pages 的设置

去到你的aierui.github.io 仓库，点击 CNAME(没有自行创建) ,再点击右下角的 铅笔 编辑，将 blog.shijinrong.cn 改成你的域名

![](https://static.shijinrong.cn/astonepshijinrong.jpg)

域名绑定成功，域名解析成功，因此你在浏览中输入aierui.github.io或者现在blog.shijinrong.cn均可以访问到主页。

**搭建成功**快和小白自己不愿动手说拜拜吧，同时也恭喜你成为博主。记得常联系我喔~~嘻嘻


## 日后站点的管理和运营

### 如何更新博文

下载博客模板的ZIP，去到你frok的仓库地址：https://github.com/你的用户名/你的用户名.github.io。点击右下角的Download ZIP,你会得到一个名为「你的用户名.github.io-master.zip」的压缩包。
![](https://static.shijinrong.cn/astonepxiazai.jpg)

### 安装 github desktop管理你的博文

这里不再多赘述，可以看看官方文档，有使用说明。

## 图床

推荐使用[七牛](http://www.qiniu.com/)（10G空间，免费，配合Markdown使用简单）。

## MarkDown

百度一大堆教程，但是我还是推荐锤子科技[锤子便签做的教程](https://cloud.smartisan.com/apps/note/md.html)。代码板块的MarkDown请读者自行学习。

# 参考资料

[1][如何在一天之内搭建以你自己名字为域名又具备cool属性的个人博客](http://wingjay.com/2015/12/07/%E5%A6%82%E4%BD%95%E5%9C%A8%E4%B8%80%E5%A4%A9%E4%B9%8B%E5%86%85%E6%90%AD%E5%BB%BA%E4%BB%A5%E4%BD%A0%E8%87%AA%E5%B7%B1%E5%90%8D%E5%AD%97%E4%B8%BA%E5%9F%9F%E5%90%8D%E7%9A%84%E5%BE%88cool%E7%9A%84%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/)  by wingjay（推荐）
[2][一步步在GitHub上创建博客主页(全系列)](http://www.pchou.info/web-build/2013/01/03/build-github-blog-page-01.html) by pchou（推荐）
[3][使用Github Pages建独立博客](http://beiyuu.com/github-pages/) by beiyuu
[4][Hexo 静态博客使用指南](http://www.jianshu.com/p/73779eacb494) by 機智的阿卡林醬（推荐）
[5][如何搭建一个独立博客——简明 Github Pages与 jekyll 教程](http://cnfeat.com/blog/2014/05/10/how-to-build-a-blog/) by cnfeat（推荐）
[6]网站优化：[动动手指，不限于NexT主题的Hexo优化（SEO篇）](http://www.arao.me/2015/hexo-next-theme-optimize-seo/) by ARAO'S BLOG（推荐）
[7][一步步在GitHub上创建博客主页-最新版](http://www.pchou.info/web-build/2014/07/04/build-github-blog-page-08.html)
[8][hexo你的博客](http://ibruce.info/2013/11/22/hexo-your-blog/?utm_source=tuicool) by ibruce
[9+]Thanks to the powerful Baidu and Google, let me solve the problem.

# 关于我

[## 大学生，脑洞大，敢尝试，乐意思考，有梦想，不愿随波浊流]
[## 爱生活，会运动，敢拼搏，敢说敢做，有爱心，知道人情世故]

这里有我更详细的简介：[ABOUT](http://blog.shijinrong.cn/about)

如果你还想了解更多的我和我交流，可以关注我的微信公众号「Astonep」。

![微信公众号：Astonep](https://static.shijinrong.cn/astonepshijishendu.jpg)