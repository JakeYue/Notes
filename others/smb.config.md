# Samba常用配置解析

```shell
#       		>>>samba configuration file<<<
#                     * 全局参数 *
#==================Global Settings ===================
# #代表注释行, ;代表示例行,都无效
[global]

# 可以让你使用另一个配置文件来覆盖缺省的配置文件,登陆后只能看到share
; config file = /usr/local/samba/lib/smb.conf.%m
# 同上(第二种方法)除了可以看到share，其他在smb.conf中定义的共享资源也可以看到
; include = /etc/samba/%U.smb.conf
# 设定 Samba Server 所要加入的工作组或者域
workgroup = WORKGROUP
# 设置Samba Server的NetBIOS名称。如果不填，则默认会使用该服务器的DNS名称的第一部分。netbios name和workgroup名字不要设置成一样了
; netbios name = smbserver
# 设置Samba Server监听哪些网卡，可以写网卡名，也可以写该网卡的IP地址
interfaces = lo enp0s5 192.168.1.1/24
# 表示允许连接到Samba Server的客户端，多个参数以空格隔开
; hosts allow = 127. 192.168.1. 192.168.10.1
# 与hosts allow 刚好相反
; hosts deny = 127. 192.168.1. 192.168.10.1
# max connections用来指定连接Samba Server的最大连接数目,如果超出连接数目，则新的连接请求将被拒绝。0表示不限制
max connections = 0
# deadtime用来设置断掉一个没有打开任何文件的连接的时间。单位是分钟，0代表Samba Server不自动切断任何连接
; deadtime = 0
# time server用来设置让nmdb成为windows客户端的时间服务器
; time server = yes/no
# 设置Samba Server日志文件的存储位置以及日志文件名称
; log file = /var/log/samba/log.%m
# 设置Samba Server日志文件的最大容量，单位为kB，0代表不限制
; max log size = 50
# 设置用户访问Samba Server的验证方式，一共有四种验证方式
# 1)share：用户访问Samba Server不需要提供用户名和口令, 安全性能较低
# 2)user：Samba Server共享目录只能被授权的用户访问,由Samba Server负责检查账号和密码的正确性。账号和密码要在本Samba Server中建立
# 3)server：依靠其他Windows NT/2000或Samba Server来验证用户的账号和密码,是一种代理验证。此种安全模式下,系统管理员可以把所有的Windows用户和口令集中到一个NT系统上,使用Windows NT进行Samba认证, 远程服务器可以自动认证全部用户和口令,如果认证失败,Samba将使用用户级安全模式作为替代的方式
# 4)domain：域安全级别,使用主域控制器(PDC)来完成认证
security = share
# passdb backend就是用户后台的意思。目前有三种后台：smbpasswd、tdbsam和ldapsam。sam应该是security account manager（安全账户管理）的简写
# 1)smbpasswd：该方式是使用smb自己的工具smbpasswd来给系统用户（真实用户或者虚拟用户）设置一个Samba密码，客户端就用这个密码来访问Samba的资源。smbpasswd文件默认在/etc/samba目录下，不过有时候要手工建立该文件
# 2)tdbsam：该方式则是使用一个数据库文件来建立用户数据库。数据库文件叫passdb.tdb，默认在/etc/samba目录下。passdb.tdb用户数据库可以使用smbpasswd –a来建立Samba用户，不过要建立的Samba用户必须先是系统用户。我们也可以使用pdbedit命令来建立Samba账户。pdbedit命令的参数很多，列出几个主要的
# pdbedit –a username：新建Samba账户。
# pdbedit –x username：删除Samba账户。
# pdbedit –L：列出Samba用户列表，读取passdb.tdb数据库文件。
# pdbedit –Lv：列出Samba用户列表的详细信息。
# pdbedit –c “[D]” –u username：暂停该Samba用户的账号。
# pdbedit –c “[]” –u username：恢复该Samba用户的账号。
# 3)ldapsam：该方式则是基于LDAP的账户管理方式来验证用户。首先要建立LDAP服务，然后设置“passdb backend = ldapsam:ldap://LDAP Server”
passdb backend = tdbsam
# 是否将认证密码加密。因为现在windows操作系统都是使用加密密码，所以一般要开启此项。不过配置文件默认已开启
; encrypt passwords = yes/no
# 用来定义samba用户的密码文件。smbpasswd文件如果没有那就要手工新建
; smb passwd file = /etc/samba/smbpasswd
# 用来定义用户名映射，比如可以将root换成administrator、admin等。不过要事先在smbusers文件中定义好。比如：root = administrator admin，这样就可以用administrator或admin这两个用户来代替root登陆Samba Server，更贴近windows用户的习惯
; username map = /etc/samba/smbusers
# 用来设置guest用户名
; guest account = guest
# 用来设置服务器和客户端之间会话的Socket选项，可以优化传输速度
socket options = TCP_NODELAY SO_RCVBUF=8192 SO_SNDBUF=8192
# 设置Samba服务器是否要成为网域主浏览器，网域主浏览器可以管理跨子网域的浏览服务
; domain master = yes/no
# local master用来指定Samba Server是否试图成为本地网域主浏览器。如果设为no，则永远不会成为本地网域主浏览器。但是即使设置为yes，也不等于该Samba Server就能成为主浏览器，还需要参加选举
;local master = yes/no
# 设置Samba Server一开机就强迫进行主浏览器选举，可以提高Samba Server成为本地网域主浏览器的机会。如果该参数指定为yes时，最好把domain master也指定为yes。使用该参数时要注意：如果在本Samba Server所在的子网有其他的机器（不论是windows NT还是其他Samba Server）也指定为首要主浏览器时，那么这些机器将会因为争夺主浏览器而在网络上大发广播，影响网络性能。如果同一个区域内有多台Samba Server，将上面三个参数设定在一台即可
; preferred master = yes/no
# 设置samba服务器的os level。该参数决定Samba Server是否有机会成为本地网域的主浏览器。os level从0到255，winNT的os level是32，win95/98的os level是1。Windows 2000的os level是64。如果设置为0，则意味着Samba Server将失去浏览选择。如果想让Samba Server成为PDC，那么将它的os level值设大些
; os level = 200
# 设置Samba Server是否要做为本地域控制器。主域控制器和备份域控制器都需要开启此项
; domain logons = yes/no
# 当使用者用windows客户端登陆，那么Samba将提供一个登陆档。如果设置成%u.bat，那么就要为每个用户提供一个登陆档。如果人比较多，那就比较麻烦。可以设置成一个具体的文件名，比如start.bat，那么用户登陆后都会去执行start.bat，而不用为每个用户设定一个登陆档了。这个文件要放置在[netlogon]的path设置的目录路径下
; logon . = %u.bat
# 设置samba服务器是否提供wins服务
; wins support = yes/no
# 设置Samba Server是否使用别的wins服务器提供wins服务
; wins server = wins serviceIP
# 设置Samba Server是否开启wins代理服务
; wins proxy = yes/no
# 设置Samba Server是否开启dns代理服务
; dns proxy = yes/no
# 设置是否在启动Samba时就共享打印机
; load printers = yes/no
# 设置共享打印机的配置文件
; printcap name = cups
# 设置Samba共享打印机的类型。现在支持的打印系统有：bsd, sysv, plp, lprng, aix, hpux, qnx
; printing = cups

#                       共享参数：
#================== Share Definitions ==================
[sharename]

# comment是对该共享的描述，可以是任意字符串
; comment = sharewindows
# path用来指定共享目录的路径,可以用%u、%m这样的宏来代替路径里的unix用户和客户机的Netbios名，用宏表示主要用于[homes]共享域。例如：如果我们不打算用home段做为客户的共享，而是在/home/share/下为每个Linux用户以他的用户名建个目录，作为他的共享目录，这样path就可以写成：path = /home/share/%u; 。用户在连接到这共享时具体的路径会被他的用户名代替，要注意这个用户名路径一定要存在，否则，客户机在访问时会找不到网络路径。同样，如果我们不是以用户来划分目录，而是以客户机来划分目录，为网络上每台可以访问samba的机器都各自建个以它的netbios名的路径，作为不同机器的共享资源，就可以这样写：path = /home/share/%m
path = /tftpboot
# browseable用来指定该共享是否可以浏览
; browseable = yes/no
# writable用来指定该共享路径是否可写
; writable = yes/no
# available用来指定该共享资源是否可用
available = yes
# admin users用来指定该共享的管理员（对该共享具有完全控制权限）。在samba 3.0中，如果用户验证方式设置成“security=share”时，此项无效。例如：admin users =bobyuan，jane（多个用户中间用逗号隔开）
; admin users = admin, usernames
# valid users用来指定允许访问该共享资源的用户。例如：valid users = bobyuan，@bob，@tech（多个用户或者组中间用逗号隔开，如果要加入一个组就用“@+组名”表示。）
; valid users = admin, @home
# invalid users用来指定不允许访问该共享资源的用户。例如：invalid users = root，@bob（多个用户或者组中间用逗号隔开）
; invalid users = root，@home
# write list用来指定可以在该共享下写入文件的用户
; write list = jake，@home
# public用来指定该共享是否允许guest账户访问
; public = yes/no
# guest ok 同“public”
; guest ok = yes/no

#>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>
# 几个特殊共享：
; [homes]
; comment = Home Directories
; browseable = no
; writable = yes
; valid users = %S
; valid users = MYDOMAIN\%S
 
; [printers]
; comment = All Printers
; path = /var/spool/samba
; browseable = no
; guest ok = no
; writable = no
; printable = yes
 
; [netlogon]
; comment = Network Logon Service
; path = /var/lib/samba/netlogon
; guest ok = yes
; writable = no
; share modes = no
 
; [Profiles]
; path = /var/lib/samba/profiles
; browseable = no
; guest ok = yes
# 🔚end
```

