

## Linux常用指令

> 日常使用中的一些linux指令的记录=

### 其他

#### 建立软链接

```
ln -s [被链接文件路径] [指向文件路径]
ln -s /home/work/odp/app/newapp/ /home/work/code/app/newapp	#示例
```

#### MySQL

**源码安装mysql服务管理**

```
//关闭MySQL
$mysql_dir/bin/mysqladmin -uroot -p shutdown
//启动MySQL的命令
$mysql_dir/bin/mysqld_safe &
```

**修改mysql密码**

```
mysql -uroot -p
use mysql;
update user set password=password('root') where user='root' and host='10.64.128.24';
flush privileges;
```

#### 用户

```
//查看所有用户
cat /etc/passwd
//查看所有用户组
cat /etc/group
```



### 分类

#### vi/vim



#### yum

```
yum [options] [command] [package ...]
```

**options：**可选，选项包括-h（帮助），-y（当安装过程提示选择全部为"yes"），-q（不显示安装的过程）等等

- 安装：`yum install <package>`
- 更新：`yum update`
- 更新指定的软件：`yum update <package>`
- 列出所有命令：`yum list`
- 查找：`yum search <keyword>` 
- 卸载：`yum remove php-common`
- 安装包信息：`yum info <package>`

#### rpm

```
rpm -qa			  				//列出所有已安装软件
rpm -ql 软件名					  //显示软件安装路径
rpm -qa | grep jenkins 			//列出所有安装的jenkins
rpm -q | grep jenkins			//软件是否安装
rpm -ql jenkins					//列出软件包安装的文件
rpm -qal |grep jenkins 			//查看jenkins所有安装包的文件存储位置
```

#### find

```
find   path   -option   [   -print ]   [ -exec   -ok   command ]   {} \;
```

列出所有软连接

```
find /home/harris/debug/ -type l -ls 
```

#### which

#### whereis

