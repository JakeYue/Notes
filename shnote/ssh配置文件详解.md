 # 整理下SSH Server的配置文件中各项对其进行解释。

# 一、 

   sshd配置文件是：/etc/ssh/sshd_config，去掉注释后全文基本如下：
```vim
Port 22  #设置ssh监听的端口号，默认22端口
ListenAddress ::
ListenAddress 0.0.0.0  #指定监听的地址，默认监听所有；
Protocol 2,1   #指定支持的SSH协议的版本号。'1'和'2'表示仅仅支持SSH-1和SSH-2协议。
#"2,1"表示同时支持SSH-1和SSH-2协议。#
HostKey /etc/ssh/ssh_host_rsa_key
HostKey /etc/ssh/ssh_host_dsa_key
HostKey /etc/ssh/ssh_host_ecdsa_key
HostKey /etc/ssh/ssh_host_ed25519_key    #HostKey是主机私钥文件的存放位置; 
#SSH-1默认是 /etc/ssh/ssh_host_key 。SSH-2默认是 /etc/ssh/ssh_host_rsa_key和
#/etc/ssh/ssh_host_dsa_key 。一台主机可以拥有多个不同的私钥。"rsa1"仅用于SSH-1，
#"dsa"和"rsa"仅用于SSH-2。
UsePrivilegeSeparation yes     #是否通过创建非特权子进程处理接入请求的方法来进行权
#限分 离。默认值是"yes"。 认证成功后，将以该认证用户的身份创另一个子进程。这样做的目的是
#为了防止通过有缺陷的子进程提升权限，从而使系统更加安全。
KeyRegenerationInterval 3600   #在SSH-1协议下，短命的服务器密钥将以此指令设置的时
#间为周期(秒)，不断重新生成；这个机制可以尽量减小密钥丢失或者黑客攻击造成的损失。设为 0 
#表示永不重新生成为 3600(秒)。
ServerKeyBits 1024    #指定服务器密钥的位数
SyslogFacility AUTH   #指定 将日志消息通过哪个日志子系统(facility)发送。有效值是：
#DAEMON, USER, AUTH(默认), LOCAL0, LOCAL1, LOCAL2, LOCAL3,LOCAL4, LOCAL5, 
#LOCAL6, LOCAL7
LogLevel INFO     #指定日志等级(详细程度)。可用值如下:QUIET, FATAL, ERROR, INFO
#(默认), VERBOSE, DEBUG, DEBUG1, DEBUG2, DEBUG3,DEBUG 与 DEBUG1 等价；DEBUG2
# 和 DEBUG3 则分别指定了更详细、更罗嗦的日志输出。比 DEBUG 更详细的日志可能会泄漏用户
# 的敏感信息，因此反对使用。
LoginGraceTime 120  #限制用户必须在指定的时限(单位秒)内认证成功，0 表示无限制。默认
#值是 120 秒;如果用户不能成功登录，在用户切断连接之前服务器需要等待120秒。
PermitRootLogin yes  #是否允许 root 登录。可用值如下："yes"(默认) 表示允许。
#"no"表示禁止。"without-password"表示禁止使用密码认证登录。"forced-commands-only"
#表示只有在指定了 command 选项的情况下才允许使用公钥认证登录，同时其它认证方法全部被禁止。
#这个值常用于做远程备份之类的事情。
StrictModes yes       #指定是否要求 sshd(8) 在接受连接请求前对用户主目录和相关的配
#置文件 进行宿主和权限检查。强烈建议使用默认值"yes"来预防可能出现的低级错误。
RSAAuthentication yes  #是否允许使用纯 RSA 公钥认证。仅用于SSH-1。默认值是"yes"。
PubkeyAuthentication yes  #是否允许公钥认证。仅可以用于SSH-2。默认值为"yes"。
IgnoreRhosts yes    #是否取消使用 ~/.ssh/.rhosts 来做为认证。推荐设为yes。
RhostsRSAAuthentication no  #这个选项是专门给 version 1 用的，使用 rhosts 档案在 　　　　　　　　　　　　　    
#/etc/hosts.equiv配合 RSA 演算方式来进行认证！推荐no。
HostbasedAuthentication no    #这个与上面的项目类似，不过是给 version 2 使用的
IgnoreUserKnownHosts no          #是否在 RhostsRSAAuthentication 或 
#HostbasedAuthentication 过程中忽略用户的 ~/.ssh/known_hosts 文件。默认值是"no"。
#为了提高安全性，可以设为"yes"。
PermitEmptyPasswords no         #是否允许密码为空的用户远程登录。默认为"no"。
ChallengeResponseAuthentication no   #是否允许质疑-应答(challenge-response)认         
#证。默认值是"yes"，所有 login.conf中允许的认证方式都被支持。
PasswordAuthentication yes      # 是否允许使用基于密码的认证。默认为"yes"。
KerberosAuthentication no    #是否要求用户为 PasswordAuthentication 提供的密码
#必须通 过 Kerberos KDC 认证，也就是是否使用Kerberos认证。使用Kerberos认证，服务器
#需要一个可以校验 KDC identity 的 Kerberos servtab 。默认值是"no"。
KerberosGetAFSToken no       #如果使用了 AFS 并且该用户有一个 Kerberos 5 TGT，
#那么开   启该指令后,将会在访问用户的家目录前尝试获取一个 AFS  token 。默认为"no"。
KerberosOrLocalPasswd yes   #如果 Kerberos 密码认证失败，那么该密码还将要通过其它
#的 认证机制(比如 /etc/passwd)。默认值为"yes"。
KerberosTicketCleanup yes    #是否在用户退出登录后自动销毁用户的 ticket 。默认
#"yes"。
GSSAPIAuthentication no      #是否允许使用基于 GSSAPI 的用户认证。默认值为"no"。
#仅用 于SSH-2。
GSSAPICleanupCredentials yes   #是否在用户退出登录后自动销毁用户凭证缓存。默认值      
#是"yes"。仅用于SSH-2。
X11Forwarding no    #是否允许进行 X11 转发。默认值是"no"，设为"yes"表示允许。如果
#允许X11转发并且sshd代理的显示区被配置为在含有通配符的地址(X11UseLocalhost)上监听。
#那么将可能有额外的信息被泄漏。由于使用X11转发的可能带来的风险，此指令默认值为"no"。需
#要注意的是，禁止X11转发并不能禁止用户转发X11通信，因为用户可以安装他们自己的转发器。如
#果启用了 UseLogin ，那么X11转发将被自动禁止。
X11DisplayOffset 10    #指定X11 转发的第一个可用的显示区(display)数字。默认值                   
#是 10 。这个可以用于防止 sshd 占用了真实的 X11 服务器显示区，从而发生混淆。
PrintMotd no                #登入后是否显示出一些信息呢？例如上次登入的时间、地点等
#等，预设是 yes ，但是，如果为了安全，可以考虑改为 no ！
PrintLastLog yes           #指定是否显示最后一位用户的登录时间。默认值是"yes"
TCPKeepAlive yes       #指定系统是否向客户端发送 TCP keepalive 消息。默认值是"yes"
#。这种消息可以检测到死连接、连接不当关闭、客户端崩溃等异常。可以设为"no"关闭这个特性。
UseLogin no               #是否在交互式会话的登录过程中使用 login。默认值是"no"。
#如果开启此指令，那么 X11Forwarding 将会被禁止，因为 login 不知道如何处理 xauth 
#cookies 。需要注意的是，login是禁止用于远程执行命令的。如果指定了 
#UsePrivilegeSeparation ，那么它将在认证完成后被禁用。
MaxStartups 10        #最大允许保持多少个未认证的连接。默认值是 10 。到达限制后，
#将不再接受新连接，除非先前的连接认证成功或超出 LoginGraceTime 的限制。
MaxAuthTries 6     #指定每个连接最大允许的认证次数。默认值是 6 。如果失败认证的次数超
#过这个数值的一半，连接将被强制断开，且会生成额外的失败日志消息。
UseDNS no          #指定是否应该对远程主机名进行反向解析，以检查此主机名是否与其IP
#地址真实对应。
Banner /etc/issue.net   #将这个指令指定的文件中的内容在用户进行认证前显示给远程用户。
#这个特性仅能用于SSH-2，默认什么内容也不显示。"none"表示禁用这个特性。
Subsystem sftp /usr/lib/openssh/sftp-server   #配置一个外部子系统(例如，一个文件
#传输守   护进程)。仅用于SSH-2协议。值是一个子系统的名字和对应的命令行(含选项和参数)。
UsePAM yes     #是否使用PAM模块认证
```
* 总结：
  一般ssh远程连接不上，后台登陆服务器之后，确认网络有无问题，防火墙是否放行ssh端口，查一下ssh
服务又没有启动，检查sshd_config文件配置（注意端口号、是否有地址绑定、是否允许root登陆等）；
如果都没问题，再查下/etc/hosts.deny 和 /etc/hosts.allow 是否有限制登陆。

# 二、相关补充

1、SSH1 vs. SSH2

```SSH协议规范存在一些小版本的差异，但是有两个主要的大版本：SSH1 (版本号 1.XX) 和 SSH2 (版本号 2.00)。事实上，SSH1和SSH2是两个完全不同互不兼容的协议。SSH2明显地提升了SSH1中的很多方面。首先，SSH是宏设计，几个不同的功能（如：认证、传输、连接）被打包进一个单一的协议，SSH2带来了比SSH1更强大的安全特性，如基于MAC的完整性检查，灵活的会话密钥更新、充分协商的加密算法、公钥证书等等。这就是用SecurityCRT的时候为什么会让你选择ssh1还是ssh2的原因。```

```SSH2由IETF标准化，且它的实现在业界被广泛部署和接受。由于SSH2对于SSH1的流行和加密优势，现在几乎所有Linux新发行版中，OpenSSH服务器默认禁用了SSH1。```



2、SSH基于密钥的安全验证

    这种登陆方式，需要依靠密钥，也就是说你必须为自己创建一对密钥对（公钥和私钥），并且把该公钥放到需要访问的服务器上。

* 注意：不能在需要访问的服务器上创建密钥，否则无法通过该密钥连接该服务器，但是通过该密钥连接其他服务器是正常的。

  如果你要连接到ssh服务器，ssh客户端会向ssh服务器发出请求，请求用你的密钥进行安全验证。ssh服务器在收到该请求之后，会先在ssh服务器上，检查你登陆的用户的主目录下寻找对应的公钥，然后把它和你发送过来的公钥进行比较。如果两个公钥一致，ssh服务器就用公钥加密“质询”（challenge）并把它发送给ssh客户端。ssh客户端在收到“质询”之后就可以用你的私钥解密该“质询”，再把它发送给ssh服务器。



# 3、X11

  #X11也叫做X Window系统，X Window系统 (X11或X)是一种 位图 显示的 视窗系统 。它是在 Unix 和 类Unix 操作系统 ，以及 OpenVMS 上建立图形用户界面 的标准工具包和协议，并可用于几乎所有已有的现代操作系统。



# 4、解决SSH登陆缓慢

     编辑sshd_config配置文件，把UseDNS改为no，或者在尾部添加一行：UseDNS no

     关闭GSSAPI认证：GSSAPIAuthentication no 

     修改完成之后重启ssh服务。



# 5、/etc/hosts.allow 和/etc/hosts.deny

  有些server会用这两个文件限制登陆的ip，通常是deny里面deny all，然后再allow需要登陆的ip地址；例如：

先禁止所有：

[root @test root]# cat /etc/hosts.deny

sshd: ALL : deny　

再放行允许访问的ip:

[root @test root]# cat  /etc/hosts.allow

sshd:10.10.10.* : allow      允许10.10.10.0/24网段登陆

sshd: 192.168.2.2 : allow   允许192.168.2.2这个ip登陆



# 6、PAM  

  PAM全称是可插拔身份认证模块(Pluggable Authentication Modules);

  Linux-PAM (Linux下的可插入式认证模组) 是一套共享函数库,允许系统管理员来决定应用程式如何识别用户。换句话说，就是用不着(重写和)重新编译一个(支援PAM的)程式,就可以切换它所用的认证机制. 你可以整个的升级你的证系统而不用去管应用程式本身。Linux-PAM 给系统管理员提供了相当大的弹性来设定系统里程式的权限赋予。

  PAM这里先不多说，以后再做单独详细介绍。

