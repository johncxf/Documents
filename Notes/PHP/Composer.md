# Composer

Composer 是 PHP 的一个依赖管理工具

- 官网：https://www.phpcomposer.com/

## 安装

### MasOS

#### Homebrew 安装

```sh
$ brew install composer
```

#### 更新

```sh
$ composer selfupdate
$ composer self-update
```

#### 切换镜像

```sh
# 全局配置
$ composer config -g repo.packagist composer https://mirrors.aliyun.com/composer/
# 全局取消
$ composer config -g --unset repos.packagist

# 配置
$ composer config repo.packagist composer https://mirrors.aliyun.com/composer/
# 取消
$ composer config --unset repos.packagist
```

## 常用指令

```sh
# 安装
$ composer install

# 安装 require-dev 中列出的包
$ composer install --dev

# 跳过 require-dev 字段中列出的包
$ composer install --no-dev

# 安装项目(以 laravel 为例)
$ composer create-project laravel/laravel example-app

# 安装软件包
$ composer require ${package}

# 安装指定版本的软件包
$ composer require "${package}:^3.1"

#安装 MASTER 分支的代码
$ composer require ${package}:dev-master

# 查看所有安装的软件包
$ composer show

# 查看安装的指定软件包的详细信息
$ composer show ${package}

# 更新所有扩展包
$ composer update

# 更新指令的软件包
$ composer update ${package}

# 删除软件包
$ composer remove ${package}

# 搜索软件包
$  composer search ${package}

# 查看已经安装扩展包信息与版本
$ composer show -i

# 查看已经安装扩展包信息与版本
$ composer config -l -g
```


