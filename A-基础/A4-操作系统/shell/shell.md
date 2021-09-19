# shell解析器

```shell
# Linux提供的shell解析器
cat /etc/shells
# bash 和 sh 的关系
ll /bin/ |grep bash
# CentOS 默认解析器
echo $SHELL
```



# shell脚本入门

## 脚本格式

脚本以`#!/bin/bash`开头（==指定解析器==）

## 脚本执行方式

### 第一种方式

- sh + 脚本相对路径
- bash + 脚本相对路径
- bash + 脚本绝对路径

### 第二种方式

- 直接输入脚本绝对路径或相对路径（需要有可执行权限）

### 两种方式对比

- 第一种方式，本质是委托给bash解析器执行，所以无需执行权限。
- 第二种方式，本质是让脚本自动运行，所以需要执行权限。

# shell变量

## 系统变量

```shell
# 常用系统变量
echo $HOME
echo $PWD
echo $SHELL
echo $USER
```

## 自定义变量

### 基本语法

1. 定义变量：变量=值（注意不能有空格）
2. 撤销变量：`unset 变量`
3. 声明静态变量：`readonly 变量`(注意：不能unset，只能重启)

### 变量定义规则

1. 变量名称可以由字母、数字和下划线组成，但是不能以数字开头，==环境变量==名建议大写。
2. ==等号两侧不能有空格==
3. 在bash中，变量默认类型==都是==字符串类型，无法直接进行数值运算。
4. 变量的值如果有空格，需要使用双引号或单引号括起来。

## 特殊变量

```wiki
$n
功能描述：n为数字，$0代表该脚本名称，$1-$9代表第一到第九个参数，十以上的参数，十以上的参数需要用【大括号】包含，如${10}。

$#
功能描述：获取所有输入参数【个数】，常用于循环。

$*、$@
$* 功能描述：这个变量代表命令行中所有的参数，$*把所有的参数看成一个整体。
$@ 功能描述：这个变量也代表命令行中所有的参数，不过$@把每个参数区分对待。

$？
功能描述：最后一次执行的命令的返回状态。如果这个变量的值为0，证明上一个命令正确执行；如果这个变量的值为非0（具体是哪个数，由命令自己来决定），则证明上一个命令执行不正确了。
```

# 运算符

 ## 基本语法

```wiki
1. “$((运算式))” 或 “$[运算式]”
2. expr  + , - , \*,  /,  %   加，减，乘，除，取余

注意：
1. 乘号需要转义
2. expr 运算符间要有空格
```

```shell
# 计算 3+2
expr 2 + 3
# 计算 (2+3)*4
expr `expr 2 + 3` \* 4
# 或者
s=$[(2+3)*4]
echo $s
```

# 条件判断（重点）

## 基本语法

```wiki
[ condition ]（注意condition前后要有空格）
注意：条件非空即为true，[ zhangsan ]返回true，[] 返回false。
```



## 常用判断条件

### 两个整数
```wiki
= 字符串比较
-lt 小于（less than）
-le 小于等于（less equal）
-eq 等于（equal）				
-gt 大于（greater than）
-ge 大于等于（greater equal）	
-ne 不等于（Not equal）
```

### 文件权限
```wiki
-r 有读的权限（read）
-w 有写的权限（write）
-x 有执行的权限（execute）
```

### 文件类型
```wiki
-f 文件存在并且是一个常规的文件（file）
-e 文件存在（existence）
-d 文件存在并是一个目录（directory）
```

# 流程控制（重点）

## 基本语法

### if

```wiki
if [ 条件判断式 ];then 
    程序 
fi 

或者

if [ 条件判断式 ] 
then 
    程序 
fi

注意事项：
1. [ condition ]，中括号和条件判断式之间必须有空格
2. if 后要有空格
```

### case

```wiki
case $变量名 in 
  "值1"） 
    如果变量的值等于值1，则执行程序1 
    ;; 
  "值2"） 
    如果变量的值等于值2，则执行程序2 
    ;; 
  …省略其他分支… 
  *） 
    如果变量的值都不是以上的值，则执行此程序 
    ;; 
esac（把case倒过来写）

注意事项：
1. case行尾必须为单词“in”，每一个模式匹配必须以右括号“）”结束。
2. 双分号“;;”表示命令序列结束，相当于java中的break。
3. 最后的“*）”表示默认模式，相当于java中的default。
```

### for

```wiki
for (( 初始值;循环控制条件;变量变化 )) 
do 
    程序
done

或

for 变量 in 值1 值2 值3… 
do 
    程序 
done
```

```shell
#!/bin/bash
s=0
for((i=0;i<=100;i++))
do
    s=$[$s+$i]
done
echo $s
```

```shell
#!/bin/bash

# 打印数字
for i in $*
do
    echo "ban zhang love $i"
done

for j in $@
do      
    echo "ban zhang love $j"
done
# 注意 $取值之后加双引号
for i in "$*"
do
    echo "ban zhang love $i"
done
for j in "$@"
do      
    echo "ban zhang love $j"
done
```

### while

```wiki
while [ 条件判断式 ] 
do 
   程序
done
```

```shell
#!/bin/bash
s=0
i=1
while [ $i -le 100 ]
do
   s=$[$s+$i]
   i=$[$i+1]
done
echo $s
```

# read 读取控制台输入

## 基础语法

```wiki
read(选项)(参数)

选项：
-p：指定读取值时的提示符；
-t：指定读取值时等待的时间（秒）。

参数
变量：指定读取值的变量名
```

```shell
#!/bin/bash
read -t 7 -p "Enter your name in 7 seconds " NAME
echo $NAME
```

# 函数

## 系统函数

### basename

```wiki
basename [string / pathname] [suffix]  	（功能描述：basename命令会删掉所有的前缀包括最后一个（‘/’）字符，然后将字符串显示出来。

选项：
suffix为后缀，如果suffix被指定了，basename会将pathname或string中的suffix去掉。
```

```shell
$ basename /home/idc/zhangsan.txt 
zhangsan.txt

$ basename /home/idc/zhangsan.txt .txt
zhangsan
```

### dirname

```wiki
dirname 文件绝对路径（功能描述：从给定的包含绝对路径的文件名中去除文件名（非目录的部分），然后返回剩下的路径（目录的部分））
```

```shell
$ dirname /home/idc/zhangsan.txt 
/home/idc
```

## 自定义函数

```wiki
基本语法
[ function ] funname[()]
{
	Action;
	[return int;]
}
funname

经验技巧
1. 必须在调用函数地方之前，先声明函数，shell脚本是逐行运行。不会像其它语言一样先编译。
2. 函数返回值，只能通过$?系统变量获得，可以显示加：return返回，如果不加，将以最后一条命令运行结果，作为返回值。return后跟数值n(0-255)
```

```shell
#!/bin/bash
function sum()
{
    s=0
    s=$[ $1 + $2 ]
    echo "$s"
}

read -p "Please input the number1: " n1
read -p "Please input the number2: " n2
sum $n1 $n2
```

# shell工具（重点）

## cut

```wiki
cut的工作就是“剪”，具体的说就是在文件中负责剪切数据用的。cut 命令从文件的每一行剪切字节、字符和字段并将这些字节、字符和字段输出。

基本用法
cut [选项参数]  filename
说明：默认分隔符是制表符

-f：列号，提取第几列
-d：分隔符，按照指定分隔符分割列
```

```shell
$ cut -d " " -f 1 cut.txt
$ cut -d " " -f 2,3 cut.txt
$ ifconfig ens33 | grep "inet " | cut -d " " -f 10
```

## sed

```wiki
sed是一种流编辑器，它一次处理一行内容。处理时，把当前处理的行存储在临时缓冲区中，称为“模式空间”，接着用sed命令处理缓冲区中的内容，处理完成后，把缓冲区的内容送往屏幕。接着处理下一行，这样不断重复，直到文件末尾。【文件内容并没有改变】，除非你使用重定向存储输出。

基本用法
sed [选项参数]  ‘command’  filename

-e：直接在指令列模式上进行sed的动作编辑。

a：新增，a的后面可以接字串，在下一行出现
d：删除
s：查找并替换 
```

```shell
$ echo "aaaa" >> sed.txt
$ echo "bbbb" >> sed.txt
$ echo "cccc" >> sed.txt

# 在第二行下插入
$ sed "2a 1111" sed.txt
# 删除
$ sed "/b/d" sed.txt
# 替换，把b替换为d
$ sed "s/b/d/g" sed.txt （加g，global，全部替换）
# -e 的用法
$ sed -e "2d" -e "s/c/d/g" sed.txt
```

## awk

```wiki
一个强大的文本分析工具，把文件逐行的读入，以空格为默认分隔符将每行切片，切开的部分再进行分析处理。

基本用法
awk [选项参数] ‘pattern1{action1}  pattern2{action2}...’ filename
pattern：表示AWK在数据中查找的内容，就是匹配模式
action：在找到匹配内容时所执行的一系列命令

-F	指定输入文件折分隔符
-v	赋值一个用户定义变量

内置变量
FILENAME	文件名
NR	已读的记录数
NF	浏览记录的域的个数（切割后，列的个数）
```

```shell
# 以root关键字开头的所有行，并输出该行的第7列
$ awk -F: '/^root/{print $7}' passwd
# 以root关键字开头的所有行，并输出该行的第1列和第7列，中间以“，”号分割
$ awk -F: '/^root/{print $1","$7}' passwd
# 只显示/etc/passwd的第一列和第七列，以逗号分割，且在所有行前面添加列名“user，shell”，在最后一行添加"lisi，/bin/zuishuai"
$ awk -F : 'BEGIN{print "user, shell"} {print $1","$7} END{print "lisi,/bin/zuishuai"}' passwd
# 统计 passwd 文件名，每行的行号，每行的列数
$ awk -F: '{print "filename:"  FILENAME ", linenumber:" NR  ",columns:" NF}' passwd
# 切割IP
$ ifconfig ens33 | grep "inet "|awk -F " " '{print $2}'
# 查询sed.txt中空行所在的行号（面试题）
$ awk '/^$/{print NR}' sed.txt
```

## sort

```wiki
sort命令是在Linux里非常有用，它将文件进行排序，并将排序结果标准输出。

基本语法
sort(选项)(参数)

-n	依照数值的大小排序
-r	以相反的顺序来排序
-t	设置排序时所用的分隔字符
-k	指定需要排序的列

参数：指定待排序的文件列表
```

```shell
# sort.txt 文件数据
bb:40:5.4
bd:20:4.2
xz:50:2.3
cls:10:3.5
ss:30:1.6

$ sort -t : -nrk 2 sort.txt
```

