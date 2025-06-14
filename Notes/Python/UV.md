# UV

- https://lzw.me/doc/uv-zh-cn/getting-started/installation/
- https://docs.astral.sh/uv/#installation

## 安装

```sh
# curl 下载脚本安装
$ curl -LsSf https://astral.sh/uv/install.sh | sh

# homebrew
$ brew install uv
```

## 使用

### 项目

#### 创建项目

```sh
# 最基础的应用
$ uv init example-app

# 需要打包发布的应用
$ uv init --package example-pkg
```

#### 依赖管理

```sh
# 添加依赖
$ uv add httpx

# 删除依赖
$ uv remove httpx

# 安装指导版本依赖
$ uv add httpx==版本号

# 添加开发依赖
$ uv add --dev pytest
```

### 安装 Python

```sh
# 查看系统中python版本
$ uv python list

# 虚拟环境使用指定版本 python
$ uv venv --python 3.12

# 激活虚拟环境
source .venv/bin/activate
```

