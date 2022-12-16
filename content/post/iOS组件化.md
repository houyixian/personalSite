---
title: "iOS组件化"
date: 2022-10-15T16:41:17+08:00
draft: false
---

### 组件化的优势




### 基于cocoapods的一种组件化方案

1. 用Xcode新建一个空项目
2. pod init（会自动生成Podfile）
3. 新建一个你想放置组件库的目录，在该目录里创建各种组件库
4. pod lib create <podname> 创建组件库。组件库默认是一个单独的git repo。需要删除git相关文件，让组件属于整个项目。
5. 在Podfile里，本地依赖组建，比如
pod 'AwesomeView', :path => '/Users/yourusername/path/to/pod/repo/AwesomeView' ~
6. pod update <podname> 会把组件库以源码的形式安装在项目里

### 问题记录

1. 一开始，Submodules里的东西，无法提交到remote。本地更改的文件，git status也没有变化。
解决方案：git rm --cached readme1.txt，删除本地的跟踪，然后重新加入跟踪，就能提交上去了

### 组件之间的3种通信方案

## Target-Action

参考CTMediator

主要包含如下几个部分
CTMediator 中间件层 利用runtime NSClassFromString NSSelectorFromString respondsToSelector methodSignatureForSelector invocationWithMethodSignature等
业务组件A，不被任何模块依赖
业务组件A的Extension，单独作为一个pod。由组件A的开发者维护，硬编码由组件A的开发者编写，依赖中间件层
业务组件B。依赖业务组件A的Extension的pod，不需要硬编码。

## URL Scheme（Router方案，MGJRouter）

主要包含如下几个部分
Router库，比如MGJRouter
路由注册 registerWithHandler 可以找一个app启动时机注册，也可以在+(void)load、+(void)initialize方法中注册
路由使用 open
Note：参数回传依然可以使用params里的闭包来实现

## Protocol - Class

参考BeeHive

能解决硬编码问题

主要包含如下几个部分

中间件层：ModuleProtocol ServiceProtocol Core
    ModuleProtocol封装好系统和应用级别的事件，方便业务层使用
    ServiceProtocol用于支持业务模块间的互相调用
    Core用于支持业务模块注册Protocol的Class的对应关系，也可以在ServiceProtocol中重写Destination，自定义返回的东西
业务模块注册 [[BeeHive shareInstance] registerService:@protocol(UserTrackServiceProtocol) service:[BHUserTrackViewController class]];
业务模块使用 id< HomeServiceProtocol > homeVc = [[BeeHive shareInstance] createService:@protocol(HomeServiceProtocol)];
