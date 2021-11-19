# awk

> AWK遵循一个简单的工作流程：读取，执行和重复

![截屏2021-09-18 下午2.18.51](https://raw.githubusercontent.com/JakeYue/picgo/master/uPic/%E6%88%AA%E5%B1%8F2021-09-18%20%E4%B8%8B%E5%8D%882.18.51.png2021%2009%2018%2014%2019%2021.png2021%2009%2018%2014%2019%2027?token=AKFPFWI73V7OJSSE4UMFTO3BIWCTA)





##读取

AWK从输入流(文件，管道，或标准输入)读取一行，并将其存储在存储器(内存)中。



## 执行

所有的AWK命令依次提交输入。默认情况下AWK执行命令每一行，但我们可以通过提供的模式限制。



## 重复

重复这个过程，直到该文件被处理完成

```shell
#awk语法
awk BEGIN {awk-commands} /pattern/ {awk-commands} END {awk-commands}
```

### BEGIN 块

```
BEGIN {awk-commands}
```

在BEGIN块被在程序启动时执行，且只执行一次。这是一个很好的初始化变量的地方。 BEGIN是AWK关键字，因此它必须是大写。请注意，这个块是可选的。

**循环开始前执行一次**

### Body块

```
/pattern/ {awk-commands}
```

主体块适用于AWK的每个输入行命令。默认情况下AWK执行每一行命令，但可以通过提供的模式限制。请注意，没有主体块的关键字。

***pattern*** 模式2种类型

```shell
# 数学及逻辑运算符: < <= == > >= != && || !
# 正则匹配: ~(正则匹配返回true) !~(正则不匹配返回true)
```



### END 块

```
END {awk-commands}
```

END块被在程序结束时执行。END是AWK关键字，因此它必须是大写。请注意，此块也是可选的

**循环结束后执行一次**

### 格式

````shell
awk 动作 文件名
awk -F':' '{print $1}' test.txt
```
root
daemon
bin
sys
sync
```

````

### 变量

`````shell	
$n 代表 某个字段(列)
$0 表示全部字段
NF 表示当前有多少行
$NF 表示最后一行
···
echo 'this is a test' | awk '{print $NF}'
···
$(NF-1) 表示倒数第二个字段
···
awk -F ':' '{print $1, $(NF-1)}' test.txt
···
$1和$(NF-1)中间的逗号表示用空格分开内容

NR 表示当前处理的是第几行
awk -F ':' '{print NR ") " $1}' demo.txt
···
1) root
2) daemon
3) bin
4) sys
5) sync
···
```
print里面要输出)需要加“”

FNR 各个文件行号分别的记录,多文件处理可以体现.
ARGC 命令行参数的个数
ARGV 数组,保存的是命令行所给定的各参数

awk的其他内置变量如下:
FILENAME：当前文件名
NR: 表示第几行(NR==1)
FS：字段分隔符，默认是空格和制表符。
RS：行分隔符，用于分割每一行，默认是换行符。
OFS：输出字段的分隔符，用于打印时分隔字段，默认为空格。
ORS：输出记录的分隔符，用于打印时分隔记录，默认为换行符。
OFMT：数字输出的格式，默认为％.6g
````
`````

### 函数

````shell
函数toupper()用于将字符转为大写
```
wk -F ':' '{ print toupper($1) }' demo.txt
ROOT
DAEMON
BIN
SYS
SYNC
```
···
其他常用函数:
tolower()：字符转为小写
length()：返回字符串长度
substr()：返回子字符串
sin()：正弦
cos()：余弦
sqrt()：平方根
rand()：随机数
···

````

### 条件

````shell
awk允许指定输出条件，只输出符合条件的行
#格式
awk '条件 动作' 文件名
```
awk -F ':' '/usr/ {print $1}' demo.txt
root
daemon
bin
sys
```
awk支持正则表达式,原生支持扩展的正则
正则使用//覆盖
````

### if语句

````shell
awk提供了if结构，用于编写复杂的条件
awk -F ':' '{if ($1 > "m") print $1}' demo.txt
```
root
sys
sync
```

if结构还可以指定else部分
awk -F ':' '{if ($1 > "m") print $1; else print "---"}' demo.txt
```
root
---
---
sys
sync
```
````

### 脚本方式

```shell
# chmod +x test.awk
#!/usr/bin/awk -f

# 使用BEGIN指定字符来设定"FS"内置变量，不能用"-F"来指定了
BEGIN { FS=":" }

# 这里可以写命令行方式引号内的语句，即正则+命名
{
    print $1
}

#使用方式
./test.awk /path/file
```

```shell
awk 格式化输出
命令 printf 通过占位符的方式输出

# *号放在前面，相当于字符串拼接，所以无所谓它是什么符号
awk '{printf "%5s %s *%d\n",$1,$2,$3}' test.txt
# 会报错，因为%d之间是不可能用+、-号以外的其它符号的
awk '{printf "%5s %s %*d\n",$1,$2,$3}' test.txt
# -号表示左对齐，不表示负号
awk '{printf "%5s %s %-d\n",$1,$2,$3}' test.txt
# 表示负数要把负号放在真正的值里
awk '{printf "%5s %s %d\n",$1,$2,-$3}' test.txt


```

### for循环

```shell
for/while/do...while/continue/break

for
awk 'BEGIN{for(i=1;i<=6;i++){print i}}'

while
awk 'BEGIN{i=1;while(i<=5){print i;i++}}'

do....while
awk 'BEGIN{i=1;do{print "test";i++}while(i<1)}'
awk 'BEGIN{i=1;do{print "test";i++}while(i<5)}'

continue 跳出本次循环,进入下一次
awk 'BEGIN{for(i=0;i<6;i++){print i}}'
awk 'BEGIN{for(i=0;i<6;i++){if(i==3)continue; print i}}'

break 跳出循环
awk 'BEGIN{for(i=0;i<6;i++){if(i==3)break; print i}}'

next 和 exit 相当于continue break
awk '{if(NR==2){next} print NR,$0}' test.txt
awk 'BEGIN{print 1; exit; print 2; print 3} END{print "This is end."}'

```

### for…in 循环和数组

```shell
# awk 数组
awk只有关联数组,(键是字符串,即使写数字也被当作字符串)
awk' BEGIN{websites["google"]="www.google.com";websites["baidu"]="www.baidu.com";websites["bing"]="www.bing.com"; print websites["google"]}'
打印一个不错在的数组不会报错,默认初始化元素被当作空字符串
数组有一个删除命令delete
awk 'BEGIN{websites["google"]="www.google.com";websites["baidu"]="www.baidu.com";websites["bing"]="www.bing.com"; print "before delete: "websites["google"];delete websites["google"];print "after delete: "websites["google"]}'
或者整个删除: 如delete websites;

# for in :
for in 循环数组
awk 'BEGIN{websites["google"]="www.google.com";websites["baidu"]="www.baidu.com";websites["bing"]="www.bing.com"; for(key in websites){print websites[key]}}'

```

### awk真假值

在awk中,0为假,1以及以上的数为真,字符串的值相当于假

```shell
awk '0 {print $0}' text.txt
awk '1 {print $0}' text.txt
awk '2 {print $0}' text.txt
awk '2.1 {print $0}' text.txt
awk 'awk {print $0}' text.txt
awk '/awk/ {print $0}' text.txt
#当有模式时，动作(Action)可以忽略不写，不写默认为{print $0}，即打印全部列
awk '1' text.txt

#打印奇偶数
#打印奇数行
awk 'i=!i {print NR,$0}' test.txt
#打印偶数行
awk '!(i=!i) {print NR,$0}' test.txt

未定义的变量是空字符串,i=0 加个!非,就是1,也就是说第一个循环 i=1,为真,所以打印,第二个循环时,i=1,加个非!,值就变为 i=0,为假所以不打印,依次反复执行,打印奇数.

偶数打印在条件 i=!i 上再加个非!,!(i=!i),这样正好与之相反.
```



