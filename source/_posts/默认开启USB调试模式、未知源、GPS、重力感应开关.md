---
title: 默认开启USB调试模式、未知源、GPS、重力感应开关
date: 2018-11-22 11:12:15
categories: 
  - 日常捣鼓
---

　　默认开启usb调试	system/build.prop中添加或修改

```xml
persist.service.adb.enable=1
```

　　修改未知源

　　1，反编译SettingsProvider.apk

　　2，修改res/values/bools.xml/	修改false为true

```xml
<bool name="def_install_non_market_apps">false</bool>
```

<!-- more -->

　　关闭重力感应

　　1，反编译SettingsProvider.apk

　　2，修改res/values/bools.xml	修改true为false

```xml
<bool name="def_accelerometer_rotation">true </bool>
```



　　关闭GPS

　　1，反编译SettingsProvider.apk

　　2，修改res/values/strings.xml/	修改gps为null，null就是啥都没有的意思

```xml
<string name="def_location_providers_allowed">gps</string>
```

