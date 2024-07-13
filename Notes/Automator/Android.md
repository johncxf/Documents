# Android

官方文档：https://developer.android.com/studio/command-line/adb?hl=zh-cn

## ADB

### 简介

`adb`（Android 调试桥）是一种功能多样的命令行工具，可让`PC`端和`Android`端进行通信

### 工作原理

当您启动某个 adb 客户端时，该客户端会先检查是否有 adb 服务器进程正在运行。如果没有，它会启动服务器进程。服务器在启动后会与本地 TCP 端口 5037 绑定，并监听 adb 客户端发出的命令 - 所有 adb 客户端均通过端口 5037 与 adb 服务器通信。

然后，服务器会与所有正在运行的设备建立连接。它通过扫描 5555 到 5585 之间（该范围供前 16 个模拟器使用）的奇数号端口查找模拟器。服务器一旦发现 adb 守护程序 (adbd)，便会与相应的端口建立连接。请注意，每个模拟器都使用一对按顺序排列的端口 - 用于控制台连接的偶数号端口和用于 adb 连接的奇数号端口。例如：

模拟器 1，控制台：5554
模拟器 1，adb：5555
模拟器 2，控制台：5556
模拟器 2，adb：5557
依此类推

如上所示，在端口 5555 处与 adb 连接的模拟器与控制台监听端口为 5554 的模拟器是同一个。

服务器与所有设备均建立连接后，您便可以使用 adb 命令访问这些设备。由于服务器管理与设备的连接，并处理来自多个 adb 客户端的命令，因此您可以从任意客户端（或从某个脚本）控制任意设备。

### 安装配置

#### Homebrew 安装

```shell
brew cask install android-platform-tools
// 新版 homebrew 使用该指令
brew install --cask android-platform-tools
```

#### 手动安装

##### 下载对应工具并解压

- 官方下载地址：https://developer.android.com/studio/releases/platform-tools?hl=zh-cn

##### 将解压文件存放自定义目录下

- 我这里存放在：`~/.android-sdk-macosx` 目录下

##### 配置

```shell
# 环境变量设置
echo 'export PATH=$PATH:~/.android-sdk-macosx/platform-tools/' >> ~/.bash_profile
# 更新配置文件
source ~/.bash_profile
# 测试是否正常安装
adb devices
```

### 开启adb调试

如要在通过 USB 连接的设备上使用 adb，您必须在设备的系统设置中启用 **USB 调试**（位于**开发者选项**下）

### 常用指令集

- 查看连接设备：`adb devices`
- 安装应用：`adb install test.apk`
- 指定连接设备使用命令：`adb -s ${devices_id} shell`
- 卸载应用，需要指定包：`adb uninstall cn.com.test.mobile `
- 卸载app 但保留数据和缓存文件：`adb uninstall -k cn.com.test.mobile`
- 列出手机装的所有app 的包名：`adb shell pm list packages`
- 列出除了系统应用的第三方应用包名：`adb shell pm list packages -3`
- 查看 android 版本：`adb shell getprop ro.build.version.release`
- 查看 SDK 版本：`adb shell getprop ro.build.version.sdk`
- 终止adb服务：`adb kill-server`
- 重启adb服务：`adb start-server`
- 清除应用数据与缓存：`adb shell pm clear cn.com.test.mobile`
- 启动应用：`adb shell am start -ncn.com.test.mobile/.ui.SplashActivity`
- 包信息：`adb shell dumpsys package`
- 内存使用情况：`adb shell dumpsys meminfo`
- 强制停止应用：`adb shell am force-stop cn.com.test.mobile`
- 查看日志：`adb logcat`
- 清除 log 缓存：`adb logcat -c`
- 重启：`adb reboot`
- 获取序列号：`adb get-serialno`
- 查看占用内存前10 的app：`adb shell top -s 10`
- 从本地复制文件到设备：`adb push <local> <remote>`
- 从设备复制文件到本地：`adb pull <remote> <local>`
- 查看 bug 报告：`adb bugreport`
- 获取 pageSource：`adb shell uiautomator dump --compressed /sdcard/uidump.xml`
- 拉取文件本本地：`adb pull /sdcard/uidump.xml > ./uidump.xml`

### adb shell

#### 交互 shell

启动：

```shell
$ adb [-d | -e | -s serial_number] shell
```

退出：

```shell
exit
```

#### shell am

```shell
$ adb shell am command
```

#### shell pm

```shell
$ adb shell pm command
```

#### shell dpm

```shell
$ adb shell dpm command
```

### 权限操作

按组列出权限和状态：

```shell
$ adb shell pm list permissions -d -g
```

授予或撤消一项或多项权限：

```shell
$ adb shell pm [grant|revoke] <packageName> <permissionName> ...
```

清理某个 APP 所有数据（包括系统权限）

```shell
$ adb shell pm clear <packageName>
```

### 常用权限

```go
android.permission.READ_CALENDAR // 日历
android.permission.ACCESS_FINE_LOCATION // 定位
android.permission.READ_EXTERNAL_STORAGE // 存储
android.permission.READ_PHONE_STATE // 电话
android.permission.CAMERA // 相机
android.permission.RECORD_AUDIO // 麦克风
android.permission.READ_CONTACTS // 读取联系人
```

### 权限列表

```go
android.permission.SYSTEM_ALERT_WINDOW // 显示系统窗口 
android.permission.ACCESS_CHECKIN_PROPERTIES // 访问登记属性，读取或写入登记check-in数据库属性表的权限 
android.permission.ACCESS_COARSE_LOCATION //获取错略位置，通过WiFi或移动基站的方式获取用户错略的经纬度信息，定位精度大概误差在30~1500米 
android.permission.ACCESS_FINE_LOCATION //获取精确位置，通过GPS芯片接收卫星的定位信息，定位精度达10米以内
android.permission.ACCESS_LOCATION_EXTRA_COMMANDS //访问定位额外命令，允许程序访问额外的定位提供者指令
android.permission.ACCESS_MOCK_LOCATION // 获取模拟定位信息，获取模拟定位信息，一般用于帮助开发者调试应用 
android.permission.ACCESS_NETWORK_STATE // 获取网络状态，获取网络信息状态，如当前的网络连接是否有效 
android.permission.ACCESS_SURFACE_FLINGER // 访问Surface Flinger，Android平台上底层的图形显示支持，一般用于游戏或照相机预览界面和底层模式的屏幕截图 
android.permission.ACCESS_WIFI_STATE // 获取WiFi状态，获取当前WiFi接入的状态以及WLAN热点的信息 
android.permission.ACCOUNT_MANAGER // 账户管理，获取账户验证信息，主要为GMail账户信息，只有系统级进程才能访问的权限 
android.permission.AUTHENTICATE_ACCOUNTS //验证账户，允许一个程序通过账户验证方式访问账户管理ACCOUNT_MANAGER相关信息 
android.permission.BATTERY_STATS //电量统计，获取电池电量统计信息 
android.permission.BIND_APPWIDGET // 绑定小插件，允许一个程序告诉appWidget服务需要访问小插件的数据库，只有非常少的应用才用到此权限 
android.permission.BIND_DEVICE_ADMIN // 绑定设备管理，请求系统管理员接收者receiver，只有系统才能使用 
android.permission.BIND_INPUT_METHOD // 绑定输入法，请求InputMethodService服务，只有系统才能使用 
android.permission.BIND_REMOTEVIEWS // 绑定RemoteView，必须通过RemoteViewsService服务来请求，只有系统才能用 
android.permission.BIND_WALLPAPER // 绑定壁纸，必须通过WallpaperService服务来请求，只有系统才能用 
android.permission.BLUETOOTH //使用蓝牙，允许程序连接配对过的蓝牙设备 
android.permission.BLUETOOTH_ADMIN // 蓝牙管理，允许程序进行发现和配对新的蓝牙设备 
android.permission.BRICK // 禁止手机使用，能够禁用手机，非常危险，顾名思义就是让手机变成砖头 
android.permission.BROADCAST_PACKAGE_REMOVED // 应用删除时广播，当一个应用在删除时触发一个广播 
android.permission.BROADCAST_SMS // 收到短信时广播，当收到短信时触发一个广播 
android.permission.BROADCAST_STICKY // 连续广播，允许一个程序收到广播后快速收到下一个广播 
android.permission.BROADCAST_WAP_PUSH // WAP PUSH广播，WAP PUSH服务收到后触发一个广播 
android.permission.CALL_PHONE // 拨打电话，允许程序从非系统拨号器里输入电话号码 
android.permission.CALL_PRIVILEGED // 通话权限，允许程序拨打电话，替换系统的拨号器界面 
android.permission.CAMERA // 拍照权限，允许访问摄像头进行拍照 
android.permission.CHANGE_COMPONENT_ENABLED_STATE // 改变组件状态，改变组件是否启用状态 
android.permission.CHANGE_CONFIGURATION // 改变配置，允许当前应用改变配置，如定位 
android.permission.CHANGE_NETWORK_STATE // 改变网络状态，改变网络状态如是否能联网 
android.permission.CHANGE_WIFI_MULTICAST_STATE // 改变WiFi多播状态 
android.permission.CHANGE_WIFI_STATE // 改变WiFi状态 
android.permission.CLEAR_APP_CACHE // 清除应用缓存 
android.permission.CLEAR_APP_USER_DATA // 清除应用的用户数据 
android.permission.CWJ_GROUP // 底层访问权限,允许CWJ账户组访问底层信息 
android.permission.CELL_PHONE_MASTER_EX // 手机优化大师扩展权限 
android.permission.CONTROL_LOCATION_UPDATES // 控制定位更新，允许获得移动网络定位信息改变 
android.permission.DELETE_CACHE_FILES // 允许应用删除缓存文件 
android.permission.DELETE_PACKAGES // 允许程序删除应用 
android.permission.DEVICE_POWER // 电源管理，允许访问底层电源管理 
android.permission.DIAGNOSTIC // 应用诊断，允许程序到RW到诊断资源 
android.permission.DISABLE_KEYGUARD // 禁用键盘锁，允许程序禁用键盘锁 
android.permission.DUMP // 转存系统信息，允许程序获取系统dump信息从系统服务 
android.permission.EXPAND_STATUS_BAR // 状态栏控制，允许程序扩展或收缩状态栏 
android.permission.FACTORY_TEST // 工厂测试模式，允许程序运行工厂测试模式 
android.permission.FLASHLIGHT // 允许访问闪光灯 
android.permission.FORCE_BACK // 强制后退，允许程序强制使用back后退按键，无论Activity是否在顶层 
android.permission.GET_ACCOUNTS // 访问账户Gmail列表，访问GMail账户列表 
android.permission.GET_PACKAGE_SIZE // 获取应用大小，获取应用的文件大小 
android.permission.GET_TASKS // 获取任务信息，允许程序获取当前或最近运行的应用 
android.permission.GLOBAL_SEARCH // 允许全局搜索，允许程序使用全局搜索功能 
android.permission.HARDWARE_TEST // 硬件测试，访问硬件辅助设备，用于硬件测试 
android.permission.INJECT_EVENTS // 注射事件，允许访问本程序的底层事件，获取按键、轨迹球的事件流 
android.permission.INSTALL_LOCATION_PROVIDER // 安装定位提供，安装定位提供 
android.permission.INSTALL_PACKAGES // 安装应用程序，允许程序安装应用 
android.permission.INTERNAL_SYSTEM_WINDOW // 内部系统窗口，允许程序打开内部窗口，不对第三方应用程序开放此权限 
android.permission.INTERNET //访问网络连接，可能产生GPRS流量 
android.permission.KILL_BACKGROUND_PROCESSES // 结束后台进程，允许程序调用killBackgroundProcesses(String).方法结束后台进程 
android.permission.MANAGE_ACCOUNTS // 管理账户，允许程序管理AccountManager中的账户列表 
android.permission.MANAGE_APP_TOKENS // 管理程序引用，管理创建、摧毁、Z轴顺序，仅用于系统 
android.permission.MTWEAK_USER // 高级权限，允许mTweak用户访问高级系统权限 
android.permission.MTWEAK_FORUM // 社区权限，允许使用mTweak社区权限 
android.permission.MASTER_CLEAR // 软格式化，允许程序执行软格式化，删除系统配置信息 
android.permission.MODIFY_AUDIO_SETTINGS // 修改声音设置信息 
android.permission.MODIFY_PHONE_STATE // 修改电话状态，如飞行模式，但不包含替换系统拨号器界面 
android.permission.MOUNT_FORMAT_FILESYSTEMS // 格式化可移动文件系统，比如格式化清空SD卡 
android.permission.MOUNT_UNMOUNT_FILESYSTEMS // 挂载、反挂载外部文件系统 
android.permission.NFC // 允许NFC通讯，允许程序执行NFC近距离通讯操作，用于移动支持 
android.permission.PERSISTENT_ACTIVITY // 创建一个永久的Activity，该功能标记为将来将被移除 
android.permission.PROCESS_OUTGOING_CALLS // 处理拨出电话，允许程序监视，修改或放弃播出电话 
android.permission.READ_CALENDAR // 读取日程提醒，允许程序读取用户的日程信息 
android.permission.READ_CONTACTS // 读取联系人，允许应用访问联系人通讯录信息 
android.permission.READ_FRAME_BUFFER // 屏幕截图，读取帧缓存用于屏幕截图 
com.android.browser.permission.READ_HISTORY_BOOKMARKS // 读取收藏夹和历史记录，读取浏览器收藏夹和历史记录 
android.permission.READ_INPUT_STATE // 读取输入状态，读取当前键的输入状态，仅用于系统 
android.permission.READ_LOGS // 读取系统日志，读取系统底层日志 
android.permission.READ_PHONE_STATE // 读取电话状态
android.permission.READ_SMS // 读取短信内容
android.permission.READ_SYNC_SETTINGS // 读取同步设置，读取Google在线同步设置 
android.permission.READ_SYNC_STATS // 读取同步状态，获得Google在线同步状态 
android.permission.REBOOT // 重启设备，允许程序重新启动设备 
android.permission.RECEIVE_BOOT_COMPLETED // 开机自动允许，允许程序开机自动运行 
android.permission.RECEIVE_MMS // 接收彩信 
android.permission.RECEIVE_SMS // 接收短信
android.permission.SEND_SMS // 发送短信
android.permission.RECEIVE_WAP_PUSH // 接收WAP PUSH信息 
android.permission.RECORD_AUDIO // 录音，录制声音通过手机或耳机的麦克 
android.permission.REORDER_TASKS // 排序系统任务，重新排序系统Z轴运行中的任务 
android.permission.RESTART_PACKAGES // 结束系统任务，结束任务通过restartPackage(String)方法，该方式将在外来放弃 
android.permission.SET_ACTIVITY_WATCHER // 设置Activity观察器一般用于monkey测试 
com.android.alarm.permission.SET_ALARM // 设置闹铃提醒 
android.permission.SET_ALWAYS_FINISH // 设置程序在后台是否总是退出 
android.permission.SET_ANIMATION_SCALE // 设置全局动画缩放 
android.permission.SET_DEBUG_APP // 设置调试程序，一般用于开发 
android.permission.SET_ORIENTATION // 设置屏幕方向为横屏或标准方式显示，不用于普通应用 
android.permission.SET_PREFERRED_APPLICATIONS // 设置应用的参数，已不再工作具体查看addPackageToPreferred(String) 介绍 
android.permission.SET_PROCESS_LIMIT // 设置进程限制，允许程序设置最大的进程数量的限制 
android.permission.SET_TIME // 设置系统时间 
android.permission.SET_TIME_ZONE // 设置系统时区 
android.permission.SET_WALLPAPER // 设置桌面壁纸 
android.permission.SET_WALLPAPER_HINTS // 设置壁纸建议 
android.permission.SIGNAL_PERSISTENT_PROCESSES // 发送一个永久的进程信号 
android.permission.STATUS_BAR // 允许程序打开、关闭、禁用状态栏 
android.permission.SUBSCRIBED_FEEDS_READ // 访问订阅内容，访问订阅信息的数据库 
android.permission.SUBSCRIBED_FEEDS_WRITE // 写入订阅内容，写入或修改订阅内容的数据库 
android.permission.UPDATE_DEVICE_STATS // 更新设备状态 
android.permission.USE_CREDENTIALS // 使用证书,允许程序请求验证从AccountManager 
android.permission.USE_SIP // 使用SIP视频，允许程序使用SIP视频服务 
android.permission.VIBRATE // 允许振动 
android.permission.WAKE_LOCK // 唤醒锁定，允许程序在手机屏幕关闭后后台进程仍然运行 
android.permission.WRITE_APN_SETTINGS // 写入网络GPRS接入点设置 
android.permission.WRITE_CALENDAR // 写入日程，但不可读取 
android.permission.WRITE_CONTACTS // 写入联系人，但不可读取 
android.permission.WRITE_EXTERNAL_STORAGE // 允许程序写入外部存储，如SD卡上写文件 
android.permission.WRITE_GSERVICES // 允许程序写入Google Map服务数据 
com.android.browser.permission.WRITE_HISTORY_BOOKMARKS // 写入浏览器历史记录或收藏夹，但不可读取 
android.permission.WRITE_SECURE_SETTINGS // 允许程序读写系统安全敏感的设置项 
android.permission.WRITE_SETTINGS // 允许读写系统设置项 
android.permission.WRITE_SMS // 允许编写短信 
android.permission.WRITE_SYNC_SETTINGS // 写入Google在线同步设置
```

## UI Automator

[UI Automator](https://developer.android.com/training/testing/other-components/ui-automator?hl=zh-cn#java) 是 Google 提供的Android端UI自动化测试库（Java），基于Accessibility服务。可以对第三方App进行测试，获取屏幕上任意一个APP的任意一个控件属性，并对其进行任意操作。

但有两个缺点：1. 测试脚本只能使用Java语言 2. 测试脚本要打包成jar或者apk包上传到设备上才能运行。

因此，有人实现了一个Python库 [xiaocong/uiautomator](https://github.com/xiaocong/uiautomator)，能够直接调用UI Automator的接口，原理是在手机上运行了一个http rpc服务，将uiautomator中的功能开放出来，然后再将这些http接口封装成Python库。

由于 [xiaocong/uiautomator](https://github.com/xiaocong/uiautomator) 长时间没有维护，有人fork了一个版本，解决了一些bug，并新增了一个功能，这个库就是：[uiautomator2](https://github.com/openatx/uiautomator2)

相关文档参考：https://github.com/openatx/uiautomator2

需要在android设备上安装2两个apk包：

```javascript
uiautomator: {
    packageName: 'com.github.uiautomator',
    apkName: 'app-uiautomator.apk'
},
uiautomatorTest: {
    packageName: 'com.github.uiautomator.test',
    apkName: 'app-uiautomator-test.apk'
}
```

- apk包下载地址：https://github.com/openatx/android-uiautomator-server/releases

## atx-agent

屏蔽不同安卓机器的差异，然后开放出统一的HTTP接口供 [openatx/uiautomator2](https://github.com/openatx/uiautomator2)使用。项目最终会发布成一个二进制程序，运行在Android系统的后台。

**atx-agent工作原理？**

PC 端向 Android 机器上安装的 atx-agent 发起 HTTP 请求，atx-agent 向 Android 机器上安装的 app-uiautomator-test.apk中的 AutomatorHttpServer 发起请求，AutomatorHttpServer 根据传递的 method 参数调用 Android 自带的 Uiautomator，并将结果进行返回。

atx-agent pkg 包下载地址：https://github.com/openatx/atx-agent/releases









