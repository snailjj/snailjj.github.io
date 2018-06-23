---
layout: post
title: "iOS 10 UserNotifications——Notifications"
description: "UserNotifications学习"
date: 2017-05-23 10:22:11.000000000 +09:00
tag: iOS
---



## 远程推送的原理在这里就不多bb了，上图。

![image](/assets/images/post/2017/05/20170527132930.png)

---
- 首先在AppDelegate.m里导入头文件

`import UserNotifications`

- 注册推送 在didFinishLlaunchingWithOptions方法里面写


```
        //iOS 10+
        if #available(iOS 10.0, *) {
            center = UNUserNotificationCenter.current()
            center.delegate = self
            center.requestAuthorization(options: [.alert,.badge,.carPlay,.sound]) { (granted, error) in
                if granted {
                    print("注册通知成功")
                }else {
                    print("注册通知失败")
                }
            }
            center.getNotificationSettings(completionHandler: { (settings) in
                print(settings)
            })
            
        } else {
            //iOS 10 之前
            let types : UIUserNotificationType = [UIUserNotificationType.badge,.alert,.sound]
            application.registerUserNotificationSettings((UIUserNotificationSettings(types: types, categories: nil)))
        }
        application.registerForRemoteNotifications()//iOS 10 之前
        let types : UIUserNotificationType = [UIUserNotificationType.badge,.alert,.sound]
        application.registerUserNotificationSettings((UIUserNotificationSettings(types: types, categories: nil)))
```

- 程序在前台时，获取推送消息并且做处理的入口即可写在下面的方法里

```
    func application(_ application: UIApplication, didReceiveRemoteNotification userInfo: [AnyHashable : Any]) {
        print("didReceiveRemoteNotification----\(userInfo)")
    }
    
    func application(_ application: UIApplication, didReceiveRemoteNotification userInfo: [AnyHashable : Any], fetchCompletionHandler completionHandler: @escaping (UIBackgroundFetchResult) -> Void) {
        print("didReceiveRemoteNotification----\(userInfo)---fetchCompletionHandler")
    }

```

- 点击推送消息进入App的入口是

```
 func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey: Any]?) -> Bool {
        // Override point for customization after application launch.
        if let launchOpts = launchOptions?[UIApplicationLaunchOptionsKey.remoteNotification] as? NSDictionary {
            //根据launchOpts数据来判断哪种推送 进而做那种操作
            print(launchOpts)
        }
    }

```

- iOS10以后 推送消息在3d-touch的作用下会有各种操作，如下图

![image](/assets/images/post/2017/05/201705231802.PNG)

1、首先自定义Notification Action
```
        //MARK: - Notification Action
        let replyAction = UNNotificationAction(identifier: "reply", title: "reply", options: [UNNotificationActionOptions.foreground])
        let deleteAction = UNNotificationAction(identifier: "delete", title: "delete", options: [UNNotificationActionOptions.authenticationRequired])
        let cancelAction = UNNotificationAction(identifier: "cancel", title: "cancel", options: [UNNotificationActionOptions.destructive])
```
2、加入Notification Category
``` 
 let category1 = UNNotificationCategory(identifier: "message1", actions: [deleteAction,cancelAction], intentIdentifiers: [], options: [UNNotificationCategoryOptions.customDismissAction])
        
        let category2 = UNNotificationCategory(identifier: "message2", actions: [replyAction,cancelAction], intentIdentifiers: [], options: [UNNotificationCategoryOptions.customDismissAction])
        
        UNUserNotificationCenter.current().setNotificationCategories([category1,category2])
```

3、推送消息的格式
 ```
 "aps" : {
        "alert" : "Your message here.",
        "badge" : 9,
        "sound" : "default",
	    "category" : "message2"
    },
```

4、得到消息后的处理入口

```
//MARK： - 处理消息 获取categoryId及ActionId后做相应的处理即可
    func userNotificationCenter(_ center: UNUserNotificationCenter, didReceive response: UNNotificationResponse, withCompletionHandler completionHandler: @escaping () -> Void) {
        
        let categoryId = response.notification.request.content.categoryIdentifier
        let actionId = response.actionIdentifier
        let trigger = response.notification.request.trigger
        
        print("category:\(categoryId)-----actionId:\(actionId)----trigger\(trigger)")
        
        completionHandler()
    }

```



#### Swift 得到DeviceToken的方法
```
func application(_ application: UIApplication, didRegisterForRemoteNotificationsWithDeviceToken deviceToken: Data) {
        let device = NSData(data: deviceToken)
        let deviceId = device.description
            .replacingOccurrences(of:"<", with:"")
            .replacingOccurrences(of:">", with:"")
            .replacingOccurrences(of:" ", with:"")
        print("我的deviceToken：\(deviceId)")
    }

```