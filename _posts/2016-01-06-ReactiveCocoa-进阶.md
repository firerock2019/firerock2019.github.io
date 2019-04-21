---
layout:     post
title:      填个坑
subtitle:   Python 生成了一张全体微信好友的头像墙
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

>灵感来源[链接](https://mp.weixin.qq.com/s/S2bL7-5uSNvVqiWCMqjG1w)中『又来瞎鼓捣』我用 Python 生成了一张全体微信好友的头像墙。

![Python 楚门的世界](https://img3.doubanio.com/view/photo/l/public/p480420695.webp)

#### 工具准备

	Windows+Python3
	pynput
	wechat for windows

#### 操作思想

	pynput打几个点，对windows版微信进行操作。操作就是一个接一个的点开头像，然后保存。
	再对储存的头像进行处理，生成图片。

#### 操作方法

**相关链接：**
[python用pynput监听控制键盘鼠标](https://www.jianshu.com/p/03010ac70e4c)
[pynput 1.4.2](https://pypi.org/project/pynput/)
谷歌：多张人脸拼成一张


#### 注意事项


- **微信**
	微信打开头像太多了就故意打开失败，遇到这种情况，`监控下储存头像的文件夹的文件数`，如果没有增长就重新打开一次这个人的微信头像就好了

	

- **还是微信**
	Python完全模拟键盘和鼠标，涉及一个怎么存完一个人的头像存第二个人的问题。很简单，点一下聊天人那一列，在点回联系人，按下下箭头就可以了（不截图了）



#### 写在最后


我这个和sunbelife那版不一样，不构成抄袭。
![](https://ww3.sinaimg.cn/large/006y8lVagw1fbgw1xnz74j30bj0l4408.jpg)




```

>最后附上启发者GitHub：<https://github.com/sunbelife>
