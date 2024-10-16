# Nginx

Nginx是由lgor Sysoev为俄罗斯访问量第二的rambler.ru站点设计开发的，从2004年发布至今；

Nginx 是一个轻量级/高性能的web服务器，主要功能包括：反向代理、负载均衡、配置SSL证书、防盗链、解决跨域问题、缓存、限流、动静资源分离等

- 中文文档：https://www.nginx.cn/doc/

### 代理

正向代理（forward proxy）：是一个位于客户端和目标服务器之间的服务器(代理服务器)，为了从目标服务器取得内容，客户端向代理服务器发送一个请求并指定目标，然后代理服务器向目标服务器转交请求并将获得的内容返回给客户端。

反向代理（reverse proxy）：是指以代理服务器来接受internet上的连接请求，然后将请求转发给内部网络上的服务器，并将从服务器上得到的结果返回给internet上请求连接的客户端，此时代理服务器对外就表现为一个反向代理服务器。

## 安装配置

### Mac

使用`homebrew`进行安装

```sh
$ brew install nginx
```

#### 常用指令

```sh
# 启动|停止|重启 服务
brew services start|stop|restart nginx
# 测试配置是否有语法错误
nginx -t
# 快速停止nginx
nginx -s quit
# 查看版本，以及配置文件地址
nginx -V
# 查看版本
nginx -v
# 重新加载配置|重启|快速停止|安全关闭nginx
nginx -s reload|reopen|stop|quit
# 帮助
nginx -h
```

#### 常见问题

- `sudo nginx -s reload`时报错：`nginx: [error] invalid PID number "" in "/usr/local/var/run/nginx.pid"`

  ```sh
  $ sudo nginx -c /usr/local/etc/nginx/nginx.conf
  $ sudo nginx -s reload
  ```

- 访问php文件报错：File not found

  更改配置文件nginx.conf 
  
  ```nginx
  fastcgi_param SCRIPT_FILENAME /scripts$fastcgi_script_name; 
  # 替换成下面
  fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
  ```

#### 个人配置

`/usr/local/etc/nginx/nginx.conf`

```nginx
#user  nobody;
worker_processes  1;

events {
    worker_connections  1024;
}

http {
    include       mime.types;
    default_type  application/octet-stream;

    sendfile        on;
    #tcp_nopush     on;

    keepalive_timeout  65;

    #gzip  on;

    server {
        listen       8080;
        server_name  localhost;

        location / {
            root   html;
            index  index.html index.htm index.php;
        }

        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }

        # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
        location ~ \.php$ {
            root           html;
            fastcgi_pass   127.0.0.1:9000;
            fastcgi_index  index.php;
            fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
            include        fastcgi_params;
        }

    }
    include servers/*.conf;
}
```

`/usr/local/etc/nginx/servers/laradmin.com.conf`

```nginx
server {
    #监听端口
    listen    8088;

    #虚拟主机域名
    server_name  laradmin.com;

    #网站根目录
    root /Users/chenxinfang/person/code/laradmin/public;

    #定义路径下默认访问的文件名
    index index.php index.html index.htm default.php default.htm default.html;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
        #打开目录浏览功能，可以列出整个目录
        #autoindex on;
    }

    # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
    location ~ \.php$ {
        fastcgi_pass     127.0.0.1:9000;
        fastcgi_index    index.php;
        include          fastcgi_params;
        fastcgi_param    SCRIPT_FILENAME  $document_root$fastcgi_script_name;
    }
    #禁止访问的文件或目录
    location ~ ^/(\.user.ini|\.htaccess|\.git|\.svn|\.project|LICENSE|README.md)
    {
        return 404;
    }

    location ~ .*\.(gif|jpg|jpeg|png|bmp|swf)$
    {
        expires      30d;
        error_log off;
        access_log off;
    }

    location ~ .*\.(js|css)?$
    {
        expires      12h;
        error_log off;
        access_log off;
    }
}
```

## 配置文件

### 配置示例

默认配置文件`nginx.config.default`：

```nginx
#定义Nginx运行的用户和用户组，默认为nobody nobody。
user www www;

#nginx进程数，通常设置成和cpu的数量相等，默认为1
worker_processes 4;

#制定日志路径，级别。这个设置可以放入全局块，http块，server块，级别以此为：debug|info|notice|warn|error|crit|alert|emerg
#error_log  logs/error.log;
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;

#指定nginx进程运行文件存放地址
#pid        logs/nginx.pid;


events {
    worker_connections  1024;
}

events {
  	#设置网路连接序列化，防止惊群现象发生，默认为on
  	accept_mutex on;
  
  	#设置一个进程是否同时接受多个网络连接，默认为off
    multi_accept on;
  
 	  #事件驱动模型，select|poll|kqueue|epoll|resig|/dev/poll|eventport
    use epoll
    
    #单个进程最大连接数（最大连接数=连接数+进程数），默认为512。根据硬件调整，和前面工作进程配合使用，尽量大，但是别把cup跑满
    worker_connections  1024;
    
    #keepalive 超时时间
    keepalive_timeout 60;

		#客户端请求头部的缓冲区大小。根据系统分页大小设置，一般一个请求头的大小不会超过1k，不过由于一般系统分页都要大于1k，所以这里设置为分页大小。分页大小可以用命令getconf PAGESIZE 取得。
    #[root@web001 ~]# getconf PAGESIZE
    #但也有client_header_buffer_size超过4k的情况，但是client_header_buffer_size该值必须设置为“系统分页大小”的整倍数。
    client_header_buffer_size 4k;
    
    #这个将为打开文件指定缓存，默认是没有启用的，max指定缓存数量，建议和打开文件数一致，inactive是指经过多长时间文件没被请求后删除缓存。
    open_file_cache max=65535 inactive=60s;
    
    
    #这个是指多长时间检查一次缓存的有效信息。
    #语法:open_file_cache_valid time 默认值:open_file_cache_valid 60 使用字段:http, server, location 这个指令指定了何时需要检查open_file_cache中缓存项目的有效信息.
    open_file_cache_valid 80s;
    
    
    #open_file_cache指令中的inactive参数时间内文件的最少使用次数，如果超过这个数字，文件描述符一直是在缓存中打开的，如上例，如果有一个文件在inactive时间内一次没被使用，它将被移除。
    #语法:open_file_cache_min_uses number 默认值:open_file_cache_min_uses 1 使用字段:http, server, location  这个指令指定了在open_file_cache指令无效的参数中一定的时间范围内可以使用的最小文件数,如果使用更大的值,文件描述符在cache中总是打开状态.
    open_file_cache_min_uses 1;
    
    #语法:open_file_cache_errors on | off 默认值:open_file_cache_errors off 使用字段:http, server, location 这个指令指定是否在搜索一个文件是记录cache错误.
    open_file_cache_errors on;
}

#设定http服务器，利用它的反向代理功能提供负载均衡支持
http {
  	#文件扩展名与文件类型映射表
    include       mime.types;
  
  	#默认文件类型，默认为text/plain
    default_type  application/octet-stream;

  	#默认编码
    charset utf-8;
  
  	#日志格式
  	#$remote_addr与$http_x_forwarded_for用以记录客户端的ip地址；
    #$remote_user：用来记录客户端用户名称；
    #$time_local： 用来记录访问时间与时区；
    #$request： 用来记录请求的url与http协议
    #$status： 用来记录请求状态；成功是200，
    #$body_bytes_sent ：记录发送给客户端文件主体内容大小；
    #$http_referer：用来记录从那个页面链接访问过来的；
    #$http_user_agent：记录客户浏览器的相关信息；
    #log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
    #                  '$status $body_bytes_sent "$http_referer" '
    #                  '"$http_user_agent" "$http_x_forwarded_for"';

    #服务日志 
    #access_log  logs/access.log  main;

  	#设定通过nginx上传文件的大小
    client_max_body_size 8m;
  
  	#允许sendfile方式传输文件，默认为off，可以在http块，server块，location块。
    sendfile        on;
  
  	#此选项允许或禁止使用socke的TCP_CORK的选项，此选项仅在使用sendfile的时候使用
    #tcp_nopush     on;
  
  	#开启目录列表访问，合适下载服务器，默认关闭。
    autoindex off;
  
  	#每个进程每次调用传输数量不能大于设定的值，默认为0，即不设上限。
  	sendfile_max_chunk 100k;

  	#长连接超时时间，默认为75s，可以在http，server，location块。
    keepalive_timeout  65;
  
  	#FastCGI相关参数是为了改善网站的性能：减少资源占用，提高访问速度。下面参数看字面意思都能理解。
    fastcgi_connect_timeout 300;
    fastcgi_send_timeout 300;
    fastcgi_read_timeout 300;
    fastcgi_buffer_size 64k;
    fastcgi_buffers 4 64k;
    fastcgi_busy_buffers_size 128k;
    fastcgi_temp_file_write_size 128k;
  	fastcgi_intercept_errors on;
    
    #gzip模块设置，默认关闭
  	#开启gzip压缩输出
    gzip on;
    gzip_min_length 1k;    #最小压缩文件大小
    gzip_buffers 4 16k;    #压缩缓冲区
    gzip_http_version 1.1; #压缩版本（默认1.1，前端如果是squid2.5请使用1.0）
    gzip_comp_level 2;     #压缩等级
  	#压缩类型，默认就已经包含textml，所以下面就不用再写了，写上去也不会有问题，但是会有一个warn。
    gzip_types text/plain application/javascript application/x-javascript text/javascript text/css application/xml;
    gzip_vary on;
    gzip_proxied   expired no-cache no-store private auth;
    gzip_disable   "MSIE [1-6]\.";
		limit_conn_zone $binary_remote_addr zone=perip:10m;
		limit_conn_zone $server_name zone=perserver:10m;

    #开启限制IP连接数的时候需要使用
    #limit_zone crawler $binary_remote_addr 10m;
  
  	#负载均衡配置
    upstream www.yiqiesuifeng.cn {
        #upstream的负载均衡，weight是权重，可以根据机器配置定义权重。weigth参数表示权值，权值越高被分配到的几率越大。
        server IP:80 weight=3;
        server IP:80 weight=2;
        server IP:80 weight=3;
		#nginx的upstream目前支持的分配:轮询|weight|ip_hash
  	}
  
  	#虚拟主机的配置
    server {
    	#监听端口
        listen       8080;
    
    	#监听地址，域名可以有多个，用空格隔开
        server_name  localhost;
				
    		#默认编码
        #charset koi8-r;

        #access_log  logs/host.access.log  main;
    
		#默认入口文件名称
        location / {
            root   html;
            index  index.html index.htm;
        }
    
    	#对******进行负载均衡
        location ~ .*.(php|php5)?$
        {
            fastcgi_pass 127.0.0.1:9000;
            fastcgi_index index.php;
            include fastcgi.conf;
        }
         
        #图片缓存时间设置
        location ~ .*.(gif|jpg|jpeg|png|bmp|swf)$
        {
            expires 10d;
        }
         
        #JS和CSS缓存时间设置
        location ~ .*.(js|css)?$
        {
            expires 1h;
        }

        #error_page  404              /404.html;

        # redirect server error pages to the static page /50x.html
        #
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }

        # proxy the PHP scripts to Apache listening on 127.0.0.1:80
        #
        #location ~ \.php$ {
        #    proxy_pass   http://127.0.0.1;
        #}

    		# 开启php-fpm配置
        # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
        location ~ \.php$ {
            root           html;
            fastcgi_pass   127.0.0.1:9000;
            fastcgi_index  index.php;
            fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
            include        fastcgi_params;
        }

        # deny access to .htaccess files, if Apache's document root
        # concurs with nginx's one
        #
        #location ~ /\.ht {
        #    deny  all;
        #}
    		#定义本虚拟主机的访问日志
        access_log  /www/wwwlogs/yiqiesuifeng.cn.log;
    		error_log  /www/wwwlogs/yiqiesuifeng.cn.error.log;
    }
		# 需要引入的配置，一般引入server模块的配置
    include servers/*;
}
```

### 配置文件结构

```nginx
...              #全局块

events {         #events块
   ...
}

http      #http块
{
    ...   #http全局块
    server        #server块
    { 
        ...       #server全局块
        location [PATTERN]   #location块
        {
            ...
        }
        location [PATTERN] 
        {
            ...
        }
    }
    server
    {
      ...
    }
    ...     #http全局块
}
```

- **全局块**：配置影响nginx全局的指令。一般有运行nginx服务器的用户组，nginx进程pid存放路径，日志存放路径，配置文件引入，允许生成worker process数等。
- **events块**：配置影响nginx服务器或与用户的网络连接。有每个进程的最大连接数，选取哪种事件驱动模型处理连接请求，是否允许同时接受多个网路连接，开启多个网络连接序列化等。
- **http块**：可以嵌套多个server，配置代理，缓存，日志定义等绝大多数功能和第三方模块的配置。如文件引入，mime-type定义，日志自定义，是否使用sendfile传输文件，连接超时时间，单连接请求数等。
- **server块**：配置虚拟主机的相关参数，一个http中可以有多个server。
- **location块**：配置请求的路由，以及各种页面的处理情况。

上面是nginx的基本配置，需要注意的有以下几点：

几个常见配置项：

- $remote_addr 与 $http_x_forwarded_for 用以记录客户端的ip地址；
- $remote_user ：用来记录客户端用户名称；
- $time_local ： 用来记录访问时间与时区；
- $request ： 用来记录请求的url与http协议；
- $status ： 用来记录请求状态；成功是200；
- $body_bytes_s ent ：记录发送给客户端文件主体内容大小；
- $http_referer ：用来记录从那个页面链接访问过来的；
- $http_user_agent ：记录客户端浏览器的相关信息；

2.惊群现象：一个网路连接到来，多个睡眠的进程被同时叫醒，但只有一个进程能获得链接，这样会影响系统性能。

3.每个指令必须有分号结束。

## 常用模块

### 配置 location 块

#### 语法结构

- 匹配 URI 类型，有四种参数可选，当然也可以不带参数。
- 命名location，用@来标识，类似于定义goto语句块。

```nginx
location [ = | ~ | ~* | ^~ ] /URI { … }
location @/name/ { … }
```

参数解释：

| 参数      | 解释                                                         |
| --------- | ------------------------------------------------------------ |
| **`空`**  | location 后没有参数直接跟着 **标准 URI**，表示前缀匹配，代表跟请求中的 URI 从头开始匹配。 |
| **`=`**   | 用于**标准 URI** 前，要求请求字符串与其精准匹配，成功则立即处理，nginx停止搜索其他匹配。 |
| **`^~`**  | 用于**标准 URI** 前，并要求一旦匹配到就会立即处理，不再去匹配其他的那些个正则 URI，一般用来匹配目录 |
| **`~`**   | 用于**正则 URI** 前，表示 URI 包含正则表达式， **区分**大小写 |
| **`~\*`** | 用于**正则 URI** 前， 表示 URI 包含正则表达式， **不区分**大小写 |
| **`@`**   | @ 定义一个命名的 location，@ 定义的locaiton名字一般用在内部定向，例如error_page, try_files命令中。它的功能类似于编程中的goto。 |

#### 匹配优先级

nginx有两层指令来匹配请求 URI 。第一个层次是 server 指令，它通过域名、ip 和端口来做第一层级匹配，当找到匹配的 server 后就进入此 server 的 location 匹配。

匹配优先级如下：

```nginx
1. location =    # 精准匹配
2. location ^~   # 带参前缀匹配
3. location ~    # 正则匹配（区分大小写）
4. location ~*   # 正则匹配（不区分大小写）
5. location /a   # 普通前缀匹配，优先级低于带参数前缀匹配。
6. location /    # 任何没有匹配成功的，都会匹配这里处理
```

