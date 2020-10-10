[**首页**](https://github.com/qdw497874677/myNotes/blob/master/首页检索.md)

# 初级命令

## ls

ls全称list，默认列出当前目录。

格式

~~~
ls [OPTION]... [FILE]...
~~~

参数

> -a  列出指定目录下的所有文件，包括隐藏文件
>
> -c 使用最后一次更改文件状态以进行排序(-t)或长时间打印(-l)的时间
>
> -h 与-l选项一起使用时，请使用单位后缀:Byte、Kilobyte、mete、gb、tb和Petabyte，以便使用以2为基数的大小将数字减少到3或更少
>
> -l 长格式列表。(见下文)。如果输出到终端，则所有文件大小的总和将输出到长清单前面的一行中
>
> -n 以数字形式显示用户和组id，而不是在长(-l)输出中转换为用户或组名。这个选项默认打开-l选项
>
> -o 以长格式列出，但省略组id
>
> -s 显示每个文件实际使用的文件系统块的数量，以512字节为单位，其中部分单元四舍五入为下一个整数值
>
> -t 在按照字典顺序对操作数排序之前，先按修改的时间排序(最近修改的是first)
>
> -u 使用最后一次访问的时间，而不是最后一次修改文件进行排序



## pwd

打印当前工作目录的完整路径名。(print name of current/working directory)



## touch

将每个文件的访问和修改时间更新为当前时间。除非提供-c或-h，否则将不存在的FILE参数创建为空。(change file timestamps)

格式

> touch [OPTION]... FILE...

参数

> -a  或--time=atime或--time=access或--time=use 只更改存取时间。
>
> -c  或--no-create 不建立任何文档。
>
> -d 使用指定的日期时间，而非现在的时间。
>
> -f 此参数将忽略不予处理，仅负责解决BSD版本touch指令的兼容性问题。
>
> -m  或--time=mtime或--time=modify 只更改变动时间。
>
> -r 把指定文档或目录的日期时间，统统设成和参考文档或目录的日期时间相同。
>
> -t 使用指定的日期时间，而非现在的时间。

示例

~~~bash
#创建三个文件
$ touch test1 test2 test3
#不创建文档
$ touch -c test5
$ ls
test1  test2  test3
~~~



## cat&tac 

将FILE或标准输入连接到标准输出。 (Concatenate FILE(s), or standard input, to standard output.)

格式

> cat [OPTION]... [FILE]...

参数

> -A, --show-all      等价于 -vET
>
> -b, --number-nonblank  对非空输出行编号
>
> -e            等价于 -vE
>
> -E, --show-ends     在每行结束处显示
>
> -n, --number   对输出的所有行编号,由1开始对所有输出的行数编号
>
> -s, --squeeze-blank 有连续两行以上的空白行，就代换为一行的空白行
>
> -t            与 -vT 等价
>
> -T, --show-tabs     将跳格字符显示为 ^I
>
> -u            (被忽略)
>
> -v, --show-nonprinting  使用 ^ 和 M- 引用，除了 LFD 和 TAB 之外

示例

~~~bash
#展示文件内容
$ cat test  
-A, --show-all      等价于 -vET
-b, --number-nonblank  对非空输出行编号
-e            等价于 -vE
#展示文件内容并且展示行号
$ cat -n test  
     1    -A, --show-all      等价于 -vET
     2    -b, --number-nonblank  对非空输出行编号
     3    -e            等价于 -vE
~~~

用tac展示的内容相反

~~~bash
$ tac test
-e            等价于 -vE
-b, --number-nonblank  对非空输出行编号
-A, --show-all      等价于 -vET
~~~



## mkdir

如果目录不存在，则创建目录。（Make Directory）

格式

> mkdir [OPTION]... DIRECTORY...

参数

> -m, --mode=模式，设定权限<模式> (类似 chmod)，而不是 rwxrwxrwx 减 umask
>
> -p, --parents 可以是一个路径名称。此时若路径中的某些目录尚不存在,加上此选项后,系统将自动建立好那些尚不存在的目录,即一次可以建立多个目录;
>
> -v, --verbose 每次创建新目录都显示信息
>
> --help  显示此帮助信息并退出
>
> --version 输出版本信息并退出

示例

~~~bash
#创建目录文件test
$ mkdir test
#连续创建
$ mkdir -p test1/tmp
$ ls
test  test1
#创建时置顶目录权限
#tmp目录拥有可执行权限
$ mkdir -pm 777 test2/tmp
$ ls -lh
total 12K
drwxr-xr-x 2 localhost hero 4.0K Dec 21 21:39 test
drwxr-xr-x 3 localhost hero 4.0K Dec 21 21:40 test1
drwxr-xr-x 3 localhost hero 4.0K Dec 21 21:40 test2
$ ls
test  test1  test2
#-v 参数可确定文件是否已经存在，如果不存在则会创建，并显示如下信息
$ mkdir -v test
mkdir: cannot create directory ‘test’: File exists

$ mkdir -v test7
mkdir: created directory ‘test7’
~~~



## cd

切换当前目录到指定目录。（Change Directory）



## rm&rmdir

尝试删除命令行上指定的非目录类型文件。如果文件的权限不允许写入，并且标准输入设备是终端，则会提示用户（在标准错误输出上）进行确认。（Remove Directory）

格式

> rm [-dfiPRrvW] file ...

参数

> -f, --force  忽略不存在的文件，从不给出提示。
>
> -i, --interactive 进行交互式删除
>
> -r, -R, --recursive  指示rm将参数中列出的全部目录和子目录均递归地删除。
>
> -d, --dir 删除空目录

示例

~~~bash
# 创建三个文件
$ touch tmp.cc tmp.java tmp.py tmp.go
#创建目录文件
$ mkdir -p linux/test
#查看文件是否创建成功
$ ls
linux    tmp.cc   tmp.go   tmp.java tmp.py
#删除文件，并进行提示
$ rm -i tmp.cc
remove tmp.cc? y
#强制删除
$ rm  -f tmp.go
#删除目录
$ rm -f linux  #删除目录失败
rm: linux: is a directory
#循环删除目录下所有文件
$ rm -rf linux  #删除目录成功，
$ ls
tmp.java tmp.py  
~~~

rmdir==rm -d  删除空目录



## mv

移动目录或者文件到置顶目录下，同时具有重命名的功能。（Move）

参数

> mv [-f | -i | -n] [-v] source target mv [-f | -i | -n] [-v] source ... directory

格式

> -b ：若需覆盖文件，则覆盖前先行备份。
>
> -f ：force 强制的意思，如果目标文件已经存在，不会询问而直接覆盖；
>
> -i ：若目标文件 (destination) 已经存在时，就会询问是否覆盖
>
> -n：不要覆盖现有文件。（-n选项将覆盖以前的任何-f或-i选项。）
>
> -u ：若目标文件已经存在，且 source 比较新，才会更新(update)

示例

~~~bash
##修改文件名
$ touch tmp.cc

$ ls
tmp.cc

#重命名
$ mv tmp.cc tmp.java

$ ls
tmp.java
#移动文件或者目录
$ pwd
/Users/localhost/test
#移动文件并重命名
$ mv /Users/localhost/logs/tmp.txt ./tmp.log 

$ ls /Users/localhost/logs/
discover-client metabase        tesla

$ ls ./
tmp.java tmp.log
#移动目录并重命名
$ mv /Users/localhost/logs/tesla  ./tesla.ba 

$ ls
tesla.ba tmp.java tmp.log
~~~



## cp

将source_file的内容复制到target_file。在第二个大纲格式中，每个命名的source_file的内容都复制到目标target_directory。文件本身的名称不会更改。如果cp检测到尝试将文件复制到自身的尝试，则复制将失败。（Copy）

格式

> cp [-R [-H | -L | -P]] [-fi | -n] [-apvX] source_file target_file cp [-R [-H | -L | -P]] [-fi | -n] [-apvX] source_file ... target_directory

参数

> -a, --archive  等于-dR --preserve=all
>
> --backup[=CONTROL  为每个已存在的目标文件创建备份
>
> -b        类似--backup 但不接受参数
>
> --copy-contents    在递归处理是复制特殊文件内容
>
> -d        等于--no-dereference --preserve=links
>
> -f, --force    如果目标文件无法打开则将其移除并重试(当 -n 选项
>
> 存在时则不需再选此项)
>
> -i, --interactive    覆盖前询问(使前面的 -n 选项失效)
>
> -H        跟随源文件中的命令行符号链接
>
> -l, --link      链接文件而不复制
>
> -L, --dereference  总是跟随符号链接
>
> -n, --no-clobber  不要覆盖已存在的文件(使前面的 -i 选项失效)
>
> -P, --no-dereference  不跟随源文件中的符号链接
>
> -p        等于--preserve=模式,所有权,时间戳
>
> --preserve[=属性列表  保持指定的属性(默认：模式,所有权,时间戳)，如果
>
> 可能保持附加属性：环境、链接、xattr 等
>
> -R, -r, --recursive 复制目录及目录内的所有项目

示例

~~~bash
$ cat tmp.cc
change world

#拷贝文件内容
$ cp tmp.cc tmp.java

$ cat tmp.java
change world
~~~



## echo

将任何指定的操作数写入标准输出，这些操作数由单个空格（）字符分隔，后跟换行符（\ n'）字符。

示例

~~~bash
$ echo "change world"
change world

#s输出PWD环境变量的值
$ echo $PWD
/Users/localhost/test
~~~



## PS

显示标题行，其后是包含有关具有控制终端的所有进程的信息的行。

格式

> ps [-AaCcEefhjlMmrSTvwXx] [-O fmt | -o fmt] [-G gid[,gid...]] [-g grp[,grp...]] [-u uid[,uid...]] [-p pid[,pid...]] [-t tty[,tty...]] [-U user[,user...]] ps [-L]

参数

> a 显示所有进程
>
> -a 显示同一终端下的所有程序
>
> -A 显示所有进程
>
> c 显示进程的真实名称
>
> -N 反向选择
>
> -e 等于“-A”
>
> e 显示环境变量
>
> f 显示程序间的关系
>
> -H 显示树状结构
>
> r 显示当前终端的进程
>
> T 显示当前终端的所有程序
>
> u 指定用户的所有进程
>
> -au 显示较详细的资讯
>
> -aux 显示所有包含其他使用者的行程
>
> -C<命令> 列出指定命令的状况
>
> --lines<行数> 每页显示的行数
>
> --width<字符数> 每页显示的字符数

示例

~~~bash
#查看所有进程
$ps -a
#查看进程的环境变量和程序间的关系
$ps -ef
~~~



## kill&killall

命令kill将指定的信号发送到指定的进程或进程组。如果未指定信号，则发送TERM信号。TERM信号将杀死不捕获该信号的进程。对于其他过程，可能需要使用KILL（9）信号，因为无法捕获该信号。

格式

> kill [-s signal|-p] [-q sigval] [-a] [--] pid...
> kill -l [signal]

参数

> -l 信号，若果不加信号的编号参数，则使用“-l”参数会列出全部的信号名称
>
> -a 当处理当前进程时，不限制命令名和进程号的对应关系
>
> -p 指定kill 命令只打印相关进程的进程号，而不发送任何信号
>
> -s 指定发送信号
>
> -u 指定用户

示例

~~~bash
#查看当前系统信号
$ kill -l
 1) SIGHUP     2) SIGINT   3) SIGQUIT  4) SIGILL   5) SIGTRAP
 6) SIGABRT     7) SIGBUS   8) SIGFPE   9) SIGKILL 10) SIGUSR1
11) SIGSEGV    12) SIGUSR2 13) SIGPIPE 14) SIGALRM 15) SIGTERM
16) SIGSTKFLT    17) SIGCHLD 18) SIGCONT 19) SIGSTOP 20) SIGTSTP
21) SIGTTIN    22) SIGTTOU 23) SIGURG  24) SIGXCPU 25) SIGXFSZ
26) SIGVTALRM    27) SIGPROF 28) SIGWINCH    29) SIGIO   30) SIGPWR
31) SIGSYS    34) SIGRTMIN    35) SIGRTMIN+1  36) SIGRTMIN+2  37) SIGRTMIN+3
38) SIGRTMIN+4    39) SIGRTMIN+5  40) SIGRTMIN+6  41) SIGRTMIN+7  42) SIGRTMIN+8
43) SIGRTMIN+9    44) SIGRTMIN+10 45) SIGRTMIN+11 46) SIGRTMIN+12 47) SIGRTMIN+13
48) SIGRTMIN+14    49) SIGRTMIN+15 50) SIGRTMAX-14 51) SIGRTMAX-13 52) SIGRTMAX-12
53) SIGRTMAX-11    54) SIGRTMAX-10 55) SIGRTMAX-9  56) SIGRTMAX-8  57) SIGRTMAX-7
58) SIGRTMAX-6    59) SIGRTMAX-5  60) SIGRTMAX-4  61) SIGRTMAX-3  62) SIGRTMAX-2
63) SIGRTMAX-1    64) SIGRTMAX
~~~

HUP    1    终端断线
INT     2    中断（同 Ctrl + C）
QUIT    3    退出（同 Ctrl + \）
TERM   15    终止
KILL    9    强制终止
CONT   18    继续（与STOP相反， fg/bg命令）
STOP    19    暂停（同 Ctrl + Z）

kill -9 是我们使用的最多的信号，其实这种方式一点也不优雅，应该使用kill -15信号，大部分程序接收到SIGTERM信号后，会先释放自己的资源，然后再停止。但是也有程序可能接收信号后，做一些其他的事情（如果程序正在等待IO，可能就不会立马做出响应，等到io完成后在结束），也就是说，SIGTERM多半是会被阻塞的。



## chmod

权限有读（r、4）、写（w、2）、执行（x、1）

所以只读4、读运行5、读写运行7

赋予权限的对象分别为 文件所有者、群组用户、其他用户

# 进阶命令

## find

find实用程序对列出的每个路径递归地遍历目录树，根据树中的每个文件计算表达式(由下面列出的“初选”和“操作数”组成)。

格式

> find [-H | -L | -P] [-EXdsx] [-f path] path ... [expression] find [-H | -L | -P] [-EXdsx] -f path [path ...] [expression]

参数

> -print：find命令将匹配的文件输出到标准输出。
>
> -exec：find命令对匹配的文件执行该参数所给出的shell命令。相应命令的形式为'command' { } \;，注意{  }和\；之间的空格。
>
> -name  按照文件名查找文件。
>
> -perm  按照文件权限来查找文件。
>
> -prune 使用这一选项可以使find命令不在当前指定的目录中查找，如果同时使用-depth选项，那么-prune将被find命令忽略。
>
> -user  按照文件属主来查找文件。
>
> -group 按照文件所属的组来查找文件。
>
> -mtime -n +n 按照文件的更改时间来查找文件， - n表示文件更改时间距现在n天以内，+ n表示文件更改时间距现在n天以前。find命令还有-atime和-ctime 选项，但它们都和-m time选项。
>
> -nogroup 查找无有效所属组的文件，即该文件所属的组在/etc/groups中不存在。
>
> -nouser  查找无有效属主的文件，即该文件的属主在/etc/passwd中不存在。
>
> -newer file1 ! file2 查找更改时间比文件file1新但比文件file2旧的文件。
>
> -type 查找某一类型的文件，诸如：
>
> b - 块设备文件。
>
> d - 目录。
>
> c - 字符设备文件。
>
> p - 管道文件。
>
> l - 符号链接文件。
>
> f - 普通文件。
>
> -size n：[c] 查找文件长度为n块的文件，带有c时表示文件长度以字节计。-depth：在查找文件时，首先查找当前目录中的文件，然后再在其子目录中查找。
>
> -fstype：查找位于某一类型文件系统中的文件，这些文件系统类型通常可以在配置文件/etc/fstab中找到，该配置文件中包含了本系统中有关文件系统的信息。
>
> -mount：在查找文件时不跨越文件系统mount点。
>
> -follow：如果find命令遇到符号链接文件，就跟踪至链接所指向的文件。
>
> -cpio：对匹配的文件使用cpio命令，将这些文件备份到磁带设备中。
>
> 另外,下面三个的区别:
>
> -amin n  查找系统中最后N分钟访问的文件
>
> -atime n 查找系统中最后n*24小时访问的文件
>
> -cmin n  查找系统中最后N分钟被改变文件状态的文件
>
> -ctime n 查找系统中最后n*24小时被改变文件状态的文件
>
> -mmin n  查找系统中最后N分钟被改变文件数据的文件
>
> -mtime n 查找系统中最后n*24小时被改变文件数据的文件

示例

~~~bash
# 查找/user目录下所有以.log结尾的文件
find /Users -name "*.log" -print
~~~

举一个我在工作中经常用到的例子，我有个日志目录，我系统的所有日志都会打到这个目录，目录的日志文件命名很随意，我没办法说根据名字删除，于是我想到用日期的方式删除，保存一个月的日志即可。

~~~bash
$find /home/midou/logs// -mtime +30 -name "*.log.gz" -exec rm -rf {} \;
# {} 这个是语法不能丢了 ，还有结尾的 ； 也不能丢了。
~~~

## grep

grep实用程序搜索任何给定的输入文件，选择与一个或多个模式匹配的行。默认情况下，如果模式中的正则表达式（RE）匹配输入行而没有尾随换行符，则该模式会匹配输入行。空表达式匹配每行。与至少一种模式匹配的每条输入线均写入标准输出

格式

> grep [-abcdDEFGHhIiJLlmnOopqRSsUVvwxZ] [-A num] [-B num] [-C[num]] [-e pattern] [-f file] [--binary-files=value] [--color[=when]] [--colour[=when]][--context[=num]] [--label] [--line-buffered] [--null] [pattern] [file ...]

参数

> -a  --text  不要忽略二进制的数据。
>
> -A<显示行数>  --after-context=<显示行数>  #除了显示符合范本样式的那一列之外，并显示该行之后的内容。
>
> -b  --byte-offset  #在显示符合样式的那一行之前，标示出该行第一个字符的编号。
>
> -B<显示行数>  --before-context=<显示行数>  #除了显示符合样式的那一行之外，并显示该行之前的内容。
>
> -c  --count  #计算符合样式的列数。
>
> -C<显示行数>  --context=<显示行数>或-<显示行数>  #除了显示符合样式的那一行之外，并显示该行之前后的内容。
>
> -d <动作>   --directories=<动作>  #当指定要查找的是目录而非文件时，必须使用这项参数，否则grep指令将回报信息并停止动作。
>
> -e<范本样式> --regexp=<范本样式>  #指定字符串做为查找文件内容的样式。
>
> -E   --extended-regexp  #将样式为延伸的普通表示法来使用。
>
> -f<规则文件> --file=<规则文件>  #指定规则文件，其内容含有一个或多个规则样式，让grep查找符合规则条件的文件内容，格式为每行一个规则样式。
>
> -F  --fixed-regexp  #将样式视为固定字符串的列表。
>
> -G  --basic-regexp  #将样式视为普通的表示法来使用。
>
> -h  --no-filename  #在显示符合样式的那一行之前，不标示该行所属的文件名称。
>
> -H  --with-filename  #在显示符合样式的那一行之前，表示该行所属的文件名称。
>
> -i  --ignore-case  #忽略字符大小写的差别。
>
> -l  --file-with-matches  #列出文件内容符合指定的样式的文件名称。
>
> -L  --files-without-match  #列出文件内容不符合指定的样式的文件名称。
>
> -n  --line-number  #在显示符合样式的那一行之前，标示出该行的列数编号。
>
> -q  --quiet或--silent  #不显示任何信息。
>
> -r  --recursive  #此参数的效果和指定“-d recurse”参数相同。
>
> -s  --no-messages  #不显示错误信息。
>
> -v  --revert-match  #显示不包含匹配文本的所有行。
>
> -V  --version  #显示版本信息。
>
> -w  --word-regexp  #只显示全字符合的列。
>
> -x  --line-regexp  #只显示全列符合的列。
>
> -y  此参数的效果和指定“-i”参数相同。

示例

~~~bash
$grep '20:[1-5][0-9]:' *.log  #匹配当前目录下搜索log日志中，20点的日志
$grep '20:[1-5][0-9]' 1.log 2.log 3.log  #指定在这三个文件中查找
#grep规则是支持正则表达式的
$ps -ef|grep java    #查找所有java进程
$ps -ef|grep java    #-c可以统计查找的个数
$grep '20:[1-5][0-9]:' *.log | grep -v '20:[3-4][0-9]:'   # -v反向选择，相当于过滤
$grep 'ab|bc' *.log  #支持|语法，匹配含有ab或者bc的文本行
~~~

统计error行数

~~~bash
cat web.access.log | grep error |wc -l
~~~

将error重定向到文件中

~~~bash
grep error web.access.log >mylog1.txt
~~~



## cut

cut实用程序从每个文件中剪切出每行的选定部分（由列表指定），并将它们写入标准输出。如果未指定文件参数，或者文件参数为单破折号（-），则从标准输入中读取内容。列表指定的项目可以是列位置，也可以是由特殊字符分隔的字段。列编号从1开始。

格式

> cut -b list [-n] [file ...] cut -c list [file ...] cut -f list [-d delim] [-s] [file ...]

参数

> -b：仅显示行中指定直接范围的内容；
>
> -c：仅显示行中指定范围的字符；
>
> -d：指定字段的分隔符，默认的字段分隔符为“TAB”；
>
> -f：显示指定字段的内容；
>
> -n：与“-b”选项连用，不分割多字节字符；
>
> --complement：补足被选择的字节、字符或字段；
>
> --out-delimiter=<字段分隔符>：指定输出内容是的字段分割符；

示例

~~~bash
$cut -c-10 tmp.txt  #cut tmp.txt文件的前10列
$cut -c3-5 tmp.txt  #cut tmp.txt文件的第3到5列
$cut -c3- tmp.txt  #cut tmp.txt文件的第3到结尾列
~~~

面试案例

要求：tmp.cc文件中有一行字符，把这一行搞成10行，并且取出每一列的第七个字符。

~~~bash
# 管道符“|”将两个命令隔开，管道符左边命令的抄输出就会作为管道符右边命令的输入。
# >>是追加内容 >是覆盖内容
$ cat tmp.cc| >>tmp.cc|>>tmp.cc|>>tmp.cc|head -n10|>tmp.cc|cut -c7-7
w
w
w
w
w
w
w
w
w
w
~~~



## tar&gzip

用来压缩和解压文件。tar本身不具有压缩功能。他是调用压缩功能实现的。

格式

> tar [bundled-flags <args>] [<file> | <pattern> ...]
> tar {-c} [options] [files | directories]
> tar {-r | -u} -f archive-file [options] [files | directories]
> tar {-t | -x} [options] [patterns]

参数

> -A 新增压缩文件到已存在的压缩
>
> -B 设置区块大小
>
> -c 建立新的压缩文件
>
> -d 记录文件的差别
>
> -r 添加文件到已经压缩的文件
>
> -u 添加改变了和现有的文件到已经存在的压缩文件
>
> -x 从压缩的文件中提取文件
>
> -t 显示压缩文件的内容
>
> -z 支持gzip解压文件
>
> -j 支持bzip2解压文件
>
> -Z 支持compress解压文件
>
> -v 显示操作过程
>
> -l 文件系统边界设置
>
> -k 保留原有文件不覆盖
>
> -m 保留文件不被覆盖
>
> -W 确认压缩文件的正确性
>
> -b 设置区块数目
>
> -C 切换到指定目录
>
> -f 指定压缩文件

示例

~~~bash
#打包  tar -cvf 包名  文件名
$tar -cvf test.tar test.txt 
#解包  tar -xvf 包名 
$tar -xvf test.tar
#压缩  tar -czvf 包名 文件名
$tar -czvf test.tgz test.txt
#解压  tar -xzvf 包名
$tar -xzvf test.tgz
~~~



## wget

GNU Wget是一个免费实用程序，用于从Web非交互式下载文件。它支持HTTP，HTTPS和FTP协议，以及通过HTTP代理进行检索。

格式

> wget [option]... [URL]...

参数

> 启动：
>   -V,  --version           显示 Wget 的版本信息并退出。
>   -h,  --help              打印此帮助。
>   -b,  --background        启动后转入后台。
>   -e,  --execute=COMMAND   运行一个“.wgetrc”风格的命令。
>
> 日志和输入文件：
>   -o,  --output-file=FILE    将日志信息写入 FILE。
>   -a,  --append-output=FILE  将信息添加至 FILE。
>   -d,  --debug               打印大量调试信息。
>   -q,  --quiet               安静模式 (无信息输出)。
>   -v,  --verbose             详尽的输出 (此为默认值)。
>   -nv, --no-verbose          关闭详尽输出，但不进入安静模式。
>   -i,  --input-file=FILE     下载本地或外部 FILE 中的 URLs。
>   -F,  --force-html          把输入文件当成 HTML 文件。
>   -B,  --base=URL            解析与 URL 相关的
>                              HTML 输入文件 (由 -i -F 选项指定)。
>        --config=FILE         Specify config file to use.
>
> 下载：
>   -t,  --tries=NUMBER            设置重试次数为 NUMBER (0 代表无限制)。
>        --retry-connrefused       即使拒绝连接也是重试。
>   -O,  --output-document=FILE    将文档写入 FILE。
>   -nc, --no-clobber              skip downloads that would download to
>                                  existing files (overwriting them).
>   -c,  --continue                断点续传下载文件。
>        --progress=TYPE           选择进度条类型。
>   -N,  --timestamping            只获取比本地文件新的文件。
>   --no-use-server-timestamps     不用服务器上的时间戳来设置本地文件。
>   -S,  --server-response         打印服务器响应。
>        --spider                  不下载任何文件。
>   -T,  --timeout=SECONDS         将所有超时设为 SECONDS 秒。
>        --dns-timeout=SECS        设置 DNS 查寻超时为 SECS 秒。
>        --connect-timeout=SECS    设置连接超时为 SECS 秒。
>        --read-timeout=SECS       设置读取超时为 SECS 秒。
>   -w,  --wait=SECONDS            等待间隔为 SECONDS 秒。
>        --waitretry=SECONDS       在获取文件的重试期间等待 1..SECONDS 秒。
>        --random-wait             获取多个文件时，每次随机等待间隔
>                                  0.5*WAIT...1.5*WAIT 秒。
>        --no-proxy                禁止使用代理。
>   -Q,  --quota=NUMBER            设置获取配额为 NUMBER 字节。
>        --bind-address=ADDRESS    绑定至本地主机上的 ADDRESS (主机名或是 IP)。
>        --limit-rate=RATE         限制下载速率为 RATE。
>        --no-dns-cache            关闭 DNS 查寻缓存。
>        --restrict-file-names=OS  限定文件名中的字符为 OS 允许的字符。
>        --ignore-case             匹配文件/目录时忽略大小写。
>   -4,  --inet4-only              仅连接至 IPv4 地址。
>   -6,  --inet6-only              仅连接至 IPv6 地址。
>        --prefer-family=FAMILY    首先连接至指定协议的地址
>                                  FAMILY 为 IPv6，IPv4 或是 none。
>        --user=USER               将 ftp 和 http 的用户名均设置为 USER。
>        --password=PASS           将 ftp 和 http 的密码均设置为 PASS。
>        --ask-password            提示输入密码。
>        --no-iri                  关闭 IRI 支持。
>        --local-encoding=ENC      IRI (国际化资源标识符) 使用 ENC 作为本地编码。
>        --remote-encoding=ENC     使用 ENC 作为默认远程编码。
>        --unlink                  remove file before clobber.
>
> 目录：
>   -nd, --no-directories           不创建目录。
>   -x,  --force-directories        强制创建目录。
>   -nH, --no-host-directories      不要创建主目录。
>        --protocol-directories     在目录中使用协议名称。
>   -P,  --directory-prefix=PREFIX  以 PREFIX/... 保存文件
>        --cut-dirs=NUMBER          忽略远程目录中 NUMBER 个目录层。
>
> HTTP 选项：
>        --http-user=USER        设置 http 用户名为 USER。
>        --http-password=PASS    设置 http 密码为 PASS。
>        --no-cache              不在服务器上缓存数据。
>        --default-page=NAME     改变默认页
>                                (默认页通常是“index.html”)。
>   -E,  --adjust-extension      以合适的扩展名保存 HTML/CSS 文档。
>        --ignore-length         忽略头部的‘Content-Length’区域。
>        --header=STRING         在头部插入 STRING。
>        --max-redirect          每页所允许的最大重定向。
>        --proxy-user=USER       使用 USER 作为代理用户名。
>        --proxy-password=PASS   使用 PASS 作为代理密码。
>        --referer=URL           在 HTTP 请求头包含‘Referer: URL’。
>        --save-headers          将 HTTP 头保存至文件。
>   -U,  --user-agent=AGENT      标识为 AGENT 而不是 Wget/VERSION。
>        --no-http-keep-alive    禁用 HTTP keep-alive (永久连接)。
>        --no-cookies            不使用 cookies。
>        --load-cookies=FILE     会话开始前从 FILE 中载入 cookies。
>        --save-cookies=FILE     会话结束后保存 cookies 至 FILE。
>        --keep-session-cookies  载入并保存会话 (非永久) cookies。
>        --post-data=STRING      使用 POST 方式；把 STRING 作为数据发送。
>        --post-file=FILE        使用 POST 方式；发送 FILE 内容。
>        --content-disposition   当选中本地文件名时
>                                允许 Content-Disposition 头部 (尚在实验)。
>        --auth-no-challenge     发送不含服务器询问的首次等待
>                                的基本 HTTP 验证信息。
>
> HTTPS (SSL/TLS) 选项：
>        --secure-protocol=PR     选择安全协议，可以是 auto、SSLv2、
>                                 SSLv3 或是 TLSv1 中的一个。
>        --no-check-certificate   不要验证服务器的证书。
>        --certificate=FILE       客户端证书文件。
>        --certificate-type=TYPE  客户端证书类型，PEM 或 DER。
>        --private-key=FILE       私钥文件。
>        --private-key-type=TYPE  私钥文件类型，PEM 或 DER。
>        --ca-certificate=FILE    带有一组 CA 认证的文件。
>        --ca-directory=DIR       保存 CA 认证的哈希列表的目录。
>        --random-file=FILE       带有生成 SSL PRNG 的随机数据的文件。
>        --egd-file=FILE          用于命名带有随机数据的 EGD 套接字的文件。
>
> FTP 选项：
>        --ftp-user=USER         设置 ftp 用户名为 USER。
>        --ftp-password=PASS     设置 ftp 密码为 PASS。
>        --no-remove-listing     不要删除‘.listing’文件。
>        --no-glob               不在 FTP 文件名中使用通配符展开。
>        --no-passive-ftp        禁用“passive”传输模式。
>        --retr-symlinks         递归目录时，获取链接的文件 (而非目录)。
>
> 递归下载：
>   -r,  --recursive          指定递归下载。
>   -l,  --level=NUMBER       最大递归深度 (inf 或 0 代表无限制，即全部下载)。
>        --delete-after       下载完成后删除本地文件。
>   -k,  --convert-links      让下载得到的 HTML 或 CSS 中的链接指向本地文件。
>   -K,  --backup-converted   在转换文件 X 前先将它备份为 X.orig。
>   -m,  --mirror             -N -r -l inf --no-remove-listing 的缩写形式。
>   -p,  --page-requisites    下载所有用于显示 HTML 页面的图片之类的元素。
>        --strict-comments    用严格方式 (SGML) 处理 HTML 注释。
>
> 递归接受/拒绝：
>   -A,  --accept=LIST               逗号分隔的可接受的扩展名列表。
>   -R,  --reject=LIST               逗号分隔的要拒绝的扩展名列表。
>   -D,  --domains=LIST              逗号分隔的可接受的域列表。
>        --exclude-domains=LIST      逗号分隔的要拒绝的域列表。
>        --follow-ftp                跟踪 HTML 文档中的 FTP 链接。
>        --follow-tags=LIST          逗号分隔的跟踪的 HTML 标识列表。
>        --ignore-tags=LIST          逗号分隔的忽略的 HTML 标识列表。
>   -H,  --span-hosts                递归时转向外部主机。
>   -L,  --relative                  只跟踪有关系的链接。
>   -I,  --include-directories=LIST  允许目录的列表。
>   --trust-server-names             use the name specified by the redirection
>                                    url last component.
>   -X,  --exclude-directories=LIST  排除目录的列表。
>   -np, --no-parent                 不追溯至父目录。

示例

~~~bash
#下载某个文件，wget 文件的地址
$wget https://blog.csdn.net/qq_38646470
~~~



## netstat

netstat命令以符号形式显示各种与网络相关的数据结构的内容。有多种输出格式，具体取决于显示信息的选项。该命令的第一种形式显示每个协议的活动套接字列表。第二种形式根据选择的选项显示其他网络数据结构之一的内容。使用第三种形式，并指定等待间隔，netstat将在配置的网络接口上连续显示有关数据包流量的信息。第四种形式显示指定协议或地址族的统计信息。如果指定了等待间隔，将显示最近间隔秒的协议信息。第五种形式显示指定协议或地址族的每个接口的统计信息。第六种形式显示mbuf（9）统计信息。第七种形式显示指定地址系列的路由表。第八种形式显示路由统计信息。

格式

>   netstat [-AaLlnW] [-f address_family | -p protocol]
>      netstat [-gilns] [-v] [-f address_family] [-I interface]
>      netstat -i | -I interface [-w wait] [-c queue] [-abdgqRtS]
>      netstat -s [-s] [-f address_family | -p protocol] [-w wait]
>      netstat -i | -I interface -s [-f address_family | -p protocol]
>      netstat -m [-m]
>      netstat -r [-Aaln] [-f address_family]
>      netstat -rs [-s]

参数

> -a或–all 显示所有连线中的Socket。
>
> -A<网络类型>或–<网络类型> 列出该网络类型连线中的相关地址。
>
> -c或–continuous 持续列出网络状态。
>
> -C或–cache 显示路由器配置的快取信息。
>
> -e或–extend 显示网络其他相关信息。
>
> -F或–fib 显示FIB。
>
> -g或–groups 显示多重广播功能群组组员名单。
>
> -h或–help 在线帮助。
>
> -i或–interfaces 显示网络界面信息表单。
>
> -l或–listening 显示监控中的服务器的Socket。
>
> -M或–masquerade 显示伪装的网络连线。
>
> -n或–numeric 直接使用IP地址，而不通过域名服务器。
>
> -N或–netlink或–symbolic 显示网络硬件外围设备的符号连接名称。
>
> -o或–timers 显示计时器。
>
> -p或–programs 显示正在使用Socket的程序识别码和程序名称。
>
> -r或–route 显示Routing Table。
>
> -s或–statistice 显示网络工作信息统计表。
>
> -t或–tcp 显示TCP传输协议的连线状况。
>
> -u或–udp 显示UDP传输协议的连线状况。
>
> -v或–verbose 显示指令执行过程。
>
> -V或–version 显示版本信息。
>
> -w或–raw 显示RAW传输协议的连线状况。
>
> -x或–unix 此参数的效果和指定”-A unix”参数相同。
>
> –ip或–inet 此参数的效果和指定”-A inet”参数相同。

示例

~~~bash
#列出所有端口使用情况
$netstat -a
#显示当前UDP连接状况
$netstat -nu
#显示UDP端口号的使用情况
$netstat -apu
#显示网卡列表
$netstat -i
#显示网络统计信息
$netstat -s
#显示监听的套接口
$netstat -l
#显示所有已建立的有效连接
$netstat -n
#显示关于路由表的信息
$netstat -r
#列出所有 tcp 端口
$netstat -at
#找出程序运行的端口
$netstat -ap | grep ssh
#在 netstat 输出中显示 PID 和进程名称
$netstat -pt
~~~



## ifconfig

Ifconfig用于配置内核驻留的网络接口。它在引导时用于根据需要设置接口。之后，通常仅在调试或需要系统调整时才需要它。

格式

>  ifconfig [-v] [-a] [-s] [interface]
>  ifconfig [-v] interface [aftype] options | address ...

参数

> up 启动指定网络设备/网卡。
>
> down 关闭指定网络设备/网卡。该参数可以有效地阻止通过指定接口的IP信息流，如果想永久地关闭一个接口，我们还需要从核心路由表中将该接口的路由信息全部删除。
>
> arp 设置指定网卡是否支持ARP协议。
>
> -promisc 设置是否支持网卡的promiscuous模式，如果选择此参数，网卡将接收网络中发给它所有的数据包
>
> -allmulti 设置是否支持多播模式，如果选择此参数，网卡将接收网络中所有的多播数据包
>
> -a 显示全部接口信息
>
> -s 显示摘要信息（类似于 netstat -i）
>
> add 给指定网卡配置IPv6地址
>
> del 删除指定网卡的IPv6地址
>
> <硬件地址> 配置网卡最大的传输单元
>
> mtu<字节数> 设置网卡的最大传输单元 (bytes)
>
> netmask<子网掩码> 设置网卡的子网掩码。掩码可以是有前缀0x的32位十六进制数，也可以是用点分开的4个十进制数。如果不打算将网络分成子网，可以不管这一选项；如果要使用子网，那么请记住，网络中每一个系统必须有相同子网掩码。
>
> tunel 建立隧道
>
> dstaddr 设定一个远端地址，建立点对点通信
>
> -broadcast<地址> 为指定网卡设置广播协议
>
> -pointtopoint<地址> 为网卡设置点对点通讯协议
>
> multicast 为网卡设置组播标志
>
> address 为网卡设置IPv4地址
>
> txqueuelen<长度> 为网卡设置传输列队的长度

示例

~~~bash
#显示网络设备信息
$ifconfig
#启动关闭指定网卡
$ifconfig eth0 up
$ifconfig eth0 down
#配置IP地址
$ifconfig eth0 ip
#启用和关闭ARP协议
$ifconfig eth0 arp
$ifconfig eth0 -arp
#设置最大传输单元
$ifconfig eth0 mtu 1500
~~~

##### 解释

- eth0 表示第一块网卡， 其中 HWaddr 表示网卡的物理地址
- inet addr 用来表示网卡的IP地址
- lo 是表示主机的回坏地址，这个一般是用来测试一个网络程序，但又不想让局域网或外网的用户能够查看，只能在此台主机上运行和查看所用的网络接口。

> 第一行：连接类型：Ethernet（以太网）HWaddr（硬件mac地址）
>
> 第二行：网卡的IP地址、子网、掩码
>
> 第三行：UP（代表网卡开启状态）RUNNING（代表网卡的网线被接上）MULTICAST（支持组播）MTU:1500（最大传输单元）：1500字节
>
> 第四、五行：接收、发送数据包情况统计
>
> 第七行：接收、发送数据字节数统计信息。



## vmstat

vmstat报告有关进程，内存，页面调度，块IO，陷阱，磁盘和cpu活动的信息。

格式

> vmstat [options] [delay [count]]

参数

> -a：显示活跃和非活跃内存
>
> -f：显示从系统启动至今的fork数量 。
>
> -m：显示slabinfo
>
> -n：只在开始时显示一次各字段名称。
>
> -s：显示内存相关统计信息及多种系统活动数量。
>
> delay：刷新时间间隔。如果不指定，只显示一条结果。
>
> count：刷新次数。如果不指定刷新次数，但指定了刷新时间间隔，这时刷新次数为无穷。
>
> -d：显示磁盘相关统计信息。
>
> -p：显示指定磁盘分区统计信息
>
> -S：使用指定单位显示。参数有 k 、K 、m 、M ，分别代表1000、1024、1000000、1048576字节（byte）。默认单位为K（1024 bytes）

示例

~~~bash
#显示虚拟内存情况
$ vmstat
procs -----------memory---------- ---swap-- -----io---- -system-- ------cpu-----
 r  b   swpd   free   buff  cache   si   so    bi    bo   in   cs us sy id wa st
 1  0      0 23764228 507816 36953948    0    0     3     5    0    0  1  0 98  0  0
~~~

##### 解释

Procs（进程）：

r: 运行队列中进程数量

b: 等待IO的进程数量

Memory（内存）：

swpd: 使用虚拟内存大小

free: 可用内存大小

buff: 用作缓冲的内存大小

cache: 用作缓存的内存大小

Swap：

si: 每秒从交换区写到内存的大小

so: 每秒写入交换区的内存大小

IO：（现在的Linux版本块的大小为1024bytes）

bi: 每秒读取的块数

bo: 每秒写入的块数

系统：

in: 每秒中断数，包括时钟中断。

cs: 每秒上下文切换数。

CPU（以百分比表示）：

us: 用户进程执行时间(user time)

sy: 系统进程执行时间(system time)

id: 空闲时间(包括IO等待时间),中央处理器的空闲时间 。以百分比表示。

wa: 等待IO时间

```bash
#表示在3秒时间内进行3次采样。将得到一个数据汇总他能够反映真正的系统情况。
$vmstat 3 3
#查看系统fork多少次
$ vmstat -f
    166484246 forks
#查看内存使用的详细信息
$vmstat -s
#查看磁盘的读/写
$vmstat -d
#查看系统的slab信息
$vmstat -m
```



## free

free显示系统中可用和可用的物理内存和交换内存的总量，以及内核使用的缓冲区和高速缓存。

格式

> free [options]

参数

> -b 以Byte为单位显示内存使用情况。
>
> -k 以KB为单位显示内存使用情况。
>
> -m 以MB为单位显示内存使用情况。
>
> -g  以GB为单位显示内存使用情况。
>
> -o 不显示缓冲区调节列。
>
> -s<间隔秒数> 持续观察内存使用状况。
>
> -t 显示内存总和列。

示例

~~~bash
#显示内存使用情况
$ free
              total        used        free      shared  buff/cache   available
Mem:       65808884     4582700    23754736         684    37471448    60913052
$ free -h
              total        used        free      shared  buff/cache   available
Mem:            62G        4.4G         22G        684K         35G         58G
Swap:            0B          0B          0B
~~~

##### 解释

total:总计物理内存的大小。

used:已使用多大。

free:可用有多少。

Shared:多个进程共享的内存总额。

Buffers/cached:磁盘缓存的大小。

第三行(-/+ buffers/cached):

used:已使用多大。

free:可用有多少。

```bash
#周期性的查询内存使用信息，5s执行一次
$ free -s 5
```



## top

top程序提供正在运行的系统的动态实时视图。它可以显示系统摘要信息以及Linux内核当前正在管理的进程或线程的列表。所显示的系统摘要信息的类型以及为进程显示的信息的类型，顺序和大小都是用户可配置的，并且可以使配置在重新启动后保持不变。
    该程序为流程操作提供了一个有限的交互式界面，并为个人配置提供了更为广泛的界面-涵盖了其操作的各个方面。尽管在本文档中始终引用top，但是您可以随意为程序命名。然后，该新名称（可能是别名）将反映在顶部的显示屏上，并在读写配置文件时使用。

格式

> top -hv|-bcHiOSs -d secs -n max -u|U user -p pid -o fld -w [cols]

参数

> -b 批处理
>
> -c 显示完整的治命令
>
> -I 忽略失效过程
>
> -s 保密模式
>
> -S 累积模式
>
> -i<时间> 设置间隔时间
>
> -u<用户名> 指定用户名
>
> -p<进程号> 指定进程
>
> -n<次数> 循环显示的次数

示例

~~~bash
#top
$ top
top - 00:56:07 up 149 days, 14:40,  1 user,  load average: 0.00, 0.02, 0.05
Tasks: 254 total,   1 running, 253 sleeping,   0 stopped,   0 zombie
%Cpu(s):  1.4 us,  0.3 sy,  0.0 ni, 98.3 id,  0.1 wa,  0.0 hi,  0.0 si,  0.0 st
KiB Mem : 65808884 total, 23749772 free,  4586160 used, 37472952 buff/cache
KiB Swap:        0 total,        0 free,        0 used. 60909608 avail Mem

  PID USER      PR  NI    VIRT    RES    SHR S  %CPU %MEM     TIME+ COMMAND
24397 dongshan  20   0 17.972g 688312  13728 S   6.2  1.0   7:09.11 java
    1 root      20   0   42140   3684   1476 S   0.0  0.0  23:58.88 systemd
    2 root      20   0       0      0      0 S   0.0  0.0   0:05.47 kthreadd
    3 root      20   0       0      0      0 S   0.0  0.0   0:16.06 ksoftirqd/0
    5 root       0 -20       0      0      0 S   0.0  0.0   0:00.00 kworker/0:0H
    7 root      rt   0       0      0      0 S   0.0  0.0   1:27.00 migration/0
    8 root      20   0       0      0      0 S   0.0  0.0   0:00.00 rcu_bh
    9 root      20   0       0      0      0 S   0.0  0.0   0:00.00 rcuob/0
~~~

##### 解释

**第一行，任务队列信息，同 uptime 命令的执行结果，具体参数说明情况如下：**

00:56:07 — 当前系统时间

up 149 days, 14:40 — 系统已经运行了149天14小时40分钟（在这期间系统没有重启过的）

1users — 当前有1个用户登录系统

load average: 0.00, 0.02, 0.05 — load average后面的三个数分别是1分钟、5分钟、15分钟的负载情况。

load average数据是每隔5秒钟检查一次活跃的进程数，然后按特定算法计算出的数值。如果这个数除以逻辑CPU的数量，结果高于5的时候就表明系统在超负荷运转了。

**第二行，Tasks — 任务（进程）**

系统现在共有254个进程，其中处于运行中的有1个，253个在休眠（sleep），stoped状态的有0个，zombie状态（僵尸）的有0个。

**第三行，cpu状态信息**

%Cpu(s):  1.4 us,  0.3 sy,  0.0 ni, 98.3 id,  0.1 wa,  0.0 hi,  0.0 si,  0.0 st

1.4 us — 用户空间占用CPU的百分比。

0.3 sy — 内核空间占用CPU的百分比。

0.0 ni — 改变过优先级的进程占用CPU的百分比

98.3 id — 空闲CPU百分比

0.1 wa — IO等待占用CPU的百分比

0.0 hi — 硬中断（Hardware IRQ）占用CPU的百分比

0.0 si — 软中断（Software Interrupts）占用CPU的百分比

**第四行,内存状态**

65808884 total  物理内存总量

23749772 free  使用中的内存总量

4586160 used  空闲内存总量

37472952 buff/cache  缓存的内存量

**第五行，swap交换分区信息**

0 total  交换区总量

0 use  使用的交换区总量

0 free  空闲交换区总量

60909608 avail Mem  可用交换区总量

**第七行以下：各进程（任务）的状态监控**

PID — 进程id

USER — 进程所有者

PR — 进程优先级

NI — nice值。负值表示高优先级，正值表示低优先级

VIRT — 进程使用的虚拟内存总量，单位kb。VIRT=SWAP+RES

RES — 进程使用的、未被换出的物理内存大小，单位kb。RES=CODE+DATA

SHR — 共享内存大小，单位kb

S — 进程状态。D=不可中断的睡眠状态 R=运行 S=睡眠 T=跟踪/停止 Z=僵尸进程

%CPU — 上次更新到现在的CPU时间占用百分比

%MEM — 进程使用的物理内存百分比

TIME+ — 进程使用的CPU时间总计，单位1/100秒

COMMAND — 进程名称（命令名/命令行）



## sar

sar（System Activity Reporter系统活动情况报告）是目前 Linux 上最为全面的系统性能分析工具之一，可以从多方面对系统的活动进行报告，包括：文件的读写情况、 系统调用的使用情况、磁盘I/O、CPU效率、内存使用状况、进程活动及IPC有关的活动等。

格式

> sar [options] [-A] [-o file] t [n]

参数

> -A：所有报告的总和
>
> -u：输出CPU使用情况的统计信息
>
> -v：输出inode、文件和其他内核表的统计信息
>
> -d：输出每一个块设备的活动信息
>
> -r：输出内存和交换空间的统计信息
>
> -b：显示I/O和传送速率的统计信息
>
> -a：文件读写情况
>
> -c：输出进程统计信息，每秒创建的进程数
>
> -R：输出内存页面的统计信息
>
> -y：终端设备活动情况
>
> -w：输出系统交换活动信息



# 看日志

搜索日志

cat -n filename | grep "关键字"

实时监控100行日志

~~~bash
 tail -100f test.log
~~~

查询日志尾部10行日志

~~~bash
tail  -n  10  test.log
~~~

# 看端口



~~~shell
#找出程序运行的端口
$netstat -ap | grep ssh
~~~



# 看负载

先top 找出占用最高的进程的PID

通过　top -H -p PID　查看这个进程

