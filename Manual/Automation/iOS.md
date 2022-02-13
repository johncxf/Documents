# iOS

## WebDriverAgent

- appium：https://github.com/appium/WebDriverAgent/
- facebook：https://github.com/facebookarchive/WebDriverAgent

## libimobiledevice

官网：https://libimobiledevice.org/

#### 安装

```shell
$ brew install libimobiledevice
```

#### 常用指令

安装`ipa`包

```shell
$ ideviceinstaller -i xxx.ipa
```

查看设备号

```shell
# 查看所有设备号
$ idevice_id -l
```

查看设备信息

```shell
$ ideviceinfo
```

获取设备名称

```shell
$ idevicename
```

获取设备时间

```shell
$ idevicedate
```

更多：https://github.com/libimobiledevice/libimobiledevice#utilities

## idb

- 文档：https://fbidb.io/docs/commands
- github：https://github.com/facebook/idb
