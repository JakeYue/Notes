# Nginx 常用配置解析

#### nginx目录结构

```shell
├── client_body_temp
├── conf                              #配置文件目录
│   ├── fastcgi.conf
│   ├── fastcgi.conf.default
│   ├── fastcgi_params
│   ├── fastcgi_params.default
│   ├── koi-utf
│   ├── koi-win
│   ├── mime.types
│   ├── mime.types.default
│   ├── nginx.conf                      #主配置文件
│   ├── nginx.conf.default
│   ├── scgi_params
│   ├── scgi_params.default
│   ├── uwsgi_params
│   ├── uwsgi_params.default
│   └── win-utf
├── fastcgi_temp
├── html                                #初始的静态页面存放目录
│   ├── 50x.html
│   └── index.html
├── logs                                #日志目录
│   ├── access.log
│   ├── error.log
│   └── nginx.pid
├── proxy_temp
├── sbin                                #启动目录
│   └── nginx
├── scgi_temp
└── uwsgi_temp
```

#### nginx.conf

```shell
# 运行用户
user  nobody;
#启动进程，通常设置成和cpu的数量相等
worker_processes  2;

#全局错误日志及PID文件及存放路径
#error_log  logs/error.log;
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;

#pid        logs/nginx.pid;

#工作模式及连接数上限
events {
    #单个后台work process进程的最大并发链接数
    worker_connections  1024;  
}



#网页信息
http {
    #设定mine类型，类型由mine。type文件定义
    include       mime.types;
    default_type  application/octet-stream;
  
    #设定日志格式
    #log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
    #                  '$status $body_bytes_sent "$http_referer" '
    #                  '"$http_user_agent" "$http_x_forwarded_for"';
    
    #日志文件存储路径/usr/local/...(nginx的安装目录)
    #access_log  logs/access.log  main;
    #sendfile 指令指定 nginx 是否调用 sendfile 函数（zero copy 方式）来输出文件，
    #对于普通应用，必须设为 on,
    #如果用来进行下载等应用磁盘IO重负载应用，可设置为 off，
    #以平衡磁盘与网络I/O处理速度，降低系统的uptime.
    sendfile        on;
    #tcp_nopush     on;
    
    #连接超时时间
    #keepalive_timeout  0;
    keepalive_timeout  65;
    
    #开启gzip压缩 如果没有开启gzip，用户访问我们的时候就是以原图来访问。
    gzip  on;
    #小于1K的文件不适合压缩,下限是1k
    gzip_min_lenth 1k;
    #缓存的内存空间--4个16进制数据流
    gzip_buffers 4 16k;
    #http版本
    gzip_http_version 1.1
    #开启判断客户端和浏览器是否支持gzip
    gzip_vary on;
    #设定虚拟主机配置
    server {
        #监听80端口
        listen       80;
        #定义使用 访问的网址
        server_name  localhost;
        #设置字符编码
        #charset koi8-r;
        #设定本虚拟主机的访问日志
        #access_log  logs/host.access.log  main;
        #默认请求，优先级最低的配置
        location / {
            #定义服务器的默认网站根目录位置 这个root目录其实就是/usr/local目录
            root   html;
            # 匹配任何请求，因为所有请求都是以"/"开始
            # 但是更长字符匹配或者正则表达式匹配会优先匹配
            #定义首页索引文件的名称
            index  index.html index.htm;
        }
        
        #配置Nginx缓存
        location ~.*\.(jpg|png|gif)$ {
          expires 30d; #缓存存放30天，然后自动清除
        }
        location ~.*\.(css|js)? $ {
          expires 1h; #缓存存放1小时
        }

        #error_page  404              /404.html;

        # redirect server error pages to the static page /50x.html
        #定义错误页面
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }

 
    # 对 “/” 启用反向代理,对上面的实例  
    location / {
      proxy_pass http://127.0.0.1:3000;  # 设置要代理的 uri，注意最后的 /。可以是 Unix 域套接字路径，也可以是正则表达式。
      proxy_redirect off; # 设置后端服务器“Location”响应头和“Refresh”响应头的替换文本
      proxy_set_header X-Real-IP $remote_addr; # 获取用户的真实 IP 地址
      #后端的Web服务器可以通过 X-Forwarded-For 获取用户真实IP，多个 nginx 反代的情况下，例如 CDN。参见：http://gong1208.iteye.com/blog/1559835 和 http://bbs.linuxtone.org/thread-9050-1-1.html
      proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
      #以下是一些反向代理的配置，可选。
      proxy_set_header Host $host; # 允许重新定义或者添加发往后端服务器的请求头。
      client_max_body_size 10m; #允许客户端请求的最大单文件字节数
      client_body_buffer_size 128k; #缓冲区代理缓冲用户端请求的最大字节数，
      proxy_connect_timeout 90; #nginx跟后端服务器连接超时时间(代理连接超时)
      proxy_send_timeout 90; #后端服务器数据回传时间(代理发送超时)
      proxy_read_timeout 90; #连接成功后，后端服务器响应时间(代理接收超时)
      proxy_buffer_size 4k; #设置代理服务器（nginx）保存用户头信息的缓冲区大小
      proxy_buffers 4 32k; #proxy_buffers缓冲区，网页平均在32k以下的设置
      proxy_busy_buffers_size 64k; #高负荷下缓冲大小（proxy_buffers*2）
      proxy_temp_file_write_size 64k;
      #设定缓存文件夹大小，大于这个值，将从upstream服务器传
    }

    # 本地动静分离反向代理配置
    # 所有 jsp 的页面均交由tomcat或resin处理
    location ~ .(jsp|jspx|do)?$ {
      proxy_set_header Host $host;
      proxy_set_header X-Real-IP $remote_addr;
      proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
      proxy_pass http://127.0.0.1:8080;
    }
    
    # 所有静态文件由nginx直接读取不经过tomcat或resin
    location ~ .*.(htm|html|gif|jpg|jpeg|png|bmp|swf|ioc|rar|zip|txt|flv|mid|doc|ppt|pdf|xls|mp3|wma)${
      root    /data/www/ospring.pw/public;
      expires 15d;
    }
    location ~ ^/(upload|html)/  {
      root    /data/www/ospring.pw/public/html;
      expires 30d;
    }

    include     vhosts//.conf; 分割配置文件，方便管理
    }
    
    
    #这里可以配置多台虚拟主机
    # another virtual host using mix of IP-, name-, and port-based configuration
    #配置虚拟机
    #server {
    # 配置监听端口，只要端口不同就是不同的虚拟主机
    #    listen       8000;
    #    listen       somename:8080;
    #配置访问域名
    #    server_name  somename  alias  another.alias;
    
    #    location / {
    #        root   html;
    #        index  index.html index.htm;
    #    }
    #}


    # HTTPS server
    web服务器配置
    #server {
    #    listen       443 ssl;
    #    server_name  localhost;

    #    ssl_certificate      cert.pem;
    #    ssl_certificate_key  cert.key;

    #    ssl_session_cache    shared:SSL:1m;
    #    ssl_session_timeout  5m;

    #    ssl_ciphers  HIGH:!aNULL:!MD5;
    #    ssl_prefer_server_ciphers  on;

    #    location / {
    #        root   html;
    #        index  index.html index.htm;
    #    }
    #}

}
```

```shell
# 方向代理
location / {
    proxy_pass       http://localhost:8000; #需要被代理的地址
    proxy_set_header Host      $host; 
    proxy_set_header X-Real-IP $remote_addr;
}
```

```shell
#负载均衡配置文件
user  nobody;
events{
   worker_connections  1024; 
}

http{
   #配置多台代理服务器地址
   upstream myproject{
       server 59.68.29.103;     #server backend1.example.com       weight=5;
       server 59.68.29.108;
      }
      server{
       listen 8080;
       #在此处添加域名映射的规则
       server_name localhost;
       location / {
           proxy_pass http://myproject;#
           }
      }
}

#负载均衡的服务器列表
    upstream myproject{
        #设置ip_hash指令：将同一个用户引向同一个服务器
        ip_hash;
        #服务器集群中服务器地址
        server 59.68.29.103:80;
        server 59.68.29.108;
    }
    
# server指令主要用于指定服务器的名称和参数
     upstream myproject{
        #服务器集群中服务器地址
        server 182.18.22.2:80 weight=2 ;#权重越大，被访问的概率越大
        server 118.144.78.52;
    }
```

