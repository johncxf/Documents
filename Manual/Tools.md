# Tools

## Mac

#### 设置快捷命令启动 APP

```shell
# 修改配置，zsh 为例（bash 修改 ~/.bash_profile）
vim ～/.zshrc

# 添加需要快捷打开的应用路径，以 typora 和 sublim 为例
alias typ='open -a /Applications/Typora.app'
alias subl='open -a /Applications/Sublime\ Text.app'

# 生效
source ～/.zshrc
```

#### 开启合盖不睡眠

禁用Lid-Sleep的命令（保持系统唤醒）

```sh
$ sudo pmset -b sleep 0; sudo pmset -b disablesleep 1
```

激活Lid-Sleep的命令（让系统像正常一样再次进入睡眠状态）

```sh
$ sudo pmset -b sleep 5; sudo pmset -b disablesleep 0
```

## Phpmyadmin

官网：https://www.phpmyadmin.net

### 安装

- mac：

  ```shell
  brew search phpmyadmin
  ```


- 或者官网下载安装


### 配置

修改`nginx`配置文件：

```nginx
location /pma {
    alias /usr/local/share/phpmyadmin/;
    try_files $uri $uri/ /index.php;
    disable_symlinks off;
    index index.php;

    location ~ ^/pma(.+\.php)$ {
        alias /usr/local/share/phpmyadmin$1;
    		fastcgi_pass 127.0.0.1:9000;
    		include fastcgi_params;
    		fastcgi_param SCRIPT_FILENAME /usr/local/share/phpmyadmin$1;
    		fastcgi_intercept_errors        on;
    }
}
```

## Samba

### 安装

```shell
yum install -y samba samba-client
```

### 配置

`以下/etc/samba/smb.conf`文件主要配置的讲解

```yaml
# 全局配置
global]
		# 定义工作组
        workgroup = MYGROUP
        server string = Samba Server Version %v
        # samba的安全等级，有以下四种
        # 1.share：用户不需要账户及密码即可登录samba服务器
				# 2.user：由提供服务的samba服务器负责检查账户及密码（默认）
				# 3.server：检查账户及密码的工作由另一台windows或samba服务器负责
				#	4.domain：指定windows域控制服务器来验证用户的账户及密码。
        security = user
        # 用户后台，有以下三种
        # 1.smbpasswd：该方式是使用smb工具smbpasswd给系统用户（真实用户或者虚拟用户）设置一个Samba 密码，客户端就用此密码访问Samba资源。smbpasswd在/etc/samba中，有时需要手工创建该文件。
        # 2.tdbsam：使用数据库文件创建用户数据库。数据库文件叫passdb.tdb，在/etc/samba中。passdb.tdb用户数据库可使用smbpasswd -a work创建Samba用户，要创建的Samba用户必须先是系统用户。也可使用pdbedit创建Samba账户。pdbedit参数很多，列出几个主要的：
            # pdbedit –a username：新建Samba账户。
            # pdbedit –x username：删除Samba账户。
            # pdbedit –L：列出Samba用户列表，读取passdb.tdb数据库文件。
            # pdbedit –Lv：列出Samba用户列表详细信息。
            # pdbedit –c “[D]” –u username：暂停该Samba用户账号。
            # pdbedit –c “[]” –u username：恢复该Samba用户账号。
				#3.ldapsam：基于LDAP账户管理方式验证用户。首先要建立LDAP服务，设置“passdb backend = ldapsam:ldap://LDAP Server”
        passdb backend = tdbsam
        # load printers 和 cups options 两个参数用来设置打印机相关
        load printers = yes
        cups options = raw
        # 以下配置可以了解
        netbios name = MYSERVER  # 设置出现在“网上邻居”中的主机名
				hosts allow = 127.192.168.12.  192.168.13. # 用来设置允许的主机，如果在前面加”;”则表示允许所有主机
				log file = /var/log/samba/%m.log #定义samba的日志，这里的%m是上面的netbios name
				max log size = 50 # 指定日志的最大容量，单位是K
# 该部分内容共享用户自己的家目录，也就是说，当用户登录到samba服务器上时实际上是进入到了该用户的家目录，用户登陆后，共享名不是homes而是用户自己的标识符，对于单纯的文件共享的环境来说，这部分可以注视掉。
[homes]
        comment = Home Directories
        browseable = no
        writable = yes
# 该部分内容设置打印机共享
[printers]
        comment = All Printers
        path = /var/spool/samba
        browseable = no
        guest ok = no
        writable = no
        printable = yes
```

在配置文件中加入

```yaml
# 这下是配置work
[work]
   comment = home work
   path = /home/work
   public = no
   browseable = yes
   writable = yes
   valid users = work
```

再执行`smbpasswd -a work`添加帐户到`samba`

### 启动

```shell
/etc/init.d/smb start
/etc/init.d/smb stop
/etc/init.d/smb restart
```

### 连接

**mac**

打开`Finder`，按`command+k`（或者点菜单中的 前往->服务器），输入地址（ip/work/）或者`ip`，使用`work`帐户登录，链接之后在命令行里的路径是：`/home/work/`

**win**

使用运行命令(`win+R`)，输入`//IP`可打开，也可以打开`我的电脑`右击选择`添加一个网络位置`，输入`//IP/work`可映射到本地为一个盘符

## GitLab

### SSH

安装ssh，已经安装可以跳过

```
sudo yum install -y curl policycoreutils-python openssh-server
```

设置ssh开机自启动

```
sudo systemctl enable sshd
```

启动ssh

```
sudo systemctl start sshd
```

### 防火墙

> 设置防火墙，为了开放HTTP的端口 

安装防火墙，已经安装则可以跳过

```
yum install firewalld systemd -y
```

开启防火墙

```
service firewalld  start 
```

添加http服务到firewalld,pemmanent表示永久生效，若不加--permanent系统下次启动后就会失效

```
sudo firewall-cmd --permanent --add-service=http
```

重启防火墙

```
sudo systemctl reload firewalld
```

### 邮件服务

安装邮件服务，当gitlab想要通过邮件通知，也可以另外配置其它的邮件服务器

```
sudo yum install postfix
```

将postfix服务设置成开机自启动

```
sudo systemctl enable postfix
```

启动postfix

```
sudo systemctl start postfix
```

注：**启动的时候如果遇到以下错误

```
Job for postfix.service failed because the control process exited with error code. See "systemctl status postfix.service" and "journalctl -xe" for details.
```

则修改`/etc/postfix/main.cf`文件

```
inet_protocols = ipv4
inet_interfaces = all
```

### 下载安装

下载gitlab镜像

```
wget https://mirrors.tuna.tsinghua.edu.cn/gitlab-ce/yum/el7/gitlab-ce-10.0.0-ce.0.el7.x86_64.rpm
```

安装

```
rpm -i gitlab-ce-10.0.0-ce.0.el7.x86_64.rpm
```

修改gitlab配置文件`/etc/gitlab/gitlab.rb`指定服务器ip和自定义端口：

>  https://docs.gitlab.com/omnibus/settings/configuration.html#configuring-the-external-url-for-gitlab 

```
external_url 'http://xxx.xx.xxx.xxx:端口'
```

 运行`sudo gitlab-ctl reconfigure`以使更改生效并重启

```
gitlab-ctl reconfigure
gitlab-ctl restart
```

### 其他

查看已经开放的端口：

```
firewall-cmd --list-ports
```

开启端口

```
firewall-cmd --zone=public --add-port=80/tcp --permanent
```

### 邮件发送

配置

修改gitlab的配置文件：`/etc/gitlab/gitlab.rb`

```
 gitlab_rails['smtp_enable'] = true
 gitlab_rails['smtp_address'] = "smtp.qq.com"
 gitlab_rails['smtp_port'] = 465
 gitlab_rails['smtp_user_name'] = "******@qq.com"
 gitlab_rails['smtp_password'] = "授权码"
 gitlab_rails['smtp_domain'] = "smtp.qq.com"
 gitlab_rails['smtp_authentication'] = "login"
 gitlab_rails['smtp_enable_starttls_auto'] = true
 gitlab_rails['smtp_tls'] = true
 gitlab_rails['gitlab_email_from'] = '******@qq.com'
```

重启

```
gitlab-ctl reconfigure
gitlab-ctl restart
```

 测试

执行 `gitlab-rails console`进入控制台。 然后在控制台提示符后输入下面的命令 发送一封测试邮件：`Notify.test_email('收件人邮箱', '邮件标题', '邮件正文').deliver_now` 

## IDE

### Sublim

#### 安装

官网下载安装：https://www.sublimetext.com/

#### 包管理

sublime 可以通过扩展包来丰富编码功能，扩展插件网站：https://packagecontrol.io/

##### 安装

官网安装教程：https://packagecontrol.io/installation

通过快捷键组合`ctrl+shift+P`唤出命令面板

在面板中输入`install package`后回车

#### 插件

##### 插件安装

`ctrl+shift+P`唤出命令面板输入相关插件命搜索安装即可

##### 常用插件

- emmet：快速编写html/css https://docs.emmet.io

- docblockr：代码注释提示插件 https://packagecontrol.io/packages/DocBlockr
- sideBarEnhancements ：扩展左侧面板 https://packagecontrol.io/packages/SideBarEnhancements
- SideBarTools：扩展左侧面板 https://packagecontrol.io/packages/SideBarTools
- AdvancedNewFile：快速创建文件 ，使用ctrl+alt+n就可以快速创建文件
- Local History：本地历史记录 https://packagecontrol.io/packages/Local%20History
- Laravel 5 Artisan：laravel命令行插件 https://packagecontrol.io/packages/Laravel%205%20Artisan 按 ctrol+shift+p 搜索并执行命令
- Laravel 5 Snippets：laravel代码片段 https://packagecontrol.io/packages/Laravel%205%20Snippets
- Laravel Blade Highlighter：laravel blade模板高亮 https://packagecontrol.io/packages/Laravel%20Blade%20Highlighter
- Blade Snippets：blade 代码片段 https://packagecontrol.io/packages/Blade%20Snippets
- Laravel Blade AutoComplete：自动识别blade父级模板内容 https://packagecontrol.io/packages/Laravel%20Blade%20AutoComplete
- Bootstrap 3 Snippets：bootstrap 代码片段 https://github.com/JasonMortonNZ/bs3-sublime-plugin
- Bootstrap 3 Autocomplete：bootstrap样式提示 https://packagecontrol.io/packages/Bootstrap%203%20Autocomplete
- Bootstrap 4 Snippets：bootstrap代码提示 https://packagecontrol.io/packages/Bootstrap%204%20Snippets
- Bootstrap 4 Autocomplete：bootstrap4 自动提示：https://packagecontrol.io/packages/Bootstrap%204%20Autocomplete
- SCSS：scss代码提示 https://packagecontrol.io/packages/SCSS
- Sass：scss语法高亮 https://packagecontrol.io/packages/Sass

### PHPStorm

> 参考博客：https://blog.yiqiesuifeng.cn/archives/165/

### WebStorm

##### 插件推荐

- Material Theme UI：主题插件
- CodeGlance：右侧显示当前文件中代码的缩略图
- GitToolBox：光标`focus`某行时,显示git纪录
- .ignore：自动生成`.gitignore`文件
- IdeaVim：支持vim语法

### VSCode

##### 插件推荐

- chinese：简体中文
- vscode-icons：目录图标
- Vscode-fesc-plugin：代码规范
- Settings Sync：同步配置
- Git History：git历史记录
- GitLens—Git surpercharged：git提交记录
- koroFileHeader：生成头文件
- one Dark Pro：推荐主题
- miniapp：微信小程序
- vscode-swan：百度智能小程序高亮与补全插件
- Bracket Pair Colorzer：括号匹配
- Eslint：Eslint语法
- Open in Brower：浏览器打开
- php
- python
- Path Autocomplete
- open in brower
- ...

## Homebrew

官网：https://brew.sh/

### 安装配置

可以安装官网方式进行安装，不过国内安装多数情况下都会安装失败

解决方案：更换镜像安装

#### 安装

```shell
export HOMEBREW_BREW_GIT_REMOTE="https://mirrors.ustc.edu.cn/brew.git"
export HOMEBREW_CORE_GIT_REMOTE="https://mirrors.ustc.edu.cn/homebrew-core.git"
/bin/bash -c "$(curl -fsSL https://gitee.com/ineo6/homebrew-install/raw/master/install.sh)"
```

#### 配置

```shell
# 对于 bash 用户：
echo 'export HOMEBREW_BOTTLE_DOMAIN=https://mirrors.ustc.edu.cn/homebrew-bottles' >> ~/.bash_profile
source ~/.bash_profile
# 对于 zsh 用户：
echo 'export HOMEBREW_BOTTLE_DOMAIN=https://mirrors.ustc.edu.cn/homebrew-bottles' >> ~/.zshrc
source ~/.zshrc
```

参考：https://brew.idayer.com/guide/change-source/

#### 切换镜像源

查看镜像源：

```shell
git -C "$(brew --repo)" remote get-url origin
git -C "$(brew --repo homebrew/core)" remote get-url origin
git -C "$(brew --repo homebrew/cask)" remote get-url origin
```

修改镜像源：

```shell
# 修改配置
git -C "$(brew --repo)" remote set-url origin 'https://github.com/Homebrew/brew.git'
git -C "$(brew --repo homebrew/core)" remote set-url origin 'https://github.com/Homebrew/homebrew-core.git'
git -C "$(brew --repo homebrew/cask)" remote set-url origin 'https://github.com/Homebrew/homebrew-cask.git'

# 替换后更新
brew update

# 替换 Homebrew-bottles
# 对于 bash 用户：
echo 'export HOMEBREW_BOTTLE_DOMAIN=https://mirrors.ustc.edu.cn/homebrew-bottles' >> ~/.bash_profile
source ~/.bash_profile
# 对于 zsh 用户：
echo 'export HOMEBREW_BOTTLE_DOMAIN=https://mirrors.ustc.edu.cn/homebrew-bottles' >> ~/.zshrc
source ~/.zshrc
```

- 官方：`https://github.com/Homebrew/`
- 中科大：`https://mirrors.ustc.edu.cn/`
- 清华：`https://mirrors.tuna.tsinghua.edu.cn/`

#### 关闭自动更新

```sh
# 临时关闭
$ export HOMEBREW_NO_AUTO_UPDATE=true

# 永久关闭
$ vim ~/.zshrc
# 添加配置
export HOMEBREW_NO_AUTO_UPDATE=true
# 生效
$ source ~/.zshrc
```

### 常用指令

```sh
# 查看帮助
$ brew --help
# 安装
$ brew install xxx
# 卸载
$ brew uninstall xxx
# 查找已安装软件列表
$ brew list
# 查看软件安装目录
$ brew list xxx
```

### FQA

安装软件报错

```sh
Warning: formula.jws.json: update failed, falling back to cached version.
```

解决方法：

```sh
$ export HOMEBREW_NO_INSTALL_FROM_API=1
```

## PM2

PM2是一个守护进程管理器，它将帮助您全天候管理和保持应用程序在线。

- 官网：https://pm2.keymetrics.io/

### 安装

```shell
npm install pm2 -g
```

### 常用指令

```shell
# 启动服务
pm2 start app.js
# 列出所有 PM2 管理的所有应用
pm2 list
# 显示应用进程详细信息
pm2 describe ${id}
# 删除应用进程
pm2 delete ${id}
# 停止应用进程
pm2 stop ${id}
# 重启应用进程
pm2 restart ${id}
```

更多：https://pm2.keymetrics.io/docs/usage/quick-start/

## NVM

nvm 是 node.js 版本管理工具

 - github：https://github.com/nvm-sh/nvm

### MAC 安装配置
#### 卸载
没有安装过`node`的跳过此步骤

```shell
$ sudo npm uninstall npm -g
$ sudo rm -rf /usr/local/lib/node /usr/local/lib/node_modules /var/db/receipts/org.nodejs.*
$ sudo rm -rf /usr/local/include/node /Users/$USER/.npm
$ sudo rm /usr/local/bin/node
```

#### 安装

##### 自动安装

```shell
// 安装
$ curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.8/install.sh | bash

// 或者 wget 安装
$ wget -qO- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.1/install.sh | bash
```

##### 手动安装（如果自动安装失败，可以尝试手动安装）

```shell
// 拉取代码
cd ~/ && git clone https://github.com/nvm-sh/nvm.git .nvm && cd ~/.nvm
// 切换指定版本
git checkout v0.39.1
// 安装
. ./nvm.sh
```

### Window 安装

下载 win 版本安装包进行安装

- https://github.com/coreybutler/nvm-windows/releases

### 常用指令

可以通过`nvm --help`查看所有指令

```shell
nvm --help                          显示所有信息
nvm --version                       显示当前安装的nvm版本
nvm install [-s] <version>          安装指定的版本，如果不存在.nvmrc,就从指定的资源下载安装
nvm install [-s] <version>  -latest-npm 安装指定的版本，平且下载最新的npm
nvm uninstall <version>             卸载指定的版本
nvm use [--silent] <version>        使用已经安装的版本  切换版本
nvm current                         查看当前使用的node版本
nvm ls                              查看已经安装的版本
nvm ls <version>                    查看指定版本
nvm ls-remote                       显示远程所有可以安装的nodejs版本
nvm ls-remote --lts                 查看长期支持的版本
nvm install --latest-npm            安装最新的npm
nvm reinstall-packages <version>    重新安装指定的版本
nvm cache dir                       显示nvm的cache
nvm cache clear                     清空nvm的cache
nvm alias default <version>			使用默认版本
```

## Gitbook

- 官网：https://www.gitbook.com/#PersonalNotes
- npm：https://www.npmjs.com/package/gitbook-cli

### 安装配置

安装 node.js（安装方式）

- gitbook 貌似对高版本 node 不兼容，亲测 node14.0 不行，node12.0 可以

- 在 [Node.js 历史版本下载网站](https://nodejs.org/zh-cn/download/releases/) 下载合适版本 pkg 安装包，直接点击安装即可。

gitbook 命令行工具使用 npm 进行安装（依赖 node.js 环境）：

```shell
$ npm install -g gitbook-cli
```

检查是否安装成功：

```shell
$ gitbook -V
```

### 创建一本书

#### 初始化

自定义项目目录，在目录中执行以下指令：

```shell
$ gitbook init
```

初始化会生成 `README.md` 、 `SUMMARY.md` 两个文件

#### 安装依赖

```sh
$ gitbook install
```

#### 预览

```shell
$ gitbook serve
```

#### 构建

```shell
$ gitbook build
```

构建完成会生成 `_book` 静态资源目录文件

### 目录结构

```json
.
├── book.json 	// 配置文件
├── README.md 	// 首页文档
├── SUMMARY.md 	// 目录导航
├── chapter1		// 章节分类
|   ├── README.md
|   └── something.md
└── chapter2
    ├── README.md
    └── something.md
```

#### README.md

首页文档

#### SUMMARY.md

目录导航，编写格式如下：

```markdown
# Summary
- [Introduction](README.md)
- [Git](git/README.md)
  - [安装配置](git/install.md)
  - [指令全集](git/commend.md)
- [Vim](vim/README.md)
  - [启动指令](vim/start.md)
  - [常用指令](vim/command.md)
```

部分内容对应目录：http://note.yiqiesuifeng.cn/

#### book.json

配置文件，配置如下：

| 变量            | 描述                                       |
| :------------ | :--------------------------------------- |
| root          | 包含所有图书文件的根文件夹的路径，除了 book.json            |
| structure     | 指定自述文件，摘要，词汇表等的路径                        |
| title         | 您的书名，默认值是从 README 中提取出来的。在 GitBook.com 上，这个字段是预填的。 |
| description   | 您的书籍的描述，默认值是从 README 中提取出来的。在 GitBook.com 上，这个字段是预填的。 |
| author        | 作者名。在GitBook.com上，这个字段是预填的。              |
| isbn          | 国际标准书号 ISBN                              |
| language      | 本书的语言类型 —— [ISO code](https://links.jianshu.com/go?to=https%3A%2F%2Fen.wikipedia.org%2Fwiki%2FList_of_ISO_639-1_codes) 。默认值是 `en` |
| direction     | 文本阅读顺序。可以是 rtl （从右向左）或 ltr （从左向右），默认值依赖于 language 的值。 |
| gitbook       | 应该使用的GitBook版本，并接受类似于 `>=3.0.0` 的条件。     |
| links         | 在左侧导航栏添加链接信息                             |
| plugins       | 要加载的插件列表([官网插件列表](https://links.jianshu.com/go?to=https%3A%2F%2Fdocs.gitbook.com%2Fv2-changes%2Fimportant-differences%23plugins)) |
| pluginsConfig | 插件的配置                                    |

### 常用指令

```shell
// 列出 gitbook 所有的命令
gitbook help
// 输出 gitbook-cli 的帮助信息
gitbook --help
// 生成静态网页
gitbook build
// 生成静态网页并运行服务器
gitbook serve
// 生成时指定gitbook的版本, 本地没有会先下载
gitbook build --gitbook=2.0.1
// 列出本地所有的gitbook版本
gitbook ls
// 列出远程可用的gitbook版本
gitbook ls-remote
// 安装对应的gitbook版本
gitbook fetch 标签/版本号
// 更新到gitbook的最新版本
gitbook update
// 卸载对应的gitbook版本
gitbook uninstall 2.0.1
// 指定log的级别
gitbook build --log=debug
// 输出错误信息
gitbook builid --debug
```

### FAQ

#### Q：`gitbook init` 报错执行报错：

```shell
warn: no summary file in this book
info: create README.md
info: create SUMMARY.md

TypeError [ERR_INVALID_ARG_TYPE]: The "data" argument must be of type string or an instance of Buffer, TypedArray, or DataView. Received an instance of Promise
```

解决方法：node.js  版本过高，将 node.js 版本降到 12.0

## Jenkins

持续集成工具

- 官网：https://www.jenkins.io/
- https://www.k8stech.net/jenkins-docs/

### 安装配置

#### 环境依赖

JDK 版本：11 ～ 17

#### 下载安装

官网下载 war 包：https://www.jenkins.io/download/

#### 启动配置

命令行执行：

```sh
$ java -jar jenkins.war

# 指定端口
$ java -jar jenkins.war --httpPort=8088
```

第一次启动控制台会输出一个 token，复制 token

在浏览器打开：http://localhost:8080/，输入 token

安装指引创建用户、安装插件

安装完成后即可进入首页

### Pipeline
