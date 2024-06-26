# Electron

Electron是一个使用 JavaScript、HTML 和 CSS 构建桌面应用程序的框架

- 官方文档：https://www.electronjs.org/zh/docs/latest/

## 安装配置

建议安装或切换最新 nodeJS 版本，低版本安装 election 可能会失败

- nodeJS
- npm

### 创建应用

创建应用目录并初始化环境

```shell
// 新建应用目录
$ mkdir electron_practice && cd electron_practice
// 初始化 npm
$ npm init
```

安装 election

```shell
// 安装最新版并打印安装进展
$ npm install electron --save-dev --verbose
```

### 打包分发

将 Electron Forge 添加到您应用的开发依赖中，并使用其"import"命令设置 Forge 的脚手架

```shell
$ npm install --save-dev @electron-forge/cli
$ npx electron-forge import
```

使用 Forge 的 `make` 命令来创建可分发的应用程序

```shell
$ npm run make
```

Electron-forge 会创建 `out` 文件夹，软件包将在那里找到

## 应用调试

#### 主进程调试

终端执行

```shell
# 格式
electron --inspect=5858 your/app

# 示例：
./node_modules/.bin/electron --inspect .
./node_modules/.bin/electron --inspect=5858 .
```

chrome 浏览器输入：`chrome://inspect` 打开，选择对应对话连接

#### 渲染进程调试

和 chrome 调试一样，快捷键打开 devtools 进行调试

也可以在主进程启动后自动打开 devtools，在 `main.js` 的 `mainWindow.loadFile` 代码后加一行：

```javascript
mainWindow.webContents.openDevTools();
```











