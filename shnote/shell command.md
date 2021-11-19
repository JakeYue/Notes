### 1、EOF

> EOF 把EOF内的内容作为输入给其他子命令或者子shell,知道遇到EOF结束

```shell
cat << EOF

/ --------------

这是个输入。     #

/ --------------

EOF

```

### 2、case

> case 多分支条件判断语句,只能判断一种条件

```shell
#格式
case $变量名 in 
var 1)
    执行
    ;;
var 1)
    执行
    ;;
* )
    执行***
    ;;
esac

```

### read

> 读取输入
>
> read命令 -p(提示语句) -n(字符个数) -t(等待时间) -s(不回显) -e(交换输入)

```shell
read -p "input password:" -s password

```

### ls -1

```shell
ls -1 显示长格式
➜ ✔ ls   
Dockerfile         docker-compose.yml
README.md          start.sh
cobbler.env        supervisord.d

➜  ✔ ls -1   
Dockerfile
README.md
cobbler.env
docker-compose.yml
start.sh
supervisord.d

```

### eval

```shell
#eval 命令:用于重新运算得出参数的内容
#eval可读取一连串的参数，然后再依参数本身的特性来执行
#eval 可
eval [参数]

```
