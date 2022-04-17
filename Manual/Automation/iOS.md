# iOS

## WebDriverAgent

2015 年  facebook开源的 iOS 端测试框架，目前 facebook 已经不在维护，现在一般推荐使用 fork 自 facebook wda的 appuim wda。

- appium：https://github.com/appium/WebDriverAgent/
- facebook：https://github.com/facebookarchive/WebDriverAgent

## usbmux&usbmuxd

- **usbmux**：是苹果的私有协议，苹果设计该协议的原因是为了自家的macOS APP能够和iDevice进行通信，从而实现诸如iTunes备份iPhone、Xcode真机调试等功能
- **usbmuxd**：usbmuxd是对usbmux协议在macOS平台的上实现，也是macOS系统上的一个守护进程（Daemon），它随着系统的启动而启动

参考：https://zyqhi.github.io/2019/08/20/usbmuxd-protocol.html

## libimobiledevice

iOS 设备操控工具，支持对 iOS 设备进行安装卸载应用，截屏，获取设备信息等。

- 官网：https://libimobiledevice.org/
- github：https://github.com/libimobiledevice/libimobiledevice

#### 安装

```shell
$ brew install libimobiledevice
```

#### 常用指令

```shell
# 查看已连接的所有设备号
$ idevice_id -l

# 安装 ipa 包
$ ideviceinstaller -i xxx.ipa

# 查看设备信息
$ ideviceinfo

#获取设备名称
$ idevicename

# 获取设备时间
$ idevicedate
```

更多：https://github.com/libimobiledevice/libimobiledevice#utilities

## idb

- 文档：https://fbidb.io/docs/commands
- github：https://github.com/facebook/idb

### 安装

