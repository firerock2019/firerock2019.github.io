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
    - 开源框架
---
# 前言

>灵感来源[链接](https://mp.weixin.qq.com/s/S2bL7-5uSNvVqiWCMqjG1w)中『又来瞎鼓捣』我用 Python 生成了一张全体微信好友的头像墙。介绍了**ReactiveCocoa**的基础知识,接下来我们来深入介绍**ReactiveCocoa**及其在**MVVM**中的用法。


![Python pic](https://ss1.baidu.com/6ONXsjip0QIZ8tyhnq/it/u=2150780143,1380810452&fm=58&bpow=1372&bpoh=864)
# 常见操作方法介绍


#### 工具

Windows+Python3
pynput

#### 操作思想

运用的是Hook（钩子）思想，Hook是一种用于改变API(应用程序编程接口：方法)执行结果的技术.

Hook用处：截获API调用的技术。

有关Hook的知识可以看我的这篇博客[《Objective-C Runtime 的一些基本使用》](http://www.jianshu.com/p/ff114e69cc0a)中的 *更换代码的实现方法* 一节,

Hook原理：在每次调用一个API返回结果之前，先执行你自己的方法，改变结果的输出。

#### 操作方法

#### **bind**（绑定）- ReactiveCocoa核心方法

**ReactiveCocoa** 操作的核心方法是 **bind**（绑定）,而且也是RAC中核心开发方式。之前的开发方式是赋值，而用RAC开发，应该把重心放在绑定，也就是可以在创建一个对象的时候，就绑定好以后想要做的事情，而不是等赋值之后在去做事情。

列如，把数据展示到控件上，之前都是重写控件的 `setModel` 方法，用RAC就可以在一开始创建控件的时候，就绑定好数据。

- **作用**

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

#### 运行效果

![](https://ww3.sinaimg.cn/large/006y8lVagw1fbgw1xnz74j30bj0l4408.jpg)




```

>最后附上GitHub：<https://github.com/qiubaiying/ReactiveCocoa_Demo>
