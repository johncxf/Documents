# NMP

Nginx、PHP、Mysql等环境的安装配置

## Nginx

详见：[Nginx](../Server/Nginx.md)

## PHP

### Mac 环境安装配置

#### 安装

Homebrew安装：

homebew中没有php的一些历史版本

```
# 查看所有版本
brew search php
# 安装指定版本php
brew install php@7.3
```

#### php-fpm 配置

php中自带了php-fpm，下面讲解下mac环境php-fpm的配置

修改php-fpm.conf文件中的error_log项，默认该项被注释掉，这里需要去注释并且修改为error_log = /usr/local/var/log/php-fpm.log。如果不修改该值，运行php-fpm的时候会提示log文件输出路径不存在的错误。

```
sudo cp /private/etc/php-fpm.conf.default /private/etc/php-fpm.conf
vim /private/etc/php-fpm.conf
```

启动php-fpm

```
sudo php-fpm
# 干掉php进程()/pkill强制删除php
sudo pkill php-fpm
sudo php-fpm 
```

#### 常见问题

- 启动报错：`WARNING: Nothing matches the include pattern '/private/etc/php-fpm.d/*.conf' from /private/etc/php-fpm.conf at line 143`

  ```
  cd /private/etc/php-fpm.d 
  sudo cp www.conf.default www.conf
  ```