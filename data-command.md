数据操作起始于Pandas或Tidyverse。从理论上看，这个概念没有错。毕竟，这是为什么这些工具首先存在的原因。然而，对于分隔符转换等简单任务来说，这些选项通常可能是过于重量级了。

**有意掌握命令行应该在每个开发人员的技能链上，特别是数据科学家。学习shell中的来龙去脉无可否认地会让你更高效。**除此之外，命令行还在计算方面有一次伟大的历史记录。例如，awk - 一种数据驱动的脚本语言。Awk首次出现于1977年，它是在传奇的K&R一书中的K，Brian Kernighan的帮助下出现的。在今天，大约50年之后，awk仍然与每年出现的新书保持相关联！ 因此，可以肯定的是，对命令行技术的投入不会很快贬值的。



### ICONV

文件编码总是棘手的问题。目前大部分文件都是采用的 UTF-8 编码。要想了解 UTF-8 的魔力，可以看看这个优秀的视频。尽管如此，有时候我们还是会收到非 UTF-8 编码的文件。这种情况下就需要尝试转码。iconv 就是这种状况下的救世主。

 输入/输出格式规范：
  -f, --from-code=名称     原始文本编码
  -t, --to-code=名称       输出编码

 信息：
  -l, --list                 列举所有已知的字符集

 输出控制：
  -c                         从输出中忽略无效的字符
  -o, --output=FILE          输出文件
  -s, --silent               关闭警告
      --verbose              打印进度信息

  -?, --help                 给出该系统求助列表
      --usage                给出简要的用法信息
  -V, --version              打印程序版本号

~~~linux

~~~

iconv -f ISO-8859-1 -t UTF-8 < input.txt > output.txt

\#Converting -f (from) latin1 (ISO-8859-1)
\#-t (to) standard UTF_8

### HEAD

如果你是重度Pandas的用户，那么你会对head很熟悉。通常在处理新数据时，我们想要做的第一件事就是了解究竟存在那些东西。这会引起Panda启动，读取数据，然后调用df.head() - 很费劲，至少可以说。head，不需要任何标志，将输出文件的前10行。head真正的能力在于彻查清除操作。 例如，如果我们想将文件的分隔符从逗号改变为pipe通配符。

一个快速测试将是：head mydata.csv | sed 's/,/|/g'

~~~linux
# Prints out first 10 lines
head filename.csv
# Print first 3 lines
head -n 3 filename.csv
~~~



### TR

Tr类似于翻译，它是基于文件清理的一个强大使用的工具。一个理想的用法是替换文件中的分隔符。

~~~linux
#将文件中的制表符分割转换成逗号
cat tab_delimited.txt | tr "	" "," comma_delimited.csv
[root@wsh myroot]# cat aa.txt |tr "," "."    替换所有的，为 .
~~~

Tr的另一个特性是在你的处理中设置上所有的[:class:]变量。包括：

~~~html
[:alnum:] 所有字母和数字
[:alpha:] 所有字母
[:blank:] 所有水平空白
[:cntrl:] 所有控制字符
[:digit:] 所有数字
[:graph:] 所有可打印的字符，不包括空格
[:lower:] 全部小写字母
[:print:] 所有可打印的字符，包括空格
[:punct:] 所有标点符号
[:space:] 所有的水平或垂直空格
[:upper:] 全部大写字母
[:xdigit:] 所有十六进制数字
~~~

可以将这些多样化的变量链接在一起，组成一个强大的程序。下面是一个基于字数统计的程序，用来检查你的README文件是否使用过度。

~~~linux
cat README.md | tr "[:punct:][:space:]
~~~

另外一个例子用于正则表达式

~~~linux
# 将所有的大写字母转换成小写
cat filename.csv | tr '[A-Z]' '[a-z]'
[root@wsh myroot]# cat aa.txt |tr '[A-Z]' '[a-z]'
,我,是,大,帅,哥
abdgsagasgfghbrvgefhgbrg
[root@wsh myroot]# cat aa.txt |tr '[a-z]' '[A-Z]'
,我,是,大,帅,哥
ABDGSAGASGFGHBRVGEFHGBRG
~~~

- **有用的选项：**

- - tr -d删除字符
  - tr -s压缩字符
  - 退格
  - 换页
  - 垂直选项卡
  - NNN八进制值为NNN的字符

### wc

##### 字数统计。它的价值主要体现在使用 -l 参数可以进行行数统计    特别方便！！

~~~LINUX
# Will return number of lines in CSV
wc -l gigantic_comma.csv
[root@wsh myroot]# wc -l aa.txt 
2 aa.txt
[root@wsh myroot]# wc -c aa.txt 
46 aa.txt
[root@wsh myroot]# wc -m aa
wc: aa: 没有那个文件或目录
[root@wsh myroot]# wc -m aa.txt 
36 aa.txt
[root@wsh myroot]# wc aa.txt 
 2  2 46 aa.txt
[root@wsh myroot]# wc -L bb.txt 
129 bb.txt
[root@wsh myroot]# wc -L aa.txt 
24 aa.txt
[root@wsh myroot]# wc -w aa.txt 
2 aa.txt
[root@wsh myroot]# wc -w bb.txt 
67 bb.txt
~~~

用这个工具来验证各个命令的输出实在方便。因此，如果我们要在文件中转换分隔符，然后运行 wc -l，验证总行数是相同的。如果不同，我们就知道一定是哪里出错了。



- **常用选项：**

- - wc -c 打印字节数
  - wc -m 打印字符数
  - wc -L 打印最长一行的长度
  - wc -w 打印字数

