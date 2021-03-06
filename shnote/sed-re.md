# sed

(Sed 主要用来自动编辑一个或多个文件、简化对文件的反复操作、编写转换程序)

## sed 格式

```shell
sed [-hnV][-e<script>][-f<script文件>][文本文件]
#选项
-n 只打印sed处理后的内容行
-e 以选项中指定的script来处理输入的文本文件
-f 以选项中指定的script文件来处理输入的文本文件
-n 仅显示script处理后的结果
-r 让sed支持扩展的正则
-i 直接修改读取的文件内容，而不是由屏幕输出

#动作说明：
a\ ：追加行， a 的后面可以接字串，而这些字串会在新的一行出现(目前的下一行)～
c\ ：替换行， c 的后面可以接字串，这些字串可以取代 n1,n2 之间的行！(多行字符串可以用\n分隔)
d ：删除行，因为是删除啊，所以 d 后面通常不接任何咚咚；
i\ ：插入行， i 的后面可以接字串，而这些字串会在新的一行出现(目前的上一行)；
p ：打印，亦即将某个选择的数据印出。通常 p 会与参数 sed -n 一起运行～
y：替换字符，通常y命令的用法是这样的：y/Source-chars/Dest-chars/，分割字符/可以用任意单字符代替，用Dest-chars中对应位置的字符替换掉Soutce-chars中对应位置的字符
s ：替换字符串，可以直接进行取代的工作！通常这个 s 的动作可以搭配正规表示法！例如 1,20s/old/new/g 就是啦！


##替换选项
\digit：Replacement中可含有后向引用中的\digit(digit是1至9)，引用前面定义的子表达

&：代表模版空间中的整个匹配部分

\L：将在其后的替换部分转换成小写字母，直到发现一个\U或\E，GNU扩展功能

\l：将下一个字符转换成小写字母，GNU扩展功能

\U：将在其后的替换部分转换成大写字母，直到发现一个\L或\E，GNU扩展功能

\u：将下一个字符转换成大写字母，GNU扩展功能

\E：停止由\L或\U指示开始的大小写转换，GNU扩展功能

##标志选项
g：将用Replacement替换模版空间中所有匹配Regexp的部分，则不仅仅是第一个匹配部分

digit：只用Replacement替换模版空间中第digit(digit是1至9)个匹配Regexp的部分

p：若发生了替换操作，指示显示模版空间中新的数据

w file-name：若发生了替换操作，指示将模版空间中新的数据写入指定的文件file-name中

i：表示进行Regexp匹配时，是不区分大小写字母的
```

### sed正则

####基本正则

```shell
#sed 不支持扩展正则,需要使用sed -r选项
*	将*前面的正则表达式匹配的结果重复任意次(含0次)
\+	与星号(*)相同，只是至少重复1次，GNU的扩展功能
\?	与星号(*)相同，只是最多重复1次，GNU的扩展功能
\{i\}	与星号(*)相同，只是重复指定的i次
\{i,j\}	与星号(*)相同，只是重复i至j次
\{i, \}	与星号(*)相同，只是至少重复i次
\(regexp\)	将regexp看作一个整体，用于后向引用，与\digit配合使用
.	匹配任意单个字符
^	匹配模版空间开始处的NULL字符串
$	匹配的是模版空间结束处的NULL字符串
[list]	匹配方括号中的字符列表中的任意一个
[^list]	否定匹配方括号中的字符列表中的任意一个
regexp1\|regexp2	用在相邻的正则表达式之间，表示匹配这些正则表达式中任一个都可以。匹配是从左向右开始的，一旦匹配成功就停止匹配
regexp1regexp2	匹配regexp1和regexp2的连接结果
\digit	匹配正则表达式前半部分定义的后向引用的第digit个子表达式。digit为1至9的数字, 1为从左开始。
\n	匹配换行符
\meta	将元字符meta转换成普通字符，以便匹配该字符本身，有$、 *、 .、 [、 \ 和 ^
```

#### 扩展正则

```shell
扩展正则表达式
	?  0次 or 1次 
    +  至少一次
	|  或
	{ }  限制字符
	( )  分组字符
```

#### 转移字符

```shell
\a	匹配一个BEL字符
\f	匹配一个换页字符
\n	匹配一个换行字符
\r	匹配一个回车字符
\t	匹配一个水平Tab字符
\v	匹配一个垂直Tab字符
\cX	匹配Control+X，X是任意字符。
\dXXX	匹配一个ASCII码是十进制XXX的字符
\oXXX	匹配一个ASCII码是八进制XXX的字符
\xXX	匹配一个ASCII码是十六进制XX的字符
\w	匹配任意一个单词字符(字母、数字和下划线)
\W	匹配任意一个非单词字符
\b	匹配一个单词的边界符：字符的左边是一个单词字符，并且右边是一个非单词字符，反之亦然
\B	匹配除单词边界符外所有字符：字符的左边和右边同时是单词字符或非单词字符
```

