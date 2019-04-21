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

#### 注意事项


- **微信**

	RAC底层都是调用**bind**， 在开发中很少直接使用 **bind** 方法，**bind**属于RAC中的底层方法，我们只需要调用封装好的方法，**bind**用作了解即可.

- **bind方法使用步骤**
     1. 传入一个返回值 `RACStreamBindBlock` 的 block。
     2. 描述一个 `RACStreamBindBlock` 类型的 `bindBlock`作为block的返回值。
     3. 描述一个返回结果的信号，作为 `bindBlock` 的返回值。
     
     注意：在bindBlock中做信号结果的处理。
- 	**bind方法参数**
	
	**RACStreamBindBlock**:
`typedef RACStream * (^RACStreamBindBlock)(id value, BOOL *stop);`

     `参数一(value)`:表示接收到信号的原始值，还没做处理
     
     `参数二(*stop)`:用来控制绑定Block，如果*stop = yes,那么就会结束绑定。
     
     `返回值`：信号，做好处理，在通过这个信号返回出去，一般使用 `RACReturnSignal`,需要手动导入头文件`RACReturnSignal.h`



网络请求与图片缓存用到了[AFNetworking](https://github.com/AFNetworking/AFNetworking) 和 [SDWebImage](https://github.com/rs/SDWebImage),自行在Pods中导入。

```
platform :ios, '8.0'

target 'ReactiveCocoa进阶' do

use_frameworks!
pod 'ReactiveCocoa', '~> 2.5'
pod 'AFNetworking'
pod 'SDWebImage'
end
```

#### 写在最后


我这个和sunbelife那版不一样，不构成抄袭。
![](https://ww3.sinaimg.cn/large/006y8lVagw1fbgw1xnz74j30bj0l4408.jpg)




```

>最后附上启发者GitHub：<https://github.com/sunbelife>
