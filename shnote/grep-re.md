# grep(匹配查找文件内容)

## grep 格式

```shell
grep 选项 参数 文件
```

### 常用选项

```shell
-c 或 --count : 计算符合样式的列数
grep -c root grep.text
#显示结果
 2
 
 -o 只输出显示匹配内容
 grep -o "boo" /etc/passwd
-d <动作> 或 --directories=<动作> : 当指定要查找的是目录而非文件时，必须使用这项参数，否则grep指令将回报信息并停止动作 

-i 忽略大小写
grep -i "boo" /etc/passwd

-r|R 递归搜索
grep -r "192.168.1.5" /etc/

-h 禁止输出文件名
grep -hR "192.168.1.5" /etc/

-w 强制只输出那些仅仅包含那个整个单词的行
grep -w "boo" file

-c 统计显示每个文件中匹配到的次数
grep -c 'word' /path/to/file
-n 搜索结果前加行号
grep -n 'root' /etc/passwd

-v 反向选择,输出不包含搜索单词的行
grep -v bar /path/to/file

-l 仅仅显示包含内容的文件名
grep -l 'main' *.c

--color 显示彩色
grep --color vivek /etc/passwd
```

### grep 正则

```shell
# grep 只支持常规正则表达式,扩展正则需要使用egrep或者grep -E
#基本正则表达式:
	[^ $ . * [] [^] () x{m,n}]
#扩展正则表达式:
	[+ ? | ]

^ 行开头匹配
grep '^right'
$ 行结尾匹配
grep 'right$'
^$ 表示空行
grep '^$'
^...$ 表示只包含内容的行
grep '^right$'
. 表示任何一个单词匹配
grep '^.ight$'
*	前一项匹配零次或多次
grep 's*right'
.* 匹配任意数量的任何字符
grep -E '^[A-Z].*[.,]$' file.txt
[] 表示匹配中括号内的一组内容,一个一个匹配
grep [123]
[^] 表示反向匹配
grep [^123]
# 预定义字符类别
    [:alnum:]	字母数字字符 
    [:alpha:]	字母字符
    [:blank:]	空格和制表符
    [:digit:]	数字
    [:lower:]	小写字母
    [:upper:]	大写字母
    [:graph:]	匹配一个看的见的字符，不包括空白字符
    [:print:]	匹配一个可以打印的字符
    [:space:]	匹配一个空白字符，包括空格、tab、换行、分页符<
    [:punct:]	匹配一个标点符号
    [:xdigit:]	匹配一个十六进制数字，即0-9,a-f,A-F

########################    

?	前一项匹配零或一次
grep 'b\?right' file.txt
grep -E 'b?right' file.txt
+	匹配先前项一次或多次
grep -E 's+right' file.txt
{n}	精确匹配先前项n次
{n,}	至少匹配n次
{,m}	最多匹配前一项m次
{n,m}	匹配前一项从n至m 次
grep -E '[[:digit:]]{3,9}' file.txt
| 交替(或)匹配内容
grep 'fatal\|error\|critical' /var/log/nginx/error.log
grep -E 'fatal|error|critical' /var/log/nginx/error.log
()分组,可使搜索内容已一组的方式进行搜索
grep -E '(fear)?less' file.txt
grep  '\(ear\)?less' file.txt

\b	匹配单词边界
\<	匹配单词开头的空字符串
\>	在单词末尾匹配一个空字符串
\w	匹配一个单词
\s	匹配空格
grep '\b[ao]bject\b' file.txt

#匹配IP地址，共4组数字，用"."隔开
egrep "^([0-9]{1,2}|1[0-9]{2}|2[0-4][0-9]|25[0-5])\.   #第一组数字
        ([0-9]{1,2}|1[0-9]{2}|2[0-4][0-9]|25[0-5])\.   #第二组数字
        ([0-9]{1,2}|1[0-9]{2}|2[0-4][0-9]|25[0-5])\.   #第三组数字
        ([0-9]{1,2}|1[0-9]{2}|2[0-4][0-9]|25[0-5])$"   #第四组数字
        testfile 

#匹配邮箱地址
egrep "^[a-z0-9]([a-z0-9]*[-_]?[a-z0-9]+)*@
       ([a-z0-9]*[-_]?[a-z0-9]+)+[.][a-z]{2,3}([.][a-z]{2})?$"
```

### 注意

1. 当需要将元字符当作普通字符匹配的时候，需要转移字符"\"，但是当元字符位于"[]"中时，除了"-"或者"^"极少数元字符以外，其它的自动转义为普通字符。

2. 正则表达式从左到右计算，遵循一定的优先级：转义符"\" > 方括号"[]" > 分组 "()" > 限定符"*,+,?,{}" > 普通字符 > 定位符"^,$" > 或"|"。

3. 匹配同一种字符可能有多种正则表达式的写法。

4. shell本身不支持正则表达式，但是支持"*"，"?"等通配符。



