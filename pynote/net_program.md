# python网络编程

> 本质其实都是两个程序之间的通讯

1. **C/S构架**

    - C/S即：client与server，客户端与服务器端构架，这种构架也是从用户层面（物理层面）来划分的

    ![827651-20171102180740138-1487379531](https://raw.githubusercontent.com/JakeYue/picgo/master/uPic/827651-20171102180740138-1487379531.png2021%2010%2014%2014%2024%2025)

2. B/S构架

    - B/S即：Browser与Server,中文意思：浏览器端与服务器端架构，这种架构是从用户层面来划分的.

    ![827651-20171102182619326-272756889](https://raw.githubusercontent.com/JakeYue/picgo/master/uPic/827651-20171102182619326-272756889.png2021%2010%2014%2014%2027%2016)![](

- **一个程序如何在网络上找到另一个程序？**

    - 程序必须要启动，其次，必须有这台机器的地址,使用IP地址来表示。
    - 找到机器后，通过port（端口）来找到具体程序。"端口"是英文port的意译，可以认为是设备与外界通讯交流的出口

    > **因此ip地址精确到具体的一台电脑，而端口精确到具体的程序**

**OSI七层模型**

> 按照分工不同把互联网协议从逻辑上划分了层级:

![827651-20180107205950456-843487109](https://raw.githubusercontent.com/JakeYue/picgo/master/uPic/827651-20180107205950456-843487109.png2021%2010%2014%2014%2058%2020)

### socket概念

> socket层

![827651-20180107205809034-380661986](https://raw.githubusercontent.com/JakeYue/picgo/master/uPic/827651-20180107205809034-380661986.png2021%2010%2014%2015%2000%2006)

**socket理解**

Socket是应用层与TCP/IP协议族通信的中间软件抽象层，它是一组接口。在设计模式中，Socket其实就是一个门面模式，它把复杂的TCP/IP协议族隐藏在Socket接口后面，对用户来说，一组简单的接口就是全部，让Socket去组织数据，以符合指定的协议.

套接字（socket）：有两种（或者称为有两个种族）,分别是基于文件型的和基于网络型的.

	-  *基于文件类型的套接字家族*：AF_UNIX；unix一切皆文件，基于文件的套接字调用的就是底层的文件系统来取数据，两个套接字进程运行在同一机器，可以通过访问同一个文件系统间接完成通信
	-  *基于网络类型的套接字家族*：AF_INET；还有AF_INET6被用于ipv6，还有一些其他的地址家族，不过，他们要么是只用于某个平台，要么就是已经被废弃，或者是很少被使用，或者是根本没有实现，所有地址家族中，AF_INET是使用最广泛的一个，python支持很多种地址家族，但是由于我们只关心网络编程，所以大部分时候我么只使用AF_INET

----

**TCP**（Transmission Control Protocol）可靠的、面向连接的协议（eg:打电话）、传输效率低全双工通信（发送缓存&接收缓存）、面向字节流。使用TCP的应用：Web浏览器；电子邮件、文件传输程序。

**UDP**（User Datagram Protocol）不可靠的、无连接的服务，传输效率高（发送前时延小），一对一、一对多、多对一、多对多、面向报文，尽最大努力服务，无拥塞控制。使用UDP的应用：域名系统 (DNS)；视频流；IP语音(VoIP)。

### socket套接字使用

![827651-20171027102242789-2142796570](https://raw.githubusercontent.com/JakeYue/picgo/master/uPic/827651-20171027102242789-2142796570.jpg2021%2010%2014%2015%2007%2041)

> 基于TCP协议的socket：**tcp是基于链接的，必须先启动服务端，然后再启动客户端去链接服务端**

```python
# server

import socket

sk = socket.socket() #创建socket对象
sk.bind(('127.0.0.1',8898))  #把地址绑定到套接字
sk.listen()          #监听链接
conn,addr = sk.accept() #接受客户端链接
ret = conn.recv(1024)  #接收客户端信息
print(ret)       #打印客户端信息
conn.send(b'hi')        #向客户端发送信息
conn.close()       #关闭客户端套接字
sk.close()        #关闭服务器套接字(可选)
```

```python
# client
import socket
sk = socket.socket()           # 创建客户套接字
sk.connect(('127.0.0.1',8898))    # 尝试连接服务器
sk.send(b'hello!')
ret = sk.recv(1024)         # 对话(发送/接收)
print(ret)
sk.close()            # 关闭客户套接字

# 地址重用，socket配置,在bind前配置
from socket import SOL_SOCKET,SO_REUSEADDR
sk.setsockopt(SOL_SOCKET, SO_REUSEADDR, 1)
```

**TCP**

```shell
from socket import socket
sk = socket(type=socket.SOCK_STREAM)
sk.bind(('127.0.0.1',9090))
sk.listen()

while 1:
    # print(123)
    conn,addr = sk.accept() #  等待连接 -- 阻塞
    # print(456)
    while 1:
        # print(789)
        msg_r = conn.recv(1024).decode('utf-8') # 阻塞等待接收客户端发来的消息
        # print('jqk')
        print('接收到来自%s的一个消息:%s' % (addr, msg_r))
        if msg_r == 'q':
            break
        msg_s = input('>>>')
        conn.send(msg_s.encode('utf-8'))# 发送给客户端消息
        if msg_s == 'q':
            break
    conn.close()# 服务器和当前客户端断开连接,程序回到上一层死循环,重新等待客户端的连接
sk.close()




from socket import  socket
sk = socket()
sk.connect(('127.0.0.1',9090))

while 1:
    msg_s = input('>>>')
    sk.send(msg_s.encode('utf-8'))
    if msg_s == 'q':
        break
    msg_r = sk.recv(1024).decode('utf-8')
    print(msg_r)
    if msg_r == 'q':
        break

sk.close()
```

**UDP**

```shell
import socket
sk = socket.socket(type=socket.SOCK_DGRAM)# udp协议
sk.bind(('127.0.0.1',9090))
dic = {'alex':'\033[0;33;42m','太白':'\033[0;35;40m'}
while 1:
    msg_r,addr = sk.recvfrom(1024)# 接收来自哪里的消息
    msg_r = msg_r.decode('utf-8')# alex : 我要退学
    # 对于msg_r,通过':'分割,获取下标为0的,也就是name,再去掉name的左右两边的空格
    name = msg_r.split(':')[0].strip()

    color = dic.get(name,'')# 获取字典中 name所对应的 颜色值
    print('%s%s \033[0m'%(color,msg_r))
    if msg_r == 'q':# 如果当前客户端想要断开连接
        continue # 服务器端不应该继续通话了,应该等待接收另一个客户端的连接,返回到recvfrom
    msg_s = input('>>>')
    sk.sendto(msg_s.encode('utf-8'), addr)
    if msg_s == 'q':
        break
sk.close()




import socket
sk = socket.socket(type=socket.SOCK_DGRAM)
name = input('请输入您的名字:>>>')
while 1:
    msg_s = input('>>>')
    msg_s = name + " : "+msg_s
    sk.sendto(msg_s.encode('utf-8'),('127.0.0.1',9090))# 发给谁一条消息
    if msg_s is 'q':
        break
    msg_r,addr = sk.recvfrom(1024)
    msg_r = msg_r.decode('utf-8')
    print(msg_r)
    if msg_r == 'q':
        break

sk.close()
```

UDP同步时间

```shell
import socket
import time
sk = socket.socket(type=socket.SOCK_DGRAM)

sk.bind(('127.0.0.1',9090))

while 1:
    tm_format,addr = sk.recvfrom(1024)
    tm_format = tm_format.decode('utf-8')# %Y-%m\%d %H:%M:%S
    local_tm = time.strftime(tm_format)# 获取到了对应格式的当前时间
    sk.sendto(local_tm.encode('utf-8'),addr)# 返回给客户端

sk.close()





import socket
import time
sk = socket.socket(type=socket.SOCK_DGRAM)
tm_format = input('>>>')
while 1:
    sk.sendto(tm_format.encode('utf-8'),('127.0.0.1',9090))
    local_tm,addr = sk.recvfrom(1024)
    print(local_tm.decode('utf-8'))
    time.sleep(2)
```

 **Python Internet 模块**

| 协议   | 功能                       | 端口号 | py模块                     |
| ------ | -------------------------- | ------ | -------------------------- |
| HTTP   | 网页访问                   | 80     | httplib，urllib，xmlrpclib |
| NNTP   | 阅读和张贴新闻文章（帖子） | 119    | nntplib                    |
| FTP    | 文件传输                   | 20     | ftplib，urllib             |
| SMTP   | 发邮件                     | 25     | smtplib                    |
| POP3   | 接受收件                   | 110    | poplib                     |
| IMAP4  | 获取收件                   | 143    | imaplib                    |
| Telnet | 命令行                     | 23     | telnetlib                  |
| Gopher | 信息查找                   | 70     | gopherlib，urllib          |

