# Tools

本文档记录一些软件工具的安装配置以及使用时，已经踩过的坑。

### Composer

> https://www.phpcomposer.com/

#### 安装配置

**mac**

```
# homebrew安装
brew install composer
# 更新
composer self-update
```

### MySQL

#### 安装配置

> 很多安装方式，这里不在讲解
>

#### 开启关闭

**源码安装**

```
//关闭MySQL
$mysql_dir/bin/mysqladmin -uroot -p shutdown
//启动MySQL的命令
$mysql_dir/bin/mysqld_safe &
```

**jumbo**安装

```
sh ${JUMBO_ROOT}/share/mysql/mysql.server start/stop
```

#### 密码重置

在配置文件中 [mysqld] 下添加 `skip-grant-tables`，保存退出（忘记密码情况）
重启 mysql服务
进入mysql `mysql -uroot -p`

执行改密码

```
mysql > use mysql;
mysql >  update user set password=password('root') where user='root';   
mysql > flush privileges;
mysql > exit;
```

在配置文件中 [mysqld] 下删除 `skip-grant-tables`，保存退出

重启 mysql 服务

**注意：**以上只是一种修改mysql密码的方法，而且不适用与高版本mysql

#### 开启远程访问

```
进入 mysql 命令行模式
mysql > use mysql;
mysql > update user set host='%' where host='localhost' and user='root';
mysql > grant all privileges on *.* to 'root'@'localhost' identified by 'root';
mysql > flush privileges;
mysql > exit
```

查看状态

```
ss -tlnp
显示*:3306则可以
```

### samba

#### 安装

```
yum install -y samba samba-client
```

#### 配置

`以下/etc/samba/smb.conf`文件主要配置的讲解

```
# 全局配置
global]
				# 定义工作组
        workgroup = MYGROUP
        # 
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

```
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

#### 本地连接

**mac**

打开`Finder`，按`command+k`（或者点菜单中的 前往->服务器），输入地址（ip/work/）或者`ip`，使用`work`帐户登录，链接之后在命令行里的路径是：`/home/work/`

**win**

使用运行命令(`win+R`)，输入`//IP`可打开，也可以打开`我的电脑`右击选择`添加一个网络位置`，输入`//IP/work`可映射到本地为一个盘符

### GitLab

#### SSH

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

#### 防火墙

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

#### 邮件服务

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

#### 下载安装

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

#### 其他

查看已经开放的端口：

```
firewall-cmd --list-ports
```

开启端口

```
firewall-cmd --zone=public --add-port=80/tcp --permanent
```

#### 邮件发送

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

### crontab

> 定时任务

### IDE

#### sublim

##### 安装

官网下载安装：https://www.sublimetext.com/

##### 包管理

sublime 可以通过扩展包来丰富编码功能，扩展插件网站：https://packagecontrol.io/

###### 安装

官网安装教程：https://packagecontrol.io/installation

通过快捷键组合`ctrl+shift+P`唤出命令面板

在面板中输入`install package`后回车

##### 插件

###### 插件安装

`ctrl+shift+P`唤出命令面板输入相关插件命搜索安装即可

###### 常用插件

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

#### PHPStorm

> 参考博客：https://blog.yiqiesuifeng.cn/archives/165/

#### webStorm

##### 插件推荐

- Material Theme UI：主题插件
- CodeGlance：右侧显示当前文件中代码的缩略图
- GitToolBox：光标`focus`某行时,显示git纪录
- .ignore：自动生成`.gitignore`文件
- IdeaVim：支持vim语法

#### VSCode

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



