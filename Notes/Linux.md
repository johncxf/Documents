# Linux

## 常用指令

```sh
# 查看域名对应IP
$ nslookup 域名
```

#### 文件上传、下载

```sh
# scp 上传文件
$ scp 「本地文件路径」 用户名@服务器地址:/服务器文件路径

# rz、sz是window和linux之间上传下载文件的工具，需要在linux使用yum进行安装，可以在xshell中使用
# rz 上传文件（在服务器面板中执行，会弹出选择文件窗口，选择完文件即可上传）
$ rz
# sz 下载文件到本地（在服务器面板中执行，会弹出保存路径选择面板）
$ sz ${文件路径}
```

## 常用指令分类

> 日常使用中的一些linux指令的记录

### 磁盘挂载

```shell
# 查看分区情况（所有磁盘）
$ fdisk -l

# 查看磁盘挂载信息（已经挂载）
$ df -Th

# 格式化空磁盘（未挂载）
$ mkfs.ext4 /dev/vdb

# 挂载磁盘到指定目录
$ mount -t ext4 /dev/vdb /home/
```

#### 开机自动挂载

查看磁盘UUID

```shell
$ blkid
```

编辑 `/etc/fatab` 文件，添加如下内容（UUID为上一步获取）：

```
# 设备  挂载点  文件系统类型  挂载选项  转储频度   自检次序
UUID=xxxxxx		/home		ext4		defaults		0		0
```

重启系统或者执行 `mount -a` 生效

#### `/etc/fatab` 文件

- 挂载设备：
  - 设备文件
  - LABEL=
  -  UUID=
- 挂载点：设备的挂载点，就是你要挂载到哪个目录下。
- 文件系统类型：包括 ext2、ext3、ext4、xfs、nfs、smb、iso9660 等
- 挂载选项
  - async/sync：设置是否为同步方式运行，默认为async
  - auto/noauto ：当下载mount -a 的命令时，此文件系统是否被主动挂载。默认为auto
  - rw/ro：是否以以只读或者读写模式挂载
  - exec/noexec：限制此文件系统内是否能够进行"执行"的操作
  - user/nouser：是否允许用户使用mount命令挂载
  - suid/nosuid：是否允许SUID的存在
  - usrquota：启动文件系统支持磁盘配额模式
  - grpquota：启动文件系统对群组磁盘配额模式的支持
  - defaults：同事具有rw,suid,dev,exec,auto,nouser,async等默认参数的设置
- 转储频度
  - 0：从不备份
  - 1：每日备份
  - 2：每隔一天备份
- 自检次序
  - 0: 不自检
  - 1：首先自检，通常只能被/使用；
  - 2：等数字为1的自检完成后，再进行自检

### 软连接

- 创建：`ln -s [源文件/目录路径] [目标文件/目录路径]`
- 修改：`ln -snf [新的源文件/目录路径] [目标文件/目录路径]`
- 删除：`rm -rf 目标文件/目录路径`
- 查看当前目录下所有软链接：`ls -alR | grep ^l`

### 用户操作

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

#### 添加用户

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

#### 删除用户

```
userdel 参数 用户名
```

参数：

-  -r：把用户的主目录一起删除。

#### 修改用户

```
usermod 参数 用户名
```

参数：

与`useradd`参数类似

#### 密码管理

```
passwd 参数 用户名
```

参数：

- -l：锁定口令，即禁用账号。
- -u：口令解锁。
- -d：使账号无口令。
- -f：强迫用户下次登录时修改口令。

### ps

**常用指令：**

```shell
// 查看进程
ps -ef | grep 进程信息
// 批量kill某个服务的所有进程
ps -A | grep -v grep | grep iproxy | awk '{print $1}' | xargs kill
```

**语法：**

```shell
ps [options] [--help]
```

**参数：**

- `-A`：列出所有的进程
- `-w`：显示加宽可以显示较多的资讯
- `-e`： 命令之后显示环境
- `-f`：全部列出，通常和其他选项联用
- `-au`：显示较详细的资讯
- `-aux`：显示所有包含其他使用者的行程

示例：

```shell
// 查找指定进程格式
ps -ef | grep 进程关键字
// 显示所有进程
ps -A
```

### kill

```shell
kill -param option
```

参数：

- `-l`：信息编号，`-l`默认列出所有信息名称
- `-i`：指定要送出的信息
  - 1 (HUP)：重新加载进程。
  - 9 (KILL)：杀死一个进程。
  - 15 (TERM)：正常停止一个进程。

### find

```shell
find   path   -option   [   -print ]   [ -exec   -ok   command ]   {} \;
```

列出所有软连接

```shell
find /home/harris/debug/ -type l -ls 
```

### grep

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

### yum

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

### rpm

```
rpm -qa			  				//列出所有已安装软件
rpm -ql 软件名					  //显示软件安装路径
rpm -qa | grep jenkins 			//列出所有安装的jenkins
rpm -q | grep jenkins			//软件是否安装
rpm -ql jenkins					//列出软件包安装的文件
rpm -qal |grep jenkins 			//查看jenkins所有安装包的文件存储位置
```

### du

用于显示指定目录或文件所占用的磁盘空间

**常用指令：**

```shell
// 以 KB/MB/GB 为单位显示
du -k/-m/-g /home/linux
// 查看当前目录
du -sh dir
// 查看指定层级目录
du -h --max-depth=0 dir
```

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

### df

 用于显示目前在 linux 系统上的文件系统磁盘使用情况统计

**常用：**

```shell
# 查看磁盘挂载信息
$ df -Th
```

**语法：**

```shell
$ df [选项]... [FILE]...
```

**参数说明：**

- 文件-a, --all 包含所有的具有 0 Blocks 的文件系统
- 文件--block-size={SIZE} 使用 {SIZE} 大小的 Blocks
- 文件-h, --human-readable 使用人类可读的格式(预设值是不加这个选项的...)
- 文件-H, --si 很像 -h, 但是用 1000 为单位而不是用 1024
- 文件-i, --inodes 列出 inode 资讯，不列出已使用 block
- 文件-k, --kilobytes 就像是 --block-size=1024
- 文件-l, --local 限制列出的文件结构
- 文件-m, --megabytes 就像 --block-size=1048576
- 文件--no-sync 取得资讯前不 sync (预设值)
- 文件-P, --portability 使用 POSIX 输出格式
- 文件--sync 在取得资讯前 sync
- 文件-t, --type=TYPE 限制列出文件系统的 TYPE
- 文件-T, --print-type 显示文件系统的形式
- 文件-x, --exclude-type=TYPE 限制列出文件系统不要显示 TYPE
- 文件-v (忽略)
- 文件--help 显示这个帮手并且离开
- 文件--version 输出版本资讯并且离开

### fdisk

Linux fdisk是一个创建和维护分区表的程序，它兼容DOS类型的分区表、BSD或者SUN类型的磁盘列表。

常用：

```shell
# 查看未挂载磁盘信息
$ fdisk -l
```

**语法：**

```shell
$ fdisk [必要参数][选择参数]
```

- 必要参数：

  - -l 列出素所有分区表

  - -u 与"-l"搭配使用，显示分区数目

- 选择参数：

  - -s 指定分区

  - -v 版本信息

- 菜单操作说明：

  - m ：显示菜单和帮助信息

  - a ：活动分区标记/引导分区

  - d ：删除分区

  - l ：显示分区类型

  - n ：新建分区

  - p ：显示分区信息

  - q ：退出不保存

  - t ：设置分区号

  - v ：进行分区检查

  - w ：保存修改

  - x ：扩展应用，高级功能

### mount

**常用：**

```shell
$ mount /dev/hda1 /home
```

**语法:**

```
mount [-hV]
mount -a [-fFnrsvw] [-t vfstype]
mount [-fnrsvw] [-o options [,...]] device | dir
mount [-fnrsvw] [-t vfstype] [-o options] device dir
```

**参数说明：**

- -V：显示程序版本
- -h：显示辅助讯息
- -v：显示较讯息，通常和 -f 用来除错。
- -a：将 /etc/fstab 中定义的所有档案系统挂上。
- -F：这个命令通常和 -a 一起使用，它会为每一个 mount 的动作产生一个行程负责执行。在系统需要挂上大量 NFS 档案系统时可以加快挂上的动作。
- -f：通常用在除错的用途。它会使 mount 并不执行实际挂上的动作，而是模拟整个挂上的过程。通常会和 -v 一起使用。
- -n：一般而言，mount 在挂上后会在 /etc/mtab 中写入一笔资料。但在系统中没有可写入档案系统存在的情况下可以用这个选项取消这个动作。
- -s-r：等于 -o ro
- -w：等于 -o rw
- -L：将含有特定标签的硬盘分割挂上。
- -U：将档案分割序号为 的档案系统挂下。-L 和 -U 必须在/proc/partition 这种档案存在时才有意义。
- -t：指定档案系统的型态，通常不必指定。mount 会自动选择正确的型态。
- -o async：打开非同步模式，所有的档案读写动作都会用非同步模式执行。
- -o sync：在同步模式下执行。
- -o atime、-o noatime：当 atime 打开时，系统会在每次读取档案时更新档案的『上一次调用时间』。当我们使用 flash 档案系统时可能会选项把这个选项关闭以减少写入的次数。
- -o auto、-o noauto：打开/关闭自动挂上模式。
- -o defaults:使用预设的选项 rw, suid, dev, exec, auto, nouser, and async.
- -o dev、-o nodev-o exec、-o noexec允许执行档被执行。
- -o suid、-o nosuid：
- 允许执行档在 root 权限下执行。
- -o user、-o nouser：使用者可以执行 mount/umount 的动作。
- -o remount：将一个已经挂下的档案系统重新用不同的方式挂上。例如原先是唯读的系统，现在用可读写的模式重新挂上。
- -o ro：用唯读模式挂上。
- -o rw：用可读写模式挂上。
- -o loop=：使用 loop 模式用来将一个档案当成硬盘分割挂上系统。

### alias

设置指令别名

**语法：**

```shell
alias[别名]=[指令名称]
```

**示例：**

```shell
// 设置
alias ll='ls -l --color=auto'
// 查看所有别名设置
alias -p
```

### netstat

命令用于显示网络状态。

## Shell

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

