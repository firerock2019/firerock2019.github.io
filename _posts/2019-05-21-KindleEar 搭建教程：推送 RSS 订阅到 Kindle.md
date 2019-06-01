---
layout:     post
title:      「收藏」KindleEar搭建
subtitle:   拯救Kindle指南
date:       2019-05-21
author:     TRY
header-img: img/home-bg-o.jpg
catalog: true
tags:
    - Kindle
    - Google App Engine
    - 熵无限大
    
---
# 想法

1、 每个Kindle账号有一个邮箱收书  

2、 发书给Kindle  

3、 用Calibre+Python生成书  

4、 让这个功能在GAE上运行  

# 前言

文章参考：

[KindleEar 搭建教程：推送 RSS 订阅到 Kindle
](https://bookfere.com/post/19.html)

[使用 Kindle EAR 推送 RSS 到 Kindle 设备](https://www.jianshu.com/p/b1f6826152f5)

---

KindleEar 是一款开源的 Python 程序，由网友 cdhigh 发起，托管在 Github。它可运行在免费的 Google APP Engine 上，把 RSS 生成排版精美的杂志模式的 MOBI 文件，并按照设置定时自动推送至你的 Kindle。如果你有 Python 和前端基础，还可以自定义排版，生成你需要的最完美的 MOBI 文件。

![KindleEar 推送 RSS 的效果](http://www.5ikindle.com/uploadImg/20170116/165823809.png)

----
**KindleEar 目前的功能有：**

- 支持类似 Calibre 的 recipe 格式的不限量 RSS/ATOM 或网页内容收集
- 不限量自定义 RSS，直接输入 RSS/ATOM 链接和标题即可自动推送
- 多账号管理，支持多用户和多 Kindle
- 生成带图的杂志格式 mobi 或带图的有目录 epub
- 自动每天定时推送
- 强大而且方便的邮件中转服务
- 和 Evernote/Pocket/Instapaper 等系统的集成

下面这个是体验账号，可以添加一个 RSS 测试推送一下（需要把邮箱账号 kindlefere@gmail.com 添加到 Kindle 的推送信任列表），但请不要正式使用。测试时，请不要更改测试账号密码影响其他人测试。

-  体验/测试地址：http://kindle-ear.tk（或 http://kindlefere-feed.appspot.com）
- 体验/测试账号：用户名 test、密码 123456

---

# 操作步骤

## 注册账号

KindleEar 依赖 Google APP Engine，所以需要Google 账号。

Google 账号注册页面：https://accounts.google.com/SignUp

## Google 账号安全设置

Google 账号在默认可能会拒绝将 KindleEar 上传到 GAE，所以需要设置一下。点击下面的链接进入你的 Google 账号“登录与安全”设置页面，找到“允许不够安全的应用”这一项，点击右边的滑动按钮，将其状态切换为“已启用”。注意，为了账号安全，上传完之后建议将此设置恢复为停用状态。

Google 账号设置页面：https://myaccount.google.com/security#connectedapps

## 创建一个 Google Cloud 项目

KindleEar 是免费托管在 Google Cloud 的 Google App Engine（GAE）应用中的，所以你需要先创建一个 Google Cloud 项目，然后再创建一个 GAE 应用。点击下面的链接并用你的 Google 账号登录。

创建 Google Cloud 项目页面：https://console.cloud.google.com

![1](https://firerock2019.github.io/img/拯救Kindle/1.jpeg)

点击页面左上角的“选择项目”，点击弹出对话框右上角的“新建项目”，然后在“新建项目”页面中输入你喜欢的“项目名称”。项目名称可随意填写，只要是你喜欢并且符合它限定的规则即可。

![2](https://firerock2019.github.io/img/拯救Kindle/2.jpeg)

需要着重注意的是项目名称下方的“项目 ID”，这个 ID 也就是我们后面提到的 APPID。默认情况下，系统会根据你输入的项目名称自动生成项目 ID，但是自动生成字符没有意义，为了方便记忆最好是自定义。点击项目 ID 后面的【修改】按钮，将其修改成你喜欢的字符串组合。这样等 KindleEar 部署成功后，你就可以通过 http://APPID.appspot.com 访问了（注意把 APPID 换成你真实的 APPID）。

![3](https://firerock2019.github.io/img/拯救Kindle/3.jpeg)

## 创建 Google App Engine 应用

Google Cloud 创建完成后需要继续创建一个 GAE 应用，否则直接上传会出现下面这样的错误提示：

``` bash

Error 404: --- begin server output --- 
This application does not exist (project_id=u'sample-appid'). To create an App Engine application in this project, run "gcloud beta app create" in your console.
--- end server output --- 

```

所以，当创建完 Google Cloud 项目之后，还需要手动创建一个 Google App Engine 应用。方法有两种：一种是使用云端 Shell 创建；另一种是在 Console 页面上进行。可根据自己的喜好选用。





方法一：上面那个错误提示给出了解决方案，直接在云端 Shell 中使用一行命令就能搞定。

![4](https://firerock2019.github.io/img/拯救Kindle/4.jpeg)

具体步骤为：点击页面右上角的 [ >_ ] 图标按钮（如下图所示），调出云端 Shell，输入以下命令按回车：

```bash
gcloud beta app create
```

命令执行后会出现 `Which region would you like to choose?` 字样，询问选择应用的位置，在 `Please enter your numeric choice:` 之后输入数字 **1**，稍等片刻即可完成 GAE 应用的创建。

```bash
rm -f uploader.sh* && \
wget https://raw.githubusercontent.com/kindlefere/KindleEar-Uploader/master/uploader.sh && \
chmod +x uploader.sh && \
./uploader.sh
```

按照脚本的提示，输入你的 Gmail 地址和准备好的 APPID，回车，等待上传成功即可。

注意，上面的代码只需要执行一次即可，如果想要重新上传或要更新代码，只需要直接运行 uploader.sh 这个 Shell 文件即可，即在云终端中输入下面这行代码即可：

* 提示：KindleEar 安装脚本托管在 GitHub：https://github.com/kindlefere/KindleEar-Uploader

方法二：点击 Google Cloud 页面左上角的菜单按钮，点击弹出菜单中的“App Engine”。在“您的第一个应用”那里点击“选择一种语言”，选择“Python”。接下来“选择位置”中页面中选择“us-central”，最后点击下一步就等待它自动部署，只到出现“让我们开始吧”的字样，就表示 GAE 应用创建成功。

### 方法二：手动上传

在开始手动上传部署 KindleEar 步骤之前，你需要保证已完成下面提到的准备工作。对于每一项准备工作，如果你已经具备条件，可忽略继续进行下一项。

### 1、下载并安装 Python 2.7

Mac OS X 系统可略过此步骤。为保证正常下载，请复制下面的链接使用迅雷下载。

Python 下载：[Windows](https://www.python.org/ftp/python/2.7.11/python-2.7.11.amd64.msi) ｜ [Mac OS X](https://www.python.org/ftp/python/2.7.11/python-2.7.11-macosx10.6.pkg) ｜ [官方页面](https://www.python.org/downloads/)

### 2、下载并安装 GAE SDK for Python

为保证正常下载，请复制下面的链接使用迅雷下载。

GAE SDK for Python 下载：[Windows](https://storage.googleapis.com/appengine-sdks/featured/GoogleAppEngine-1.9.40.msi) ｜[Mac OS X](https://storage.googleapis.com/appengine-sdks/featured/GoogleAppEngineLauncher-1.9.40.dmg) ｜ [Linux](https://storage.googleapis.com/appengine-sdks/featured/google_appengine_1.9.40.zip) ｜ [官方页面](https://cloud.google.com/appengine/downloads#Google_App_Engine_SDK_for_Python)

### 3、安装配置 SocksiPy

SocksiPy 下载：[下载链接](http://jaist.dl.sourceforge.net/project/socksipy/socksipy/SocksiPy 1.00/SocksiPy.zip) ｜ [官方页面](http://socksipy.sourceforge.net/)

Mac OS X 无需下载此文件，打开“终端”输入以下命令安装 SocksiPy：

```bash
sudo pip install PySocks
```

*注意，如果提示找不到 pip 命令，请先下载 [get-pip.py](https://raw.github.com/pypa/pip/master/contrib/get-pip.py)，然后运行 `python get-pip.py` 安装 pip。

在 Windows 中，将下载到的 SocksiPy 压缩包解压缩得到文件 **socks.py**，把这个文件拷贝到 GAE SDK for Python 安装目录，如果你是没有修改安装路径，默认情况下其路径如下所示：

```none
C:\Program Files (x86)\Google\google_appengine
```

### 4、为 GAE SDK 配置代理

在 Windows 中，进入 GAE SDK for Python 安装目录，找到文件 **appcfg.py**，通过右键菜单“Edit with IDLE”，或使用代码编辑软件（如 Sublime Text）将其打开。

在 Mac OS X 中，进入“应用程序”目录，找到并右键点击 GoogleAppEngineLauncher 应用，在弹出的菜单中点击“显示包内内容”，在下面所示的路径中找到 **appcfg.py**，使用代码编辑器打开。

```none
Contents/Resources/GoogleAppEngine-default.bundle/Contents/Resources/google_appengine/
```

找到下面这行代码：

```python
"""Convenience wrapper for starting an appengine tool."""
```

在它上面添加下列代码：

```python
import socks
import socket
socket.socket = socks.socksocket
socks.setdefaultproxy(socks.PROXY_TYPE_SOCKS5, "127.0.0.1", 8788)
```

注意，如果你使用的是 ShadowSocks，请把上面代码中的端口 `8788` 修改为 `1080`。

修改完毕后保存。

### 5、下载 KindleEar 源码

点击下面的链接下载 KindleEar 源码。如果你习惯使用 Git，也可以将项目 Clone 到本地。

KindleEar 下载：[下载链接](https://github.com/cdhigh/KindleEar/archive/master.zip) ｜ [项目页面](https://github.com/cdhigh/KindleEar)

### 6、配置 KindleEar 程序

解压缩下载到的 KindleEar 压缩包（如果是通过 Git 的 Clone 命令获取的则不用解压缩），把 KindleEar 的源代码放到你喜欢的地方。为方便演示，在本例中放置的位置如下所示：

Windows 系统中，将其放到了 C 盘根目录，路径为：

```bash
C:\kindleear
```

Mac OS X 系统中，将其放到了用户目录下，路径为：

```bash
/Users/YOURNAME/Applications/kindleear
```

接下来进入 KindleEar 源代码文件夹，按照下表修改文件并保存：

| 要修改的文件                             | 要修改的内容                      | 内容修改说明                          |
| :--------------------------------------- | :-------------------------------- | :------------------------------------ |
| app.yaml                                 | `application: **xxx**`            | 红色部分修改为你创建的 APPID          |
| module-worker.yaml                       | `application: **xxx**`            | 红色部分修改为你创建的 APPID          |
| config.py                                | `SRC_EMAIL = "**xxx@gmail.com**"` | 红色部分修改为你创建应用的 Gmail 邮箱 |
| `DOMAIN = "https://**xxx**.appspot.com`“ | 红色部分修改为你创建的 APPID      |                                       |

### 7、上传 KindleEar 程序

快捷键 Win + R 调出“运行”对话框，输入“CMD”回车打开命令提示符，然后使用 `cd` 命令进入 GAE SDK for Python 安装目录。

在 Windows 系统中，如果你是没有修改安装路径，应该是输入以下命令（小窍门：输入 cd 后，把 google_appengine 文件夹拖到命令提示符上）：

```bash
cd C:\Program Files (x86)\Google\google_appengine
```

分别输入下面两条命令将 KindleEar 上传到 GAE：

```bash
C:\python27\python.exe appcfg.py update C:\kindleear\app.yaml C:\kindleear\module-worker.yaml
C:\python27\python.exe appcfg.py update C:\kindleear
```

在 MAC OS 系统中，打开“终端”，分别输入下面两条命令上传 KindleEar：

```bash
appcfg.py update ~/Applications/kindleear/app.yaml ~/Applications/kindleear/module-worker.yaml
appcfg.py update ~/Applications/kindleear
```

出现“Email”提示，输入你的 Gmail 邮箱地址，回车；出现“Password for xxx@gmail.com”，输入你的 Gmail 邮箱密码（密码输入时不显示），回车。

等待上传。在上传的过程中，如果第一次上传，会出现一个许可验证消息，如果是同一个电脑，直接打开链接验证即可。如果是不同的电脑，需要添加参数 `--noauth_local_webserver`，打开链接获取验证码，填写验证。

注意，`--noauth_local_webserver` 后缀是添加到 `appcfg.py` 后面，如下所示：

```bash
python appcfg.py --noauth_local_webserver ...
```

# 常见问题

## 四、常见问题

很多小伙伴在安装和使用 KindleEar 的过程中会遇到一些问题，这些问题很常见也很好解决，所以书伴将这些问题整理归纳如下。如果你遇到了新的问题也可以留言提出。

#### 1、登录出现 Invalid username or password

如果确认输入的账号密码正确却仍然出现“`Invalid username or password.`”，请[点击这里](https://security.google.com/settings/security/activity)检查一下相关选项。首先找到“允许不够安全的应用”这个选项，确认是“已启用”状态。然后查看下账号登录是否被 Google 拦截，把可疑操作确认是自己操作，然后重新运行 uploader.bat 走一遍过程。如果取消拦截后还是出现“Invalid username or password.”这样的提示，请先使用浏览器登陆你的 Google 账号，看是否会有短信验证等提示，登录成功后重新运行 uploader.bat 走一遍过程。

#### 2、无法为其它 Google 账号上传程序

当为一个 Google 账号下的 GAE 上传 KindleEar后，程序会自动记录登陆邮箱密码，这样使用其它账号就无法上传了。想要切换帐号，Windows 系统请前往目录“`C:\用户\你的用户名\`”下删除“.appcfg_cookies”，然后重新运行 uploader.bat。Mac 或 Linux 系统遇到同样问题，使用命令 `rm ~/.appcfg_oauth2_tokens` 删除。

#### 3、访问页面提示 internal serve error

如果是刚上传完 KindleEar，由于 GAE 需要一些时间索引数据，在此期间访问某些页面会出现“internal serve error”的错误提示，最长等待十来分钟即可正常访问。

如果等待很长时间仍然出现“internal serve error”，请尝试这样操作：点击 Google Cloud 左上角的菜单按钮，点击弹出菜单中的“**数据存储**”，再点击数据存储页面左侧导航中的“**索引**”进入索引页面。

查看一下 Book、DeliverLog、Feed 三项的状态，如果是绿色对勾则正常，否则就需要重新索引一下。具体操作为，进入 KindleEar 目录，运行下面这条命令更新一下索引：

```shell
$ python appcfg.py update_indexes ./
```

注意，如果使用的是 Google 云端 Shell 直接使用 appcfg.py 命令即可：

```shell
$ appcfg.py update_indexes ./
```

待索引状态全部变成“**使用中**”后，也就是每一项都变成绿色对勾，即可正常访问。

#### 4、投递状态显示 wrong SRC_EMAIL

默认状态下 GAE 不允许发信，所以才会出现 wrong SRC_EMAIL 的提示，你需要设置一下把 Gmail 邮箱地址添加到“**Mail API 已获授权的发件人**”，这需要在 GAE 应用的设置中进行。

![img](https://bookfere.com/wp-content/uploads/2016/11/gae-add-email-address.gif)

▲ 向 GAE 设置中添加 Gmail 邮箱示意动画

首先访问下面这个网址进入 GAE 应用设置（将其中的 APPID 换成你真实的 APPID）：

```
https://console.cloud.google.com/appengine/settings?project=APPID
```

\* 也可以点击左上角的菜单，在弹出的菜单中点击“**App Engine**”，然后再点击 APP 引擎页面左侧的“**设置**”。

切换到“**电子邮件发件人**”，看一下“**Mail API 已获授权发件人**（Mail API authorized senders）”账号里面有无添加发送邮箱地址，如果没有就点击上方的“**添加**”按钮或“**添加已获授权的电子邮件发件人**”按钮添加一下邮箱地址，注意添加完后要**按回车确认**一下，最后点击“**添加**”，此问题即可解决。

#### 5、忘记 KindleEar 的登录密码怎么办

忘记密码可以进入 GAE 重置密码，具体方法为：访问 https://console.cloud.google.com，点击左上角的菜单，点击菜单中的“数据存储”，然后在“按种类查询”的标签项下方的“种类”中，选择“KeUser”，最后点击用户记录“名称/ID”，编辑其中的 passwd，改成 e10adc3949ba59abbe56e057f20f883e，最后点击【保存】按钮保存一下，这样就把密码临时改成了123456，登录账号修改成新密码即可。

#### 6、能否请书伴代为部署 KindleEar

如果你懒得自己部署，只想直接使用现成的程序，可以前往书伴的“[捐助](https://bookfere.com/donate)”页面扫码支付 30 元，注意备注你的 Gmail 地址，然后把你的 Gmail 账号和密码到 [bookfere@gmail.com](mailto:bookfere@gmail.com) 通知书伴即可。

## 六、自制订阅

如果你想推送的内容不提供 RSS 供稿，并且 KindleEar 内置的订阅也不能满足你的需求，建议尝试通过下面这三篇文章提供的方法自制 KindleEar 抓取脚本，教程面向没有编程基础的小伙伴。

#### 1、[如何用 KindleEar 推送无 RSS 的网站内容（上篇）](https://bookfere.com/post/750.html)

本文是如何用 KindleEar 推送无 RSS 的网站内容的上篇。对 KindleEar 抓取网站内容的两种方式以及 KindleEar 的抓取脚本做了必要说明，并提供了 KindleEar 调试环境的搭建步骤。

#### 2、[如何用 KindleEar 推送无 RSS 的网站内容（中篇）](https://bookfere.com/post/752.html)

本文是如何用 KindleEar 推送无 RSS 的网站内容的中篇。详细介绍了 KindleEar 订阅脚本的工作原理，对订阅脚本的编写过程做了详细分解说明，最终实现从网站抓取文章并转换成电子书。

#### 3、[如何用 KindleEar 推送无 RSS 的网站内容（下篇）](https://bookfere.com/post/753.html)

本文是如何用 KindleEar 推送无 RSS 的网站内容的下篇。完善了上篇编写订阅脚本，使其可处理文章列表和内容的翻页，以及抓取符合设定条件的文章条目，最后介绍了上传 KindleEar 的两种方式。

# Rss源分享

少数派
https://sspai.com/feed

好奇心日报 
https://rsshub.app/qdaily/category/5

简书-热门 
https://rsshub.app/jianshu/trending/weekly

V2EX 
https://rsshub.app/v2ex/topics/latest

看雪论坛WEB 
https://rsshub.app/pediy/topic/web

国家地理 
https://rsshub.app/natgeo/dailyphoto

半月谈 
https://rsshub.app/banyuetan/jicengzhili

极客公园-全球快讯 
https://rsshub.app/geekpark/breakingnews

央视新闻 
https://rsshub.app/cctv/world

观止 
https://rsshub.app/guanzhi

中国政府-最新政策 
https://rsshub.app/gov/zhengce/zuixin

One 
https://rsshub.app/one