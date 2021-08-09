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

### 开启adb调试

如要在通过 USB 连接的设备上使用 adb，您必须在设备的系统设置中启用 **USB 调试**（位于**开发者选项**下）

### 指令

#### 常用指令集

```shell
#查看连接设备
adb devices
#查看ADB 帮助
adb help
# 指定连接设备使用命令
adb -s ${devices_id} shell
# 安装应用
adb install test.apk
#卸载应用，需要指定包
adb uninstall cn.com.test.mobile 
#卸载app 但保留数据和缓存文件
adb uninstall -k cn.com.test.mobile
#列出手机装的所有app 的包名
adb shell pm list packages
#列出除了系统应用的第三方应用包名
adb shell pm list packages -3
#清除应用数据与缓存
adb shell pm clear cn.com.test.mobile
#启动应用
adb shell am start -ncn.com.test.mobile/.ui.SplashActivity
#包信息Package Information
adb shell dumpsys package
#内存使用情况Memory Usage
adb shell dumpsys meminfo
#强制停止应用
adb shell am force-stop cn.com.test.mobile
#查看日志
adb logcat
#清除log 缓存
adb logcat -c
#重启
adb reboot
#获取序列号
adb get-serialno
#查看Android 系统版本
adb shell getprop ro.build.version.release
#查看占用内存前10 的app
adb shell top -s 10
#从本地复制文件到设备
adb push <local> <remote>
#从设备复制文件到本地
adb pull <remote> <local>
#查看 bug 报告
adb bugreport
# 查看 android 版本
adb shell getprop ro.build.version.release
# 查看 SDK 版本
adb shell getprop ro.build.version.sdk
```

#### adb shell

##### 交互 shell

启动：

```shell
adb [-d | -e | -s serial_number] shell
```

退出：

```shell
exit
```

##### shell am

```shell
adb shell am command
```

##### shell pm

```
adb shell pm command
```

##### shell dpm

```
adb shell dpm command
```





