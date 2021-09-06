# Linux

## 常用指令

> 日常使用中的一些linux指令的记录

#### 软连接

```
ln -s [被链接文件路径] [指向文件路径]
ln -s /home/work/odp/app/newapp/ /home/work/code/app/newapp	#示例
```

#### 用户操作

```
//查看所有用户
cat /etc/passwd
// 格式化输出所有用户
cat /etc/passwd|grep -v nologin|grep -v halt|grep -v shutdown|awk -F":" '{ print $1"|"$3"|"$4 }'|more
//查看所有用户组
cat /etc/group
// 添加用户
useradd chenxinfang
// 添加用户并关联目录
useradd -d "/home/users/work" work
// 修改用户密码
passwd work
```

##### 添加用户

```shell
useradd 参数 用户名
```

参数：

- -c：指定一段注释性描述。
- -d：指定用户主目录，如果此目录不存在，则同时使用-m选项，可以创建主目录。
- -g：指定用户所属的用户组。
- -G：指定用户所属的附加组。
- -s：指定用户的登录Shell。
- -u：指定用户的用户号，如果同时有-o选项，则可以重复使用其他用户的标识号。

##### 删除用户

```
userdel 参数 用户名
```

参数：

-  -r：把用户的主目录一起删除。

##### 修改用户

```
usermod 参数 用户名
```

参数：

与`useradd`参数类似

##### 密码管理

```
passwd 参数 用户名
```

参数：

- -l：锁定口令，即禁用账号。
- -u：口令解锁。
- -d：使账号无口令。
- -f：强迫用户下次登录时修改口令。

#### grep

```
grep [-abcEFGhHilLnqrsvVwxy][-A<显示列数>][-B<显示列数>][-C<显示列数>][-d<进行动作>][-e<范本样式>][-f<范本文件>][--help][范本样式][文件或目录...]
```

参数：

- **-a 或 --text** : 不要忽略二进制的数据。
- **-A<显示行数> 或 --after-context=<显示行数>** : 除了显示符合范本样式的那一列之外，并显示该行之后的内容。
- **-b 或 --byte-offset** : 在显示符合样式的那一行之前，标示出该行第一个字符的编号。
- **-B<显示行数> 或 --before-context=<显示行数>** : 除了显示符合样式的那一行之外，并显示该行之前的内容。
- **-c 或 --count** : 计算符合样式的列数。
- **-C<显示行数> 或 --context=<显示行数>或-<显示行数>** : 除了显示符合样式的那一行之外，并显示该行之前后的内容。
- **-d <动作> 或 --directories=<动作>** : 当指定要查找的是目录而非文件时，必须使用这项参数，否则grep指令将回报信息并停止动作。
- **-e<范本样式> 或 --regexp=<范本样式>** : 指定字符串做为查找文件内容的样式。
- **-E 或 --extended-regexp** : 将样式为延伸的正则表达式来使用。
- **-f<规则文件> 或 --file=<规则文件>** : 指定规则文件，其内容含有一个或多个规则样式，让grep查找符合规则条件的文件内容，格式为每行一个规则样式。
- **-F 或 --fixed-regexp** : 将样式视为固定字符串的列表。
- **-G 或 --basic-regexp** : 将样式视为普通的表示法来使用。
- **-h 或 --no-filename** : 在显示符合样式的那一行之前，不标示该行所属的文件名称。
- **-H 或 --with-filename** : 在显示符合样式的那一行之前，表示该行所属的文件名称。
- **-i 或 --ignore-case** : 忽略字符大小写的差别。
- **-l 或 --file-with-matches** : 列出文件内容符合指定的样式的文件名称。
- **-L 或 --files-without-match** : 列出文件内容不符合指定的样式的文件名称。
- **-n 或 --line-number** : 在显示符合样式的那一行之前，标示出该行的列数编号。
- **-o 或 --only-matching** : 只显示匹配PATTERN 部分。
- **-q 或 --quiet或--silent** : 不显示任何信息。
- **-r 或 --recursive** : 此参数的效果和指定"-d recurse"参数相同。
- **-s 或 --no-messages** : 不显示错误信息。
- **-v 或 --revert-match** : 显示不包含匹配文本的所有行。
- **-V 或 --version** : 显示版本信息。
- **-w 或 --word-regexp** : 只显示全字符合的列。
- **-x --line-regexp** : 只显示全列符合的列。
- **-y** : 此参数的效果和指定"-i"参数相同。

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

```shell
find   path   -option   [   -print ]   [ -exec   -ok   command ]   {} \;
```

列出所有软连接

```shell
find /home/harris/debug/ -type l -ls 
```

#### du

用于显示指定目录或文件所占用的磁盘空间

**语法：**

```shell
du [-abcDhHklmsSx][-L <符号连接>][-X <文件>][--block-size][--exclude=<目录或文件>][--max-depth=<目录层数>][--help][--version][目录或文件]
```

**参数说明**：

- -a或-all 显示目录中个别文件的大小。
- -b或-bytes 显示目录或文件大小时，以byte为单位。
- -c或--total 除了显示个别目录或文件的大小外，同时也显示所有目录或文件的总和。
- -D或--dereference-args 显示指定符号连接的源文件大小。
- -h或--human-readable 以K，M，G为单位，提高信息的可读性。
- -H或--si 与-h参数相同，但是K，M，G是以1000为换算单位。
- -k或--kilobytes 以1024 bytes为单位。
- -l或--count-links 重复计算硬件连接的文件。
- -L<符号连接>或--dereference<符号连接> 显示选项中所指定符号连接的源文件大小。
- -m或--megabytes 以1MB为单位。
- -s或--summarize 仅显示总计。
- -S或--separate-dirs 显示个别目录的大小时，并不含其子目录的大小。
- -x或--one-file-xystem 以一开始处理时的文件系统为准，若遇上其它不同的文件系统目录则略过。
- -X<文件>或--exclude-from=<文件> 在<文件>指定目录或文件。
- --exclude=<目录或文件> 略过指定的目录或文件。
- --max-depth=<目录层数> 超过指定层数的目录后，予以忽略。
- --help 显示帮助。
- --version 显示版本信息。

**示例：**

```shell
# 查看当前目录总共占的容量
du -sh
```

## shell

Shell 是一个用 C 语言编写的程序，它是用户使用 Linux 的桥梁。Shell 既是一种命令语言，又是一种程序设计语言。

### 类型

Linux 的 Shell 种类众多，常见的有：

- Bourne Shell（/usr/bin/sh或/bin/sh）
- Bourne Again Shell（/bin/bash）
- C Shell（/usr/bin/csh）
- K Shell（/usr/bin/ksh）
- Shell for Root（/sbin/sh）
- ......

### 运行

新建`hello.sh`

```shell
#!/bin/bash
echo "Hello World !"
```

作为可执行程序执行

```
./hello.sh
```

作为解释器参数

```
sh hello.sh
```

### 变量

