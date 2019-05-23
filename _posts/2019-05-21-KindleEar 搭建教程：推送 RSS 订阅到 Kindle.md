---
layout:     post
title:      「收藏」KindleEar搭建
subtitle:   远控程序
date:       2019-05-21
author:     TRY
header-img: img/home-bg-o.jpg
catalog: true
tags:
    - Kindle
    - Google App Engine
    - 熵无限大
    
---
# 前言
文章参考：
[KindleEar 搭建教程：推送 RSS 订阅到 Kindle
](https://bookfere.com/post/19.html)[使用 Kindle EAR 推送 RSS 到 Kindle 设备](https://www.jianshu.com/p/b1f6826152f5)

---

KindleEar 是一款开源的 Python 程序，由网友 cdhigh 发起，托管在 Github。它可运行在免费的 Google APP Engine 上，把 RSS 生成排版精美的杂志模式的 MOBI 文件，并按照设置定时自动推送至你的 Kindle。如果你有 Python 和前端基础，还可以自定义排版，生成你需要的最完美的 MOBI 文件。

![KindleEar 推送 RSS 的效果](https://bookfere.com/wp-content/uploads/2016/04/KindleEar-RSS.png)

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
我的文章都只说我的理解，我觉得这样比较通俗，而且未来我要重新阅读的话也更便于回忆，各位读者就请多担待啦。
# 一、整体思路

1、 google可以给你一个免费的vps去折腾。
2、 利用这个，我们可以写个小脚本。让这个vps每天去几个地方抓数据。
3、 抓好的数据整理成电子书，发送到Kindle的邮箱里。

# 二、操作步骤

KindleEar 依赖 Google APP Engine，所以你需要有一枚 Google 账号（注册完记得安步骤说明设置一下安全选项），然后创建一个 GAE 应用。以下步骤中，如果某个条件已具备，请忽略相应步骤继续。

点击下面的链接，注册一枚带 @gmail.com 后缀的 Google 账号。

Google 账号注册页面：https://accounts.google.com/SignUp

## 3、Google 账号安全设置

Google 账号在默认可能会拒绝将 KindleEar 上传到 GAE，所以需要设置一下。点击下面的链接进入你的 Google 账号“登录与安全”设置页面，找到“允许不够安全的应用”这一项，点击右边的滑动按钮，将其状态切换为“已启用”。注意，为了账号安全，上传完之后建议将此设置恢复为停用状态。

Google 账号设置页面：https://myaccount.google.com/security#connectedapps

## 4、创建一个 Google Cloud 项目

KindleEar 是免费托管在 Google Cloud 的 Google App Engine（GAE）应用中的，所以你需要先创建一个 Google Cloud 项目，然后再创建一个 GAE 应用。点击下面的链接并用你的 Google 账号登录。

创建 Google Cloud 项目页面：https://console.cloud.google.com

点击页面左上角的“选择项目”，点击弹出对话框右上角的“新建项目”，然后在“新建项目”页面中输入你喜欢的“项目名称”。项目名称可随意填写，只要是你喜欢并且符合它限定的规则即可。

需要着重注意的是项目名称下方的“项目 ID”，这个 ID 也就是我们后面提到的 APPID。默认情况下，系统会根据你输入的项目名称自动生成项目 ID，但是自动生成字符没有意义，为了方便记忆最好是自定义。点击项目 ID 后面的【修改】按钮，将其修改成你喜欢的字符串组合。这样等 KindleEar 部署成功后，你就可以通过 http://APPID.appspot.com 访问了（注意把 APPID 换成你真实的 APPID）。

## 5、创建 Google App Engine 应用

Google Cloud 创建完成后需要继续创建一个 GAE 应用，否则直接上传会出现下面这样的错误提示：

```
Error 404: --- begin server output ---
This application does not exist (project_id=u'sample-appid'). To create an App Engine application in this project, run "gcloud beta app create" in your console.
--- end server output ---
```


所以，当创建完 Google Cloud 项目之后，还需要手动创建一个 Google App Engine 应用。方法有两种：一种是使用云端 Shell 创建；另一种是在 Console 页面上进行。可根据自己的喜好选用。

方法一：上面那个错误提示给出了解决方案，直接在云端 Shell 中使用一行命令就能搞定。具体步骤为：点击页面右上角的 [ >_ ] 图标按钮（如下图所示），调出云端 Shell，输入以下命令按回车：

!(创建GoogleAppEngine应用)[https://bookfere.com/wp-content/uploads/2016/04/active-google-cloud-shell.png]

(```)

gcloud beta app create

(```)

命令执行后会出现 Which region would you like to choose? 字样，询问选择应用的位置，在 Please enter your numeric choice: 之后输入数字 1，稍等片刻即可完成 GAE 应用的创建。

方法二：点击 Google Cloud 页面左上角的菜单按钮，点击弹出菜单中的“App Engine”。在“您的第一个应用”那里点击“选择一种语言”，选择“Python”。接下来“选择位置”中页面中选择“us-central”，最后点击下一步就等待它自动部署，只到出现“让我们开始吧”的字样，就表示 GAE 应用创建成功。

自定义RSS
少数派 删除
https://sspai.com/feed
好奇心日报 删除
https://rsshub.app/qdaily/category/5
简书-热门 删除
https://rsshub.app/jianshu/trending/weekly
V2EX 删除
https://rsshub.app/v2ex/topics/latest
V2EX 删除
https://rsshub.app/v2ex/topics/latest
看雪论坛WEB 删除
https://rsshub.app/pediy/topic/web
国家地理 删除
https://rsshub.app/natgeo/dailyphoto
半月谈 删除
https://rsshub.app/banyuetan/jicengzhili
极客公园-全球快讯 删除
https://rsshub.app/geekpark/breakingnews
央视新闻 删除
https://rsshub.app/cctv/world
观止 删除
https://rsshub.app/guanzhi
中国政府-最新政策 删除
https://rsshub.app/gov/zhengce/zuixin
One 删除
https://rsshub.app/one