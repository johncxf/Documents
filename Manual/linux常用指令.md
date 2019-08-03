##### 卸载通过yum安装的php

```
yum remove php-common
```

##### 查看服务器已经安装的软件

###### rpm查看

```
rpm -qa			  				//列出所有已安装软件
rpm -ql 软件名					  //显示软件安装路径
rpm -qa | grep jenkins 			//列出所有安装的jenkins
rpm -q | grep jenkins			//软件是否安装
rpm -ql jenkins					//列出软件包安装的文件
rpm -qal |grep jenkins 			//查看jenkins所有安装包的文件存储位置
```

###### yum查询

```
yum info package1      //显示安装包信息package1
yum list               //显示所有已经安装和可以安装的程序包
yum list package1      //显示指定程序包安装情况package1
yum groupinfo group1   //显示程序组group1信息yum search string 根据关键字string查找安装包
```



###### find查询

###### which查询

###### whereis查询

