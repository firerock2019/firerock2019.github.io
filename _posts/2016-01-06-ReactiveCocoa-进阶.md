---
layout:     post
title:      教程-用微信好友头像拼出指定照片
subtitle:   Python 生成照片墙
date:       2019-04-21
author:     TRY
header-img: img/post-bg-ios9-web.jpg
catalog: true
tags:
    - Python3
    - pynput
    - 熵无限大
    
---
# 前言

>灵感来源：[『又来瞎鼓捣』我用 Python 生成了一张全体微信好友的头像墙](https://mp.weixin.qq.com/s/S2bL7-5uSNvVqiWCMqjG1w)

![Python 楚门的世界](http://img0.imgtn.bdimg.com/it/u=3841198930,2907883680&fm=26&gp=0.jpg)

#### 工具准备

	Windows+Python3
	pynput
	wechat for windows
	AndreaMosaic

#### 操作思想

pynput打几个点，对windows版微信进行操作。操作就是一个接一个的点开头像，然后保存。  
再对储存的头像进行处理，用AndreaMosaic生成图片。

#### 操作方法

**相关链接：**

[python用pynput监听控制键盘鼠标](https://www.jianshu.com/p/03010ac70e4c)  
[pynput 1.4.2](https://pypi.org/project/pynput/)  
谷歌：`多张人脸拼成一张`  

#### 注意事项


- **微信**

	微信这个软件很鸡贼，打开头像的次数太多就故意打开失败。遇到这种情况可以`监控下储存头像的文件夹的文件数`，如果没有增长就重新打开一次这个人的微信头像就好了

	

- **还是微信**

	Python完全模拟键盘和鼠标，涉及一个怎么存完一个人的头像存第二个人的问题。很简单，点一下聊天人那一列，在点回联系人，按下下箭头就可以了（不截图了）


#### 源代码

	无（因为菜）
	
	
#### 写在最后


我这个和sunbelife那个，用到的方法都不一样。他用的是第三方微信的库，我觉得....就还是算了，所以也不构成抄袭。


```

>最后附上启发者GitHub：<https://github.com/sunbelife>
