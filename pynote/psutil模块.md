# psutil 模块

psutil 用来获取系统信息,用于监控系统运行状态

### 获取cpu的信息

```shell
In [1]: import psutil
#获取逻辑数量
In [2]: psutil.cpu_count()
Out[2]: 8
#获取物理核心数
In [3]: psutil.cpu_count(logical=False)
Out[3]: 4
```

### 统计CPU的用户/系统/空闲时间:

```shell
In [4]: psutil.cpu_times()
Out[4]: scputimes(user=12402.99, nice=0.0, system=6613.09, idle=186003.89)
```

### 类似top命令的cpu使用率,每秒刷新一次,累计10次

```shell
In [6]: for x in range(10):
   ...:     print(psutil.cpu_percent(interval=1,percpu=True))
   ...:
[24.2, 1.0, 15.0, 1.0, 14.0, 1.0, 10.0, 0.0]
[23.0, 1.0, 19.0, 1.0, 13.0, 1.0, 11.0, 2.0]
[31.4, 2.0, 23.8, 2.0, 20.8, 1.0, 18.8, 2.0]
[33.0, 2.0, 25.7, 2.0, 24.0, 1.0, 20.0, 1.0]
[24.0, 1.0, 18.2, 0.0, 13.0, 2.0, 10.0, 1.0]
[21.8, 1.0, 19.0, 1.0, 12.9, 0.0, 12.0, 0.0]
[30.3, 2.0, 23.8, 4.0, 18.0, 2.0, 14.0, 2.0]
[41.6, 3.0, 36.6, 3.0, 32.0, 4.0, 30.7, 3.0]
[36.6, 3.0, 28.0, 3.0, 23.8, 1.0, 23.0, 2.0]
[34.0, 6.9, 26.0, 4.0, 20.0, 5.0, 20.0, 5.9]

```

### 获取物理内存和交换内存信息

```python
#物理内存
In [7]: psutil.virtual_memory()
Out[7]: svmem(total=17179869184, available=7482253312, percent=56.4, used=9386016768, free=597528576, active=6884356096, inactive=6573760512, wired=2501660672)
#交换内存
In [8]: psutil.swap_memory()
Out[8]: sswap(total=0, used=0, free=0, percent=0.0, sin=8420012032, sout=65118208)
```

### 获取磁盘信息

```shell
#获取磁盘信息
In [10]: psutil.disk_partitions()
Out[10]:
[sdiskpart(device='/dev/disk1s5s1', mountpoint='/', fstype='apfs', opts='ro,local,rootfs,dovolfs,journaled,multilabel', maxfile=255, maxpath=1024),
 sdiskpart(device='/dev/disk1s4', mountpoint='/System/Volumes/VM', fstype='apfs', opts='rw,noexec,local,dovolfs,dontbrowse,journaled,multilabel,noatime', maxfile=255, maxpath=1024),
 sdiskpart(device='/dev/disk1s2', mountpoint='/System/Volumes/Preboot', fstype='apfs', opts='rw,local,dovolfs,dontbrowse,journaled,multilabel', maxfile=255, maxpath=1024),
 sdiskpart(device='/dev/disk1s6', mountpoint='/System/Volumes/Update', fstype='apfs', opts='rw,local,dovolfs,dontbrowse,journaled,multilabel', maxfile=255, maxpath=1024),
 sdiskpart(device='/dev/disk1s1', mountpoint='/System/Volumes/Data', fstype='apfs', opts='rw,local,dovolfs,dontbrowse,journaled,multilabel', maxfile=255, maxpath=1024)]
 
 #获取磁盘使用情况
In [13]: psutil.disk_usage('/')
Out[13]: sdiskusage(total=499963174912, used=15331123200, free=444596436992, percent=3.3)

#获取磁盘IO
In [15]: psutil.disk_io_counters()
Out[15]: sdiskio(read_count=1905918, write_count=715337, read_bytes=16326389760, write_bytes=10883731456, read_time=801003, write_time=318445)
```

### 获取网络接口和连接信息

```shell
#获取网络读写字节/包的个数
In [16]: psutil.net_io_counters()
Out[16]: snetio(bytes_sent=553115648, bytes_recv=1393315840, packets_sent=1108777, packets_recv=1265272, errin=0, errout=2269, dropin=0, dropout=0)

#获取网络接口信息
In [18]: psutil.net_if_addrs()
Out[18]:
{'lo0': [snicaddr(family=<AddressFamily.AF_INET: 2>, address='127.0.0.1', netmask='255.0.0.0', broadcast=None, ptp=None),
  snicaddr(family=<AddressFamily.AF_INET6: 30>, address='::1', netmask='ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff', broadcast=None, ptp=None),
  snicaddr(family=<AddressFamily.AF_INET6: 30>, address='fe80::1%lo0', netmask='ffff:ffff:ffff:ffff::', broadcast=None, ptp=None)],
 'en0': [snicaddr(family=<AddressFamily.AF_INET: 2>, address='10.79.54.20', netmask='255.255.252.0', broadcast='10.79.55.255', ptp=None),
  snicaddr(family=<AddressFamily.AF_LINK: 18>, address='f8:ff:c2:68:05:25', netmask=None, broadcast=None, ptp=None),
  snicaddr(family=<AddressFamily.AF_INET6: 30>, address='fe80::14d4:86a9:f320:ba5%en0', netmask='ffff:ffff:ffff:ffff::', broadcast=None, ptp=None)],
 'en4': [snicaddr(family=<AddressFamily.AF_LINK: 18>, address='ac:de:48:00:11:22', netmask=None, broadcast=None, ptp=None),
  snicaddr(family=<AddressFamily.AF_INET6: 30>, address='fe80::aede:48ff:fe00:1122%en4', netmask='ffff:ffff:ffff:ffff::', broadcast=None, ptp=None)],
 'ap1': [snicaddr(family=<AddressFamily.AF_LINK: 18>, address='fa:ff:c2:68:05:25', netmask=None, broadcast=None, ptp=None)],
 'en1': [snicaddr(family=<AddressFamily.AF_LINK: 18>, address='82:bf:eb:07:48:01', netmask=None, broadcast=None, ptp=None)],
 'en2': [snicaddr(family=<AddressFamily.AF_LINK: 18>, address='82:bf:eb:07:48:00', netmask=None, broadcast=None, ptp=None)],
 'bridge0': [snicaddr(family=<AddressFamily.AF_LINK: 18>, address='82:bf:eb:07:48:01', netmask=None, broadcast=None, ptp=None)],
 'awdl0': [snicaddr(family=<AddressFamily.AF_LINK: 18>, address='de:66:5f:0b:e1:b3', netmask=None, broadcast=None, ptp=None),
  snicaddr(family=<AddressFamily.AF_INET6: 30>, address='fe80::dc66:5fff:fe0b:e1b3%awdl0', netmask='ffff:ffff:ffff:ffff::', broadcast=None, ptp=None)],
 'llw0': [snicaddr(family=<AddressFamily.AF_LINK: 18>, address='de:66:5f:0b:e1:b3', netmask=None, broadcast=None, ptp=None),
  snicaddr(family=<AddressFamily.AF_INET6: 30>, address='fe80::dc66:5fff:fe0b:e1b3%llw0', netmask='ffff:ffff:ffff:ffff::', broadcast=None, ptp=None)],
 'utun0': [snicaddr(family=<AddressFamily.AF_INET6: 30>, address='fe80::8ca:1ba1:aaa0:bee6%utun0', netmask='ffff:ffff:ffff:ffff::', broadcast=None, ptp=None)],
 'utun1': [snicaddr(family=<AddressFamily.AF_INET6: 30>, address='fe80::c72a:c10b:87c0:863e%utun1', netmask='ffff:ffff:ffff:ffff::', broadcast=None, ptp=None)],
 'utun2': [snicaddr(family=<AddressFamily.AF_INET6: 30>, address='fe80::c9e5:e811:fa29:15bf%utun2', netmask='ffff:ffff:ffff:ffff::', broadcast=None, ptp=None)],
 'utun3': [snicaddr(family=<AddressFamily.AF_INET6: 30>, address='fe80::cefe:455a:b073:d4ff%utun3', netmask='ffff:ffff:ffff:ffff::', broadcast=None, ptp=None)]}
 
 #获取网络接口状态
 In [19]: psutil.net_if_stats()
Out[19]:
{'lo0': snicstats(isup=True, duplex=<NicDuplex.NIC_DUPLEX_UNKNOWN: 0>, speed=0, mtu=16384),
 'gif0': snicstats(isup=False, duplex=<NicDuplex.NIC_DUPLEX_UNKNOWN: 0>, speed=0, mtu=1280),
 'stf0': snicstats(isup=False, duplex=<NicDuplex.NIC_DUPLEX_UNKNOWN: 0>, speed=0, mtu=1280),
 'en4': snicstats(isup=True, duplex=<NicDuplex.NIC_DUPLEX_FULL: 2>, speed=100, mtu=1500),
 'ap1': snicstats(isup=False, duplex=<NicDuplex.NIC_DUPLEX_UNKNOWN: 0>, speed=0, mtu=1500),
 'en0': snicstats(isup=True, duplex=<NicDuplex.NIC_DUPLEX_UNKNOWN: 0>, speed=0, mtu=1500),
 'en1': snicstats(isup=True, duplex=<NicDuplex.NIC_DUPLEX_FULL: 2>, speed=0, mtu=1500),
 'en2': snicstats(isup=True, duplex=<NicDuplex.NIC_DUPLEX_FULL: 2>, speed=0, mtu=1500),
 'bridge0': snicstats(isup=True, duplex=<NicDuplex.NIC_DUPLEX_UNKNOWN: 0>, speed=0, mtu=1500),
 'awdl0': snicstats(isup=True, duplex=<NicDuplex.NIC_DUPLEX_UNKNOWN: 0>, speed=0, mtu=1500),
 'llw0': snicstats(isup=True, duplex=<NicDuplex.NIC_DUPLEX_UNKNOWN: 0>, speed=0, mtu=1500),
 'utun0': snicstats(isup=True, duplex=<NicDuplex.NIC_DUPLEX_UNKNOWN: 0>, speed=0, mtu=1380),
 'utun1': snicstats(isup=True, duplex=<NicDuplex.NIC_DUPLEX_UNKNOWN: 0>, speed=0, mtu=2000),
 'utun2': snicstats(isup=True, duplex=<NicDuplex.NIC_DUPLEX_UNKNOWN: 0>, speed=0, mtu=1380),
 'utun3': snicstats(isup=True, duplex=<NicDuplex.NIC_DUPLEX_UNKNOWN: 0>, speed=0, mtu=1380)}
 
 #获取当前网络连接信息(如果报错请使用sudo尝试)
 In [3]: psutil.net_connections()
Out[3]:
[sconn(fd=7, family=<AddressFamily.AF_INET: 2>, type=<SocketKind.SOCK_DGRAM: 2>, laddr=addr(ip='0.0.0.0', port=0), raddr=(), status='NONE', pid=17524),
 sconn(fd=8, family=<AddressFamily.AF_INET: 2>, type=<SocketKind.SOCK_DGRAM: 2>, laddr=addr(ip='0.0.0.0', port=0), raddr=(), status='NONE', pid=17524),
 sconn(fd=12, family=<AddressFamily.AF_INET: 2>, type=<SocketKind.SOCK_DGRAM: 2>, laddr=addr(ip='0.0.0.0', port=0), raddr=(), status='NONE', pid=17524),
 sconn(fd=13, family=<AddressFamily.AF_INET: 2>, type=<SocketKind.SOCK_DGRAM: 2>, laddr=addr(ip='0.0.0.0', port=0), raddr=(), status='NONE', pid=17524),
 sconn(fd=5, family=<AddressFamily.AF_INET: 2>, type=<SocketKind.SOCK_STREAM: 1>, laddr=addr(ip='10.79.54.20', port=49604), raddr=addr(ip='17.56.48.13', port=443), status='CLOSE', pid=1674),
 sconn(fd=13, family=<AddressFamily.AF_INET: 2>, type=<SocketKind.SOCK_STREAM: 1>, laddr=addr(ip='10.79.54.20', port=49606), raddr=addr(ip='67.195.204.56', port=443), status='CLOSE', pid=1670),
 sconn(fd=16, family=<AddressFamily.AF_INET: 2>, type=<SocketKind.SOCK_STREAM: 1>, laddr=addr(ip='10.79.54.20', port=49610), raddr=addr(ip='103.254.191.161', port=443), status='CLOSE', pid=1670),
 sconn(fd=20, family=<AddressFamily.AF_INET: 2>, type=<SocketKind.SOCK_STREAM: 1>, laddr=addr(ip='10.79.54.20', port=54967), raddr=addr(ip='10.1.0.200', port=443), status='CLOSE_WAIT', pid=1050),
 sconn(fd=21, family=<AddressFamily.AF_INET: 2>, type=<SocketKind.SOCK_STREAM: 1>, laddr=addr(ip='10.79.54.20', port=53571), raddr=addr(ip='40.90.189.152', port=443), status='ESTABLISHED', pid=1050),
 sconn(fd=23, family=<AddressFamily.AF_INET: 2>, type=<SocketKind.SOCK_STREAM: 1>, laddr=addr(ip='10.79.54.20', port=54968), raddr=addr(ip='10.1.0.200', port=443), status='CLOSE_WAIT', pid=1050),
 sconn(fd=24, family=<AddressFamily.AF_INET: 2>, type=<SocketKind.SOCK_STREAM: 1>, laddr=addr(ip='10.79.54.20', port=54939), raddr=addr(ip='49.67.72.239', port=443), status='CLOSE', pid=1050),
 sconn(fd=25, family=<AddressFamily.AF_INET: 2>, type=<SocketKind.SOCK_STREAM: 1>, laddr=addr(ip='10.79.54.20', port=54993), raddr=addr(ip='142.251.10.154', port=443), status='ESTABLISHED', pid=1050),
 sconn(fd=27, family=<AddressFamily.AF_INET: 2>, type=<SocketKind.SOCK_STREAM: 1>, laddr=addr(ip='10.79.54.20', port=54995), raddr=addr(ip='202.89.233.100', port=443), status='ESTABLISHED', pid=1050),
 sconn(fd=38, family=<AddressFamily.AF_INET: 2>, type=<SocketKind.SOCK_STREAM: 1>, laddr=addr(ip='10.79.54.20', port=54970), raddr=addr(ip='10.1.0.200', port=443), status='ESTABLISHED', pid=1050),
 sconn(fd=55, family=<AddressFamily.AF_INET: 2>, type=<SocketKind.SOCK_STREAM: 1>, laddr=addr(ip='10.79.54.20', port=54991), raddr=addr(ip='220.181.33.11', port=443), status='CLOSE', pid=1050),
 sconn(fd=57, family=<AddressFamily.AF_INET: 2>, type=<SocketKind.SOCK_STREAM: 1>, laddr=addr(ip='10.79.54.20', port=54992), raddr=addr(ip='220.181.33.11', port=443), status='CLOSE', pid=1050),
 sconn(fd=73, family=<AddressFamily.AF_INET6: 30>, type=<SocketKind.SOCK_DGRAM: 2>, laddr=addr(ip='::', port=5353), raddr=(), status='NONE', pid=1050),
 sconn(fd=74, family=<AddressFamily.AF_INET: 2>, type=<SocketKind.SOCK_DGRAM: 2>, laddr=addr(ip='0.0.0.0', port=5353), raddr=(), status='NONE', pid=1050),
 sconn(fd=75, family=<AddressFamily.AF_INET6: 30>, type=<SocketKind.SOCK_DGRAM: 2>, laddr=addr(ip='::', port=5353), raddr=(), status='NONE', pid=1050),
 sconn(fd=76, family=<AddressFamily.AF_INET6: 30>, type=<SocketKind.SOCK_DGRAM: 2>, laddr=addr(ip='::', port=5353), raddr=(), status='NONE', pid=1050),
 sconn(fd=77, family=<AddressFamily.AF_INET6: 30>, type=<SocketKind.SOCK_DGRAM: 2>, laddr=addr(ip='::', port=5353), raddr=(), status='NONE', pid=1050),
 sconn(fd=78, family=<AddressFamily.AF_INET6: 30>, type=<SocketKind.SOCK_DGRAM: 2>, laddr=addr(ip='::', port=5353), raddr=(), status='NONE', pid=1050),
 sconn(fd=79, family=<AddressFamily.AF_INET6: 30>, type=<SocketKind.SOCK_DGRAM: 2>, laddr=addr(ip='::', port=5353), raddr=(), status='NONE', pid=1050),
 sconn(fd=17, family=<AddressFamily.AF_INET: 2>, type=<SocketKind.SOCK_STREAM: 1>, laddr=addr(ip='10.79.54.20', port=53573), raddr=addr(ip='27.185.215.236', port=443), status='ESTABLISHED', pid=627),
 sconn(fd=96, family=<AddressFamily.AF_INET: 2>, type=<SocketKind.SOCK_STREAM: 1>, laddr=addr(ip='10.79.54.20', port=54966), raddr=addr(ip='27.128.223.243', port=443), status='CLOSE_WAIT', pid=626),
 sconn(fd=135, family=<AddressFamily.AF_INET: 2>, type=<SocketKind.SOCK_STREAM: 1>, laddr=addr(ip='10.79.54.20', port=53572), raddr=addr(ip='122.14.230.144', port=443), status='ESTABLISHED', pid=626),
 sconn(fd=169, family=<AddressFamily.AF_INET: 2>, type=<SocketKind.SOCK_STREAM: 1>, laddr=addr(ip='10.79.54.20', port=54874), raddr=addr(ip='140.249.88.206', port=443), status='ESTABLISHED', pid=626),
 sconn(fd=173, family=<AddressFamily.AF_INET: 2>, type=<SocketKind.SOCK_STREAM: 1>, laddr=addr(ip='10.79.54.20', port=54684), raddr=addr(ip='101.36.166.19', port=443), status='ESTABLISHED', pid=626),
 sconn(fd=183, family=<AddressFamily.AF_INET: 2>, type=<SocketKind.SOCK_STREAM: 1>, laddr=addr(ip='10.79.54.20', port=54059), raddr=addr(ip='58.58.80.171', port=443), status='ESTABLISHED', pid=626),
 sconn(fd=187, family=<AddressFamily.AF_INET: 2>, type=<SocketKind.SOCK_DGRAM: 2>, laddr=addr(ip='10.79.54.20', port=52358), raddr=addr(ip='221.194.186.47', port=443), status='NONE', pid=626),
 sconn(fd=243, family=<AddressFamily.AF_INET: 2>, type=<SocketKind.SOCK_DGRAM: 2>, laddr=addr(ip='0.0.0.0', port=0), raddr=(), status='NONE', pid=626),
 sconn(fd=12, family=<AddressFamily.AF_INET: 2>, type=<SocketKind.SOCK_STREAM: 1>, laddr=addr(ip='127.0.0.1', port=32094), raddr=(), status='LISTEN', pid=597),
 sconn(fd=26, family=<AddressFamily.AF_INET: 2>, type=<SocketKind.SOCK_STREAM: 1>, laddr=addr(ip='127.0.0.1', port=32094), raddr=(), status='LISTEN', pid=597),
 sconn(fd=3, family=<AddressFamily.AF_INET: 2>, type=<SocketKind.SOCK_DGRAM: 2>, laddr=addr(ip='0.0.0.0', port=137), raddr=(), status='NONE', pid=584),
 sconn(fd=4, family=<AddressFamily.AF_INET: 2>, type=<SocketKind.SOCK_DGRAM: 2>, laddr=addr(ip='0.0.0.0', port=138), raddr=(), status='NONE', pid=584),
 sconn(fd=8, family=<AddressFamily.AF_INET: 2>, type=<SocketKind.SOCK_STREAM: 1>, laddr=addr(ip='127.0.0.1', port=46624), raddr=(), status='LISTEN', pid=574),
 sconn(fd=24, family=<AddressFamily.AF_INET: 2>, type=<SocketKind.SOCK_STREAM: 1>, laddr=addr(ip='10.79.54.20', port=54734), raddr=addr(ip='34.107.247.156', port=443), status='ESTABLISHED', pid=574),
 sconn(fd=25, family=<AddressFamily.AF_INET: 2>, type=<SocketKind.SOCK_STREAM: 1>, laddr=addr(ip='10.79.54.20', port=54748), raddr=addr(ip='172.217.194.100', port=80), status='ESTABLISHED', pid=574),
 sconn(fd=3, family=<AddressFamily.AF_INET: 2>, type=<SocketKind.SOCK_DGRAM: 2>, laddr=addr(ip='0.0.0.0', port=0), raddr=(), status='NONE', pid=555),
 sconn(fd=43, family=<AddressFamily.AF_INET: 2>, type=<SocketKind.SOCK_STREAM: 1>, laddr=addr(ip='10.79.54.20', port=53582), raddr=addr(ip='58.216.14.239', port=443), status='ESTABLISHED', pid=532),
 sconn(fd=47, family=<AddressFamily.AF_INET: 2>, type=<SocketKind.SOCK_STREAM: 1>, laddr=addr(ip='10.79.54.20', port=53586), raddr=addr(ip='27.185.215.241', port=443), status='ESTABLISHED', pid=532),
 sconn(fd=73, family=<AddressFamily.AF_INET: 2>, type=<SocketKind.SOCK_STREAM: 1>, laddr=addr(ip='10.79.54.20', port=54924), raddr=addr(ip='115.238.201.226', port=443), status='ESTABLISHED', pid=532),
 sconn(fd=78, family=<AddressFamily.AF_INET: 2>, type=<SocketKind.SOCK_STREAM: 1>, laddr=addr(ip='10.79.54.20', port=54925), raddr=addr(ip='116.211.183.214', port=443), status='ESTABLISHED', pid=532),
 sconn(fd=117, family=<AddressFamily.AF_INET: 2>, type=<SocketKind.SOCK_STREAM: 1>, laddr=addr(ip='10.79.54.20', port=54182), raddr=addr(ip='122.14.230.144', port=443), status='ESTABLISHED', pid=532),
 sconn(fd=4, family=<AddressFamily.AF_INET6: 30>, type=<SocketKind.SOCK_STREAM: 1>, laddr=addr(ip='fe80:4::aede:48ff:fe00:1122', port=53575), raddr=addr(ip='fe80:4::aede:48ff:fe33:4455', port=49352), status='ESTABLISHED', pid=522),
 sconn(fd=3, family=<AddressFamily.AF_INET: 2>, type=<SocketKind.SOCK_DGRAM: 2>, laddr=addr(ip='0.0.0.0', port=0), raddr=(), status='NONE', pid=521),
 sconn(fd=4, family=<AddressFamily.AF_INET: 2>, type=<SocketKind.SOCK_DGRAM: 2>, laddr=addr(ip='0.0.0.0', port=0), raddr=(), status='NONE', pid=501),
 sconn(fd=8, family=<AddressFamily.AF_INET: 2>, type=<SocketKind.SOCK_DGRAM: 2>, laddr=addr(ip='0.0.0.0', port=0), raddr=(), status='NONE', pid=501),
 sconn(fd=9, family=<AddressFamily.AF_INET: 2>, type=<SocketKind.SOCK_DGRAM: 2>, laddr=addr(ip='0.0.0.0', port=0), raddr=(), status='NONE', pid=501),
 sconn(fd=10, family=<AddressFamily.AF_INET: 2>, type=<SocketKind.SOCK_DGRAM: 2>, laddr=addr(ip='0.0.0.0', port=0), raddr=(), status='NONE', pid=501),
 sconn(fd=11, family=<AddressFamily.AF_INET: 2>, type=<SocketKind.SOCK_DGRAM: 2>, laddr=addr(ip='0.0.0.0', port=0), raddr=(), status='NONE', pid=501),
 sconn(fd=22, family=<AddressFamily.AF_INET: 2>, type=<SocketKind.SOCK_DGRAM: 2>, laddr=addr(ip='0.0.0.0', port=0), raddr=(), status='NONE', pid=501),
 sconn(fd=23, family=<AddressFamily.AF_INET: 2>, type=<SocketKind.SOCK_DGRAM: 2>, laddr=addr(ip='0.0.0.0', port=0), raddr=(), status='NONE', pid=501),
 sconn(fd=3, family=<AddressFamily.AF_INET: 2>, type=<SocketKind.SOCK_DGRAM: 2>, laddr=addr(ip='0.0.0.0', port=0), raddr=(), status='NONE', pid=500),
 sconn(fd=4, family=<AddressFamily.AF_INET: 2>, type=<SocketKind.SOCK_DGRAM: 2>, laddr=addr(ip='0.0.0.0', port=0), raddr=(), status='NONE', pid=500),
 sconn(fd=5, family=<AddressFamily.AF_INET: 2>, type=<SocketKind.SOCK_DGRAM: 2>, laddr=addr(ip='0.0.0.0', port=0), raddr=(), status='NONE', pid=500),
 sconn(fd=5, family=<AddressFamily.AF_INET: 2>, type=<SocketKind.SOCK_DGRAM: 2>, laddr=addr(ip='0.0.0.0', port=0), raddr=(), status='NONE', pid=484),
 sconn(fd=26, family=<AddressFamily.AF_INET: 2>, type=<SocketKind.SOCK_DGRAM: 2>, laddr=addr(ip='0.0.0.0', port=0), raddr=(), status='NONE', pid=468),
 sconn(fd=37, family=<AddressFamily.AF_INET6: 30>, type=<SocketKind.SOCK_STREAM: 1>, laddr=addr(ip='fe80:e::c9e5:e811:fa29:15bf', port=1024), raddr=addr(ip='fe80:e::b3b5:b13:f3e7:1466', port=1024), status='SYN_SENT', pid=468),
 sconn(fd=5, family=<AddressFamily.AF_INET: 2>, type=<SocketKind.SOCK_STREAM: 1>, laddr=addr(ip='0.0.0.0', port=53574), raddr=(), status='LISTEN', pid=456),
 sconn(fd=6, family=<AddressFamily.AF_INET6: 30>, type=<SocketKind.SOCK_STREAM: 1>, laddr=addr(ip='::', port=53574), raddr=(), status='LISTEN', pid=456),
 sconn(fd=7, family=<AddressFamily.AF_INET: 2>, type=<SocketKind.SOCK_DGRAM: 2>, laddr=addr(ip='0.0.0.0', port=0), raddr=(), status='NONE', pid=456),
 sconn(fd=8, family=<AddressFamily.AF_INET: 2>, type=<SocketKind.SOCK_DGRAM: 2>, laddr=addr(ip='0.0.0.0', port=0), raddr=(), status='NONE', pid=456),
 sconn(fd=12, family=<AddressFamily.AF_INET: 2>, type=<SocketKind.SOCK_DGRAM: 2>, laddr=addr(ip='0.0.0.0', port=0), raddr=(), status='NONE', pid=456),
 sconn(fd=16, family=<AddressFamily.AF_INET: 2>, type=<SocketKind.SOCK_DGRAM: 2>, laddr=addr(ip='0.0.0.0', port=0), raddr=(), status='NONE', pid=456),
 sconn(fd=3, family=<AddressFamily.AF_INET: 2>, type=<SocketKind.SOCK_DGRAM: 2>, laddr=addr(ip='0.0.0.0', port=0), raddr=(), status='NONE', pid=446),
 sconn(fd=3, family=<AddressFamily.AF_INET6: 30>, type=<SocketKind.SOCK_STREAM: 1>, laddr=addr(ip='fe80:4::aede:48ff:fe00:1122', port=49167), raddr=addr(ip='fe80:4::aede:48ff:fe33:4455', port=49342), status='ESTABLISHED', pid=429),
 sconn(fd=17, family=<AddressFamily.AF_INET: 2>, type=<SocketKind.SOCK_DGRAM: 2>, laddr=addr(ip='0.0.0.0', port=0), raddr=(), status='NONE', pid=405),
 sconn(fd=3, family=<AddressFamily.AF_INET6: 30>, type=<SocketKind.SOCK_STREAM: 1>, laddr=addr(ip='fe80:4::aede:48ff:fe00:1122', port=49161), raddr=addr(ip='fe80:4::aede:48ff:fe33:4455', port=49355), status='ESTABLISHED', pid=304),
 sconn(fd=3, family=<AddressFamily.AF_INET6: 30>, type=<SocketKind.SOCK_STREAM: 1>, laddr=addr(ip='fe80:4::aede:48ff:fe00:1122', port=49154), raddr=addr(ip='fe80:4::aede:48ff:fe33:4455', port=49360), status='ESTABLISHED', pid=272),
 sconn(fd=5, family=<AddressFamily.AF_INET6: 30>, type=<SocketKind.SOCK_STREAM: 1>, laddr=addr(ip='fe80:4::aede:48ff:fe00:1122', port=49166), raddr=addr(ip='fe80:4::aede:48ff:fe33:4455', port=49332), status='ESTABLISHED', pid=272),
 sconn(fd=4, family=<AddressFamily.AF_INET6: 30>, type=<SocketKind.SOCK_STREAM: 1>, laddr=addr(ip='fe80:4::aede:48ff:fe00:1122', port=49160), raddr=addr(ip='fe80:4::aede:48ff:fe33:4455', port=49345), status='ESTABLISHED', pid=271),
 sconn(fd=3, family=<AddressFamily.AF_INET6: 30>, type=<SocketKind.SOCK_STREAM: 1>, laddr=addr(ip='fe80:4::aede:48ff:fe00:1122', port=49619), raddr=addr(ip='fe80:4::aede:48ff:fe33:4455', port=49339), status='ESTABLISHED', pid=270),
 sconn(fd=6, family=<AddressFamily.AF_INET: 2>, type=<SocketKind.SOCK_DGRAM: 2>, laddr=addr(ip='0.0.0.0', port=5353), raddr=(), status='NONE', pid=249),
 sconn(fd=7, family=<AddressFamily.AF_INET6: 30>, type=<SocketKind.SOCK_DGRAM: 2>, laddr=addr(ip='::', port=5353), raddr=(), status='NONE', pid=249),
 sconn(fd=19, family=<AddressFamily.AF_INET: 2>, type=<SocketKind.SOCK_DGRAM: 2>, laddr=addr(ip='224.0.0.1', port=5350), raddr=(), status='NONE', pid=249),
 sconn(fd=37, family=<AddressFamily.AF_INET: 2>, type=<SocketKind.SOCK_DGRAM: 2>, laddr=addr(ip='0.0.0.0', port=49973), raddr=(), status='NONE', pid=249),
 sconn(fd=40, family=<AddressFamily.AF_INET6: 30>, type=<SocketKind.SOCK_DGRAM: 2>, laddr=addr(ip='::', port=49973), raddr=(), status='NONE', pid=249),
 sconn(fd=41, family=<AddressFamily.AF_INET: 2>, type=<SocketKind.SOCK_DGRAM: 2>, laddr=addr(ip='0.0.0.0', port=54371), raddr=(), status='NONE', pid=249),
 sconn(fd=46, family=<AddressFamily.AF_INET: 2>, type=<SocketKind.SOCK_DGRAM: 2>, laddr=addr(ip='0.0.0.0', port=56260), raddr=(), status='NONE', pid=249),
 sconn(fd=5, family=<AddressFamily.AF_INET: 2>, type=<SocketKind.SOCK_DGRAM: 2>, laddr=addr(ip='0.0.0.0', port=0), raddr=(), status='NONE', pid=238),
 sconn(fd=6, family=<AddressFamily.AF_INET: 2>, type=<SocketKind.SOCK_DGRAM: 2>, laddr=addr(ip='0.0.0.0', port=0), raddr=(), status='NONE', pid=238),
 sconn(fd=11, family=<AddressFamily.AF_INET: 2>, type=<SocketKind.SOCK_DGRAM: 2>, laddr=addr(ip='0.0.0.0', port=0), raddr=(), status='NONE', pid=238),
 sconn(fd=12, family=<AddressFamily.AF_INET6: 30>, type=<SocketKind.SOCK_DGRAM: 2>, laddr=addr(ip='::', port=0), raddr=(), status='NONE', pid=238),
 sconn(fd=14, family=<AddressFamily.AF_INET: 2>, type=<SocketKind.SOCK_DGRAM: 2>, laddr=addr(ip='0.0.0.0', port=0), raddr=(), status='NONE', pid=238),
 sconn(fd=19, family=<AddressFamily.AF_INET: 2>, type=<SocketKind.SOCK_DGRAM: 2>, laddr=addr(ip='0.0.0.0', port=0), raddr=(), status='NONE', pid=238),
 sconn(fd=23, family=<AddressFamily.AF_INET: 2>, type=<SocketKind.SOCK_DGRAM: 2>, laddr=addr(ip='0.0.0.0', port=0), raddr=(), status='NONE', pid=238),
 sconn(fd=27, family=<AddressFamily.AF_INET: 2>, type=<SocketKind.SOCK_DGRAM: 2>, laddr=addr(ip='0.0.0.0', port=0), raddr=(), status='NONE', pid=238),
 sconn(fd=30, family=<AddressFamily.AF_INET: 2>, type=<SocketKind.SOCK_DGRAM: 2>, laddr=addr(ip='0.0.0.0', port=0), raddr=(), status='NONE', pid=238),
 sconn(fd=4, family=<AddressFamily.AF_INET6: 30>, type=<SocketKind.SOCK_STREAM: 1>, laddr=addr(ip='fe80:4::aede:48ff:fe00:1122', port=49159), raddr=addr(ip='fe80:4::aede:48ff:fe33:4455', port=49347), status='ESTABLISHED', pid=204),
 sconn(fd=27, family=<AddressFamily.AF_INET: 2>, type=<SocketKind.SOCK_DGRAM: 2>, laddr=addr(ip='0.0.0.0', port=0), raddr=(), status='NONE', pid=203),
 sconn(fd=8, family=<AddressFamily.AF_INET: 2>, type=<SocketKind.SOCK_DGRAM: 2>, laddr=addr(ip='0.0.0.0', port=0), raddr=(), status='NONE', pid=156),
 sconn(fd=5, family=<AddressFamily.AF_INET: 2>, type=<SocketKind.SOCK_STREAM: 1>, laddr=addr(ip='127.0.0.1', port=27257), raddr=(), status='LISTEN', pid=144),
 sconn(fd=15, family=<AddressFamily.AF_INET: 2>, type=<SocketKind.SOCK_STREAM: 1>, laddr=addr(ip='10.79.54.20', port=53608), raddr=addr(ip='10.8.7.220', port=443), status='ESTABLISHED', pid=144),
 sconn(fd=6, family=<AddressFamily.AF_INET: 2>, type=<SocketKind.SOCK_DGRAM: 2>, laddr=addr(ip='0.0.0.0', port=0), raddr=(), status='NONE', pid=142),
 sconn(fd=29, family=<AddressFamily.AF_INET: 2>, type=<SocketKind.SOCK_STREAM: 1>, laddr=addr(ip='10.79.54.20', port=54963), raddr=addr(ip='54.209.32.124', port=443), status='ESTABLISHED', pid=128),
 sconn(fd=90, family=<AddressFamily.AF_INET: 2>, type=<SocketKind.SOCK_STREAM: 1>, laddr=addr(ip='10.79.54.20', port=54742), raddr=addr(ip='54.205.143.53', port=443), status='ESTABLISHED', pid=128),
 sconn(fd=7, family=<AddressFamily.AF_INET: 2>, type=<SocketKind.SOCK_DGRAM: 2>, laddr=addr(ip='0.0.0.0', port=0), raddr=(), status='NONE', pid=127),
 sconn(fd=6, family=<AddressFamily.AF_INET: 2>, type=<SocketKind.SOCK_STREAM: 1>, laddr=addr(ip='10.79.54.20', port=49173), raddr=addr(ip='17.57.145.169', port=5223), status='ESTABLISHED', pid=119),
 sconn(fd=3, family=<AddressFamily.AF_INET6: 30>, type=<SocketKind.SOCK_STREAM: 1>, laddr=addr(ip='fe80:4::aede:48ff:fe00:1122', port=49152), raddr=addr(ip='fe80:4::aede:48ff:fe33:4455', port=59602), status='ESTABLISHED', pid=92),
 sconn(fd=4, family=<AddressFamily.AF_INET6: 30>, type=<SocketKind.SOCK_STREAM: 1>, laddr=addr(ip='fe80:4::aede:48ff:fe00:1122', port=49153), raddr=(), status='LISTEN', pid=92),
 sconn(fd=5, family=<AddressFamily.AF_INET6: 30>, type=<SocketKind.SOCK_STREAM: 1>, laddr=addr(ip='fe80:4::aede:48ff:fe00:1122', port=49154), raddr=(), status='LISTEN', pid=92),
 sconn(fd=6, family=<AddressFamily.AF_INET6: 30>, type=<SocketKind.SOCK_STREAM: 1>, laddr=addr(ip='fe80:4::aede:48ff:fe00:1122', port=49155), raddr=(), status='LISTEN', pid=92),
 sconn(fd=7, family=<AddressFamily.AF_INET6: 30>, type=<SocketKind.SOCK_STREAM: 1>, laddr=addr(ip='fe80:4::aede:48ff:fe00:1122', port=49156), raddr=(), status='LISTEN', pid=92),
 sconn(fd=8, family=<AddressFamily.AF_INET6: 30>, type=<SocketKind.SOCK_STREAM: 1>, laddr=addr(ip='fe80:4::aede:48ff:fe00:1122', port=49157), raddr=(), status='LISTEN', pid=92),
 sconn(fd=9, family=<AddressFamily.AF_INET6: 30>, type=<SocketKind.SOCK_STREAM: 1>, laddr=addr(ip='fe80:4::aede:48ff:fe00:1122', port=49158), raddr=(), status='LISTEN', pid=92),
 sconn(fd=16, family=<AddressFamily.AF_INET: 2>, type=<SocketKind.SOCK_DGRAM: 2>, laddr=addr(ip='0.0.0.0', port=0), raddr=(), status='NONE', pid=86),
 sconn(fd=17, family=<AddressFamily.AF_INET: 2>, type=<SocketKind.SOCK_DGRAM: 2>, laddr=addr(ip='0.0.0.0', port=0), raddr=(), status='NONE', pid=86),
 sconn(fd=20, family=<AddressFamily.AF_INET: 2>, type=<SocketKind.SOCK_DGRAM: 2>, laddr=addr(ip='0.0.0.0', port=0), raddr=(), status='NONE', pid=86),
 sconn(fd=28, family=<AddressFamily.AF_INET: 2>, type=<SocketKind.SOCK_DGRAM: 2>, laddr=addr(ip='0.0.0.0', port=0), raddr=(), status='NONE', pid=86),
 sconn(fd=17, family=<AddressFamily.AF_INET: 2>, type=<SocketKind.SOCK_DGRAM: 2>, laddr=addr(ip='0.0.0.0', port=0), raddr=(), status='NONE', pid=84),
 sconn(fd=32, family=<AddressFamily.AF_INET6: 30>, type=<SocketKind.SOCK_STREAM: 1>, laddr=addr(ip='::1', port=8021), raddr=(), status='LISTEN', pid=1),
 sconn(fd=33, family=<AddressFamily.AF_INET: 2>, type=<SocketKind.SOCK_STREAM: 1>, laddr=addr(ip='127.0.0.1', port=8021), raddr=(), status='LISTEN', pid=1),
 sconn(fd=34, family=<AddressFamily.AF_INET: 2>, type=<SocketKind.SOCK_DGRAM: 2>, laddr=addr(ip='0.0.0.0', port=137), raddr=(), status='NONE', pid=1),
 sconn(fd=35, family=<AddressFamily.AF_INET6: 30>, type=<SocketKind.SOCK_STREAM: 1>, laddr=addr(ip='::1', port=8021), raddr=(), status='LISTEN', pid=1),
 sconn(fd=36, family=<AddressFamily.AF_INET: 2>, type=<SocketKind.SOCK_STREAM: 1>, laddr=addr(ip='127.0.0.1', port=8021), raddr=(), status='LISTEN', pid=1),
 sconn(fd=37, family=<AddressFamily.AF_INET: 2>, type=<SocketKind.SOCK_DGRAM: 2>, laddr=addr(ip='0.0.0.0', port=138), raddr=(), status='NONE', pid=1)]
```

### 获取进程信息

```shell
# 获取所有进程ID
In [4]: psutil.pids()
Out[4]:
[0, 1, 74,75,78,79,6225,7859,8617,8620,21035]

#获取指定进程
In [5]: p = psutil.Process(78)

In [6]: print(p)
psutil.Process(pid=78, name='uninstalld', status='running', started='08:45:54')

#获取进程名称
In [5]: p = psutil.Process(78)

In [6]: print(p)
psutil.Process(pid=78, name='uninstalld', status='running', started='08:45:54')
#交互,进程名称
In [7]: p.name()
Out[7]: 'uninstalld'
#进程exe路径
In [8]: p.exe()
Out[8]: '/System/Library/PrivateFrameworks/Uninstall.framework/Versions/A/Resources/uninstalld'
#进程工作目录
In [9]: p.cwd()
Out[9]: '/'
#进程启动命令
In [10]: p.cmdline()
Out[10]: ['/System/Library/PrivateFrameworks/Uninstall.framework/Resources/uninstalld']
#进程父进程ID
In [11]: p.ppid()
Out[11]: 1
#父进程
In [12]: p.parent()
Out[12]: psutil.Process(pid=1, name='launchd', status='running', started='08:45:53')
#子进程
In [13]: p.children()
Out[13]: []
#进程运行状态
In [15]: p.status()
Out[15]: 'running'
#进程用户名
In [16]: p.username()
Out[16]: 'root'
进程创建时间
In [17]: p.create_time()
Out[17]: 1631925954.951902
#进程终端
In [25]: p.terminal()
#进程使用cpu时间
In [19]: p.cpu_times()
Out[19]: pcputimes(user=0.313381632, system=0.7332, children_user=0.0, children_system=0.0)
#进程使用的内存
In [20]: p.memory_info()
Out[20]: pmem(rss=2854912, vms=4454162432, pfaults=6543, pageins=4)
#进程打开的文件
In [21]: p.open_files()
Out[21]: []
#进程相关网络连接
In [22]: p.connections()
Out[22]: []
#进程的线程数量
In [23]: p.num_threads()
Out[23]: 3
#所有线程
In [24]: p.threads()
#进程环境变量
In [26]: p.environ()
Out[26]: {}
#结束进程
In [27]: p.terminate()

#psutil还提供了一个test()函数，可以模拟出ps命令的效果
psutil.test()
USER         PID  %MEM     VSZ     RSS  NICE STATUS  START   TIME  CMDLINE
root           0   6.6  197.9G    1.1G        runni  08:45  19:0
root           1   0.2    4.2G   28.9M        runni  08:45  01:1
root          74   0.0    4.2G    1.2M        runni  08:45  00:0
root          75   0.1    4.2G   11.6M        runni  08:45  00:0
root          78   0.0    4.1G    2.7M        runni  08:45  00:0
root          79   0.1    4.2G    9.6M        runni  08:45  00:4
root          80   0.1    4.2G   12.7M        runni  08:45  00:0
·······

```

可以支持各种系统的系统信息获取!

psutil官网:https://github.com/giampaolo/psutil