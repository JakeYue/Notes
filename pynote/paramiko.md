# paramiko 模块

实现SSH远程登录会话操作.

### **paramiko包含两个核心组件:SSHClient、SFTPClient**

> SSHClient的作用类似于Linux的ssh命令，是对SSH会话的封装，该类封装了传输(Transport)，通道(Channel)及SFTPClient建立的方法(open_sftp)，通常用于执行远程命令

- SFTPClient的作用类似与Linux的sftp命令，是对SFTP客户端的封装，用以实现远程文件操作，如文件上传、下载、修改文件权限等操作

paramiko基础名词:

- Channel：是一种类Socket，一种安全的SSH传输通道;
- Transport：是一种加密的会话，使用时会同步创建了一个加密的Tunnels(通道)，这个Tunnels叫做Channel;
- Session：是client与Server保持连接的对象，用connect()/start_client()/start_server()开始会话。

### **SSHClient常用的方法介绍**

```shell
# connect()：实现远程服务器的连接与认证，对于该方法只有hostname是必传参数

# 常用参数:
hostname 连接的目标主机
port=SSH_PORT 指定端口
username=None 验证的用户名
password=None 验证的用户密码
pkey=None 私钥方式用于身份验证
key_filename=None 一个文件名或文件列表，指定私钥文件
timeout=None 可选的tcp连接超时时间
allow_agent=True, 是否允许连接到ssh代理，默认为True 允许l
ook_for_keys=True 是否在~/.ssh中搜索私钥文件，默认为True 允许
compress=False, 是否打开压缩

# set_missing_host_key_policy()：设置远程服务器没有在know_hosts文件中记录时的应对策略。目前支持三种策略
AutoAddPolicy 自动添加主机名及主机密钥到本地HostKeys对象，不依赖

load_system_host_key的配置。即新建立ssh连接时不需要再输入yes或no进行确认
WarningPolicy 用于记录一个未知的主机密钥的python警告。并接受，功能上和AutoAddPolicy类似，但是会提示是新连接

RejectPolicy 自动拒绝未知的主机名和密钥，依赖load_system_host_key的配置。此为默认选项

# exec_command()：在远程服务器执行Linux命令的方法
```

![流程](https://cdn.jsdelivr.net/gh/JakeYue/picgo@master/uPic/%E6%88%AA%E5%B1%8F2021-09-21%20%E4%B8%8A%E5%8D%889.32.182021%2009%2021%2009%2034.png)

```shell
# open_sftp()：在当前ssh会话的基础上创建一个sftp会话。该方法会返回一个SFTPClient对象,利用SSHClient对象的open_sftp()方法，可以直接返回一个基于当前连接的sftp对象，可以进行文件的上传等操作.

sftp = client.open_sftp() 
sftp.put('test.txt','text.txt') 
```

# 案例

### **1. 使用sftp上传文件**

``` python
import paramiko 
#获取Transport实例 
tran = paramiko.Transport("172.25.254.31",22) 
#连接SSH服务端 
tran.connect(username = "root", password = "westos") 
#获取SFTP实例 
sftp = paramiko.SFTPClient.from_transport(tran) 
#设置上传的本地/远程文件路径 
localpath="passwd.html" ##本地文件路径 
remotepath="/home/kiosk/Desktop/fish" ##上传对象保存的文件路径 
#执行上传动作 
sftp.put(localpath,remotepath) 
tran.close() 
```

### 2. **使用sftp下载文件**

```python
import paramiko 
#获取SSHClient实例 
client = paramiko.SSHClient() 
client.set_missing_host_key_policy(paramiko.AutoAddPolicy()) 
#连接SSH服务端 
client.connect("172.25.254.31",username="root",password="westos") 
#获取Transport实例 
tran = client.get_transport() 
#获取SFTP实例 
sftp = paramiko.SFTPClient.from_transport(tran) 
remotepath='/home/kiosk/Desktop/fish' 
localpath='/home/kiosk/Desktop/fish' 
sftp.get(remotepath, localpath) 
client.close() 
```

### 3.**paramiko远程密码连接**

```python
import paramiko 
##1.创建一个ssh对象 
client = paramiko.SSHClient() 
#2.解决问题:如果之前没有，连接过的ip，会出现选择yes或者no的操作， 
##自动选择yes 
client.set_missing_host_key_policy(paramiko.AutoAddPolicy()) 
#3.连接服务器 
client.connect(hostname='172.25.254.31', 
 port=22, 
 username='root', 
 password='westos') 
#4.执行操作 
stdin,stdout, stderr = client.exec_command('hostname') 
#5.获取命令执行的结果 
result=stdout.read().decode('utf-8') 
print(result) 
#6.关闭连接 
client.close() 
```

### **4. 批量远程密码连接**

```python
from paramiko.ssh_exception import NoValidConnectionsError 
from paramiko.ssh_exception import AuthenticationException 
import paramiko 
def connect(cmd,hostname,port=22,username='root',passwd='westos'): 
    ##1.创建一个ssh对象 
    client = paramiko.SSHClient() 
    #2.解决问题:如果之前没有，连接过的ip，会出现选择yes或者no的操作， 
    ##自动选择yes 
    client.set_missing_host_key_policy(paramiko.AutoAddPolicy()) 
    #3.连接服务器 
    try: 
    client.connect(hostnamehostname=hostname, 
    portport=port, 
    usernameusername=username, 
    password=passwd) 
    print('正在连接主机%s......'%(hostname)) 
    except NoValidConnectionsError as e: ###用户不存在时的报错 
    print("连接失败") 
    except AuthenticationException as t: ##密码错误的报错 
    print('密码错误') 
    else: 
    #4.执行操作 
    stdin,stdout, stderr = client.exec_command(cmd) 
    #5.获取命令执行的结果 
    result=stdout.read().decode('utf-8') 
    print(result) 
    #6.关闭连接 
    finally: 
    client.close() 
with open('ip.txt') as f: #ip.txt为本地局域网内的一些用户信息 
    for line in f: 
    lineline = line.strip() ##去掉换行符 
    hostname,port,username,passwd= line.split(':') 
    print(hostname.center(50,'*')) 
    connect('uname', hostname, port,username,passwd)
```

### **6. 基于密钥的上传和下载**

```python
import paramiko 
private_key = paramiko.RSAKey.from_private_key_file('id_rsa') 
tran = paramiko.Transport('172.25.254.31',22) 
tran.connect(username='root',password='westos') 
#获取SFTP实例 
sftp = paramiko.SFTPClient.from_transport(tran) 
remotepath='/home/kiosk/Desktop/fish8' 
localpath='/home/kiosk/Desktop/fish1' 
sftp.put(localpath,remotepath) 
sftp.get(remotepath, localpath) 
```

### **5. paramiko基于公钥密钥连接**

```python
import paramiko 
from paramiko.ssh_exception import NoValidConnectionsError, AuthenticationException 
def connect(cmd, hostname, port=22, user='root'): 
    client = paramiko.SSHClient()  
    private_key = paramiko.RSAKey.from_private_key_file('id_rsa') 
    ###id_rsa为本地局域网密钥文件 
    client.set_missing_host_key_policy(paramiko.AutoAddPolicy()) 
    try: 
    client.connect(hostnamehostname=hostname, 
    portport=port, 
    userusername=user, 
    pkey=private_key 
    ) 
    stdin, stdout, stderr = client.exec_command(cmd) 
    except NoValidConnectionsError as e: 
    print("连接失败") 
    except AuthenticationException as e: 
    print("密码错误") 
    else: 
    result = stdout.read().decode('utf-8') 
    print(result) 
    finally: 
    client.close() 
for count in range(254): 
    host = '172.25.254.%s' %(count+1) 
    print(host.center(50, '*')) 
    connect('uname', host) 
```

