# iOS

## usbmux&usbmuxd

- **usbmux**：是苹果的私有协议，苹果设计该协议的原因是为了自家的macOS APP能够和iDevice进行通信，从而实现诸如iTunes备份iPhone、Xcode真机调试等功能
- **usbmuxd**：usbmuxd是对usbmux协议在macOS平台的上实现，也是macOS系统上的一个守护进程（Daemon），它随着系统的启动而启动

参考：https://zyqhi.github.io/2019/08/20/usbmuxd-protocol.html

## xcode_select

管理 xcode 和 BSD 工具的 active developer 目录。

当你使用多个 xcode 开发环境时，可以使用 xcode_select 可以指定 xcode 

### 安装

```shell
# 移除原有的 CommandLineTools，否则可能会报 the tools are already installed
$ sudo rm -rf /Library/Developer/CommandLineTools

# 安装
$ sudo xcode-select --install
```

或者，可以手动下载安装（貌似开发者账号才可以）：

- 下载地址：https://developer.apple.com/download/all/.

### 常用指令

```shell
# 设置 xcode 路径
$ sudo xcode-select -s /Applications/Xcode.app/Contents/Developer

# 查看那 xcode 路径
$ xcode-select -p

# 查看版本
$ xcode-select -v

# 更多指令 - 查看帮助
$ xcode-select -h
```

## xcodebuild

xcodebuild 是 macOS 命令行工具包（Command Line Tools）中的一项，用于构建 xcode project 和 workspace

命令行工具包（Command Line Tools）安装可以参考：[xcode-select](#xcode-select)

#### 常用指令

```shell
# 查看 xcodebuild 用法
$ man xcodebuild
```

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

## Carthage

使用 WDA 依赖此工具

Carthage 使用于 Swift 语言编写，只支持动态框架，只支持 iOS8+ 的 Cocoa 依赖管理工具。

### 安装配置

```shell
# 1. homebrew 安装
$ brew install Carthage

# 2.手动安装
下载安装包安装：https://github.com/Carthage/Carthage/releases

# 安装完成查看版本
$ Carthage version
```

## WebDriverAgent

2015 年  facebook开源的 iOS 端测试框架，目前 facebook 已经不在维护，现在一般推荐使用 fork 自 facebook wda的 appuim wda。

- appium：https://github.com/appium/WebDriverAgent/
- facebook：https://github.com/facebookarchive/WebDriverAgent

### 安装配置

> 参考：https://github.com/facebookarchive/WebDriverAgent/wiki/Starting-WebDriverAgent

拉取 WebDriverAgent.xcodeproj 项目（可以自定义目录）

```shell
$ cd ~ && git clone https://github.com.cnpmjs.org/appium/WebDriverAgent
```

安装 [Carthage](#Carthage) 用于 wda 项目初始化

```shell
$ brew install carthage
```

### 启动

启动 wda 方式很多，如 xcode、xcodebuild、tidevice、FBSimulatorControl 等

#### xcodebuild

使用 [xcode-select](#xcode-select) 配置 xcode 全局路径以在命令行环境使用 xcodebuild：

```shell
$ sudo xcode-select -s /Applications/Xcode.app/Contents/Developer
```

使用 [xcodebuild](xcodebuild) 启动 wda：

```shell
xcodebuild -project WebDriverAgent.xcodeproj \
           -scheme WebDriverAgentRunner \
           -destination 'platform=iOS Simulator,name=iPhone 6' \
           test
```

使用 USB 代理通信，连接 mac 和 iOS 设备：

可以直接安装 [libimobiledevice](#libimobiledevice)，包含了 iProxy、usbmuxd 等工具

```shell
$ brew install libimobiledevice
```

或者参考官方文档：https://github.com/facebookarchive/WebDriverAgent/wiki/USB-support

### API 文档

> 官方：https://github.com/facebookarchive/WebDriverAgent/wiki/Queries



## idb

- 文档：https://fbidb.io/docs/commands
- github：https://github.com/facebook/idb

### 安装配置

#### idb companion

Homebrew 安装：

```shell
$ brew tap facebook/fb
$ brew install idb-companion
```

#### idb client

python版本需要大于 3.6

```shell
$ pip3.6 install fb-idb
```

### 安装踩坑

安装过程可能出现一些问题，下面整理下安装过程遇到的坑。

#### Q1：CocoaPods 版本过低

```shell
[!] The version of CocoaPods used to generate the lockfile (1.11.2) is higher than the version of the current executable (1.9.3). Incompatibility issues may arise.
```

CocoaPods 简单来说就是 iOS 和 macOS 下的一个第三方库管理工具。

##### 解决方案

升级 CocoaPods 版本

```shell
$ sudo gem install cocoapods
```

升级过程还有可能遇到报错：

```shell
ERROR:  Error installing cocoapods:
	ERROR: Failed to build gem native extension.

    current directory: /Library/Ruby/Gems/2.6.0/gems/ffi-1.15.5/ext/ffi_c
/System/Library/Frameworks/Ruby.framework/Versions/2.6/usr/bin/ruby -I /System/Library/Frameworks/Ruby.framework/Versions/2.6/usr/lib/ruby/2.6.0 -r ./siteconf20220418-19053-835qa8.rb extconf.rb
```

由于 CocoaPods 依赖 ruby，这里失败原因是 ruby 版本过低，因此需要升级 ruby 版本：

首先需要安装 rvm：

rvm 是一个命令行工具，可以提供一个便捷的多版本 ruby 环境的管理和切换

```shell
# 安装
$ curl -L get.rvm.io | bash -s stable

# 设置环境变量
$ source ~/.bashrc
$ source ~/.bash_profile
```

很遗憾，安装 rvm 还是可能会出现报错：

```shell
curl: (35) LibreSSL SSL_connect: SSL_ERROR_SYSCALL in connection to raw.githubusercontent.com:443
```

解决方案：安装 redis，然后重复 rvm 安装指令（这里我重新试了两次成功了）

```shell
$ sudo gem install redis
```

rvm 安装成功后安装 ruby 2.6

```shell
$ rvm install ruby-2.6
```

然后，就可以重新执行 CocoaPods 升级指令了。

#### Q2：xcode 版本过低

```shell
Error: A newer Command Line Tools release is available.
Update them from Software Update in System Preferences or run:
  softwareupdate --all --install --force

If that doesn't show you any updates, run:
  sudo rm -rf /Library/Developer/CommandLineTools
  sudo xcode-select --install

Alternatively, manually download them from:
  https://developer.apple.com/download/all/.
You should download the Command Line Tools for Xcode 12.4.
```
