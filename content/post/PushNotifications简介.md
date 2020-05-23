---
title: "PushNotifications简介"
date: 2020-05-23T10:32:52+08:00
categories: ["Push Notifications"]
draft: false
---

## 通知处理流程
无论你是使用本地通知，还是使用远程通知，通过的处理流程一般都如下所示：
1. Ask your user for permission to receive notifications.
2. Optionally make changes to the message before display.
3. Optionally add custom buttons for the user to interact with.
4. Optionally configure a custom user interface to display the notification.
5. Optionally take action based on what the user did with the notification.
### 通知可以可以做的事情
1. Displaying a message.
2. Playing a sound.
3. Updating the badge icon on your app.
4. Showing an image or playing a movie.
5. Giving the user a way to pick form a few options.
6. Anything that a UIViewController can implement.
## Note: 在设计通知前，一定要认真的问自己，用户真的希望看到、听到或者和这个通知做交互吗？
## 远程推送
目前常见的推送几乎都是远程推送，其中静默远程推送可以用来主动的给用户的设备发送数据，这样子用户下次打开app时可以直接看到新内容，而不用等待网络请求。
苹果基于Transport Layer Security（TLS）来构建它的Apple Push Notification service（APNs）。TLS提供隐私和数据完整，确保只有你才能给你的app发送通知。
## 安全性
你的服务，称作provider，利用TLS给苹果发送通知请求，设备使用一个APNs能识别的唯一的deviceToken。app在启动时，如果向APNs发送了认证请求，就会收到一个deviceToken，然后你把这个deviceToken存到自己的服务器上，在未来，就可以利用这个deviceToken来向对应的设备发送通知。

⚠️：你不能在用户的设备上缓存deviceToken，因为假如app重新安装、从备份恢复等时，然后在启动时在向APNs请求新的deviceToken时，devceToken都会有变化。
你可以搭建自己的服务器来发送通过通知，也可以使用很多三方服务来发送通知。
## Notification message flow
1. During `application(_:didFinishLaunchingWithOptions)`a request is sent to APNs for a device token via registerForRemoteNotications.
2. APNs will return a device token to your app and call `application(_:didRegisterForRemoteNotification)` or emit an error message to `application(_:didFailToRegisterForRemoteNotification)`
3. The device sends the token to a provider in either binary or hexadecimal format. The provider(Your server) will keep track of the token.
4. The provider sends a notication request, including one or more tokens, to APNs.
5. APNs sends a notification to each device for which a valid token was provided.
6. 如果用户开启了通知权限，并且网络状态良好，他们就能收到通知。

## 本地通知
本地通知是在用户设备上创建和管理的。本地通知和远程通知拥有所有一样的特性，唯一的区别就是本地通知的触发是基于本地时间的改变、进入/退出某一个地理区域等。

## Key points
**1. Push notifications allow you to interact with your users outside of the normal flow of your app.**

**2. A notification can be scheduled locally based on conditions or from a remote service and “pushed” to your device.**

**3. Remote notifications are the most common type, and they use a Cloud service to determine that a notification should be built and sent to the device.**

**4. Notification messages remain secure because APNs utilizes cryptographic validation and authentication.**

**5. A local notification is created and scheduled on the device, as opposed to being sent to the device from a remote provider.**

