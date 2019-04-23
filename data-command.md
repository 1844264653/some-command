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





### SPLIT

文件大小可以有显著变化。根据工作的不同，拆分文件是有益的，就像split。基本用法如下：

~~~liunx
#我们拆分这个CSV文件，每500行分割为一个新的文件new_filename

split -l 500 filename.csv new_filename_

# filename.csv
# ls output
# new_filename_aaa
# new_filename_aab
# new_filename_aac
两个地方很奇怪：一个是命名方式，一个是缺少扩展名。后缀约定可以通过-d标识来数字化。添加文件扩展名，你需要执行下面这个find命令。他会给当前文件夹下的所有文件追加.csv后缀，所以需要小心使用。
find . -type f -exec mv '{}' '{}'.csv ;

# ls output
# filename.csv.csv
# new_filename_aaa.csv
# new_filename_aab.csv
# new_filename_aac.csv
~~~

- **有效的选项：**

- - split -b按特定字节大小拆分
  - split -a生成长度为N的后缀
  - split -x使用十六进制后缀分割



### **SORT & UNIQ**

前面的命令是显而易见的：他们按照自己说的做。这两者提供了最重要的一击（即去重单词计数）。这是由于有uniq，它只处理重复的相邻行。因此在管道输出之前进行排序。一个有趣的事情是，sort -u将获得与sort file.txt | uniq相同的结果。



Sort确实对数据科学家来说是一种很有用的小技巧：能够根据特定的列对整个CSV进行排序。

~~~linux
# Sorting a CSV file by the second column alphabetically
sort -t"," -k2,2 filename.csv
# Numerically
sort -t"," -k2n,2 filename.csv
# Reverse order
sort -t"," -k2nr,2 filename.csv
这里的-t选项是指定逗号作为分隔符。通常假设是空格或制表符。此外，-k标志是用来指定我们的键的。它的语法是-km,n，m是起始字段，n是最后一个字段。
~~~

- **有用的选项：**

- - sort -f 忽略大小写
  - sort -r 逆序
  - sort -R 乱序
  - uniq -c 计算出现次数
  - uniq -d 只打印重复行



### cut命令

~~~linux
#cut用于删除列。举个栗子，如果我们只想要第一列和第三列。
cut -d, -f 1,3 filename.csv
#选择除了第一列以外的所有列
cut -d, -f 2- filename.csv
#与其他的命令组合使用，cut命令作为过滤器
＃打印存在“some_string_value”的第1列和第3列的前10行
head filename.csv | grep "some_string_value" | cut -d, -f 1,3
#找出第二列中唯一值的数量。
cat filename.csv | cut -d, -f 2 | sort | uniq | wc -l
# 计算唯一值出现的次数，限制输出前10个结果
cat filename.csv | cut -d, -f 2 | sort | uniq -c | head
~~~



### **PASTE**

paste 是个**有趣的小命令**。如果你想合并两个文件，而这两个文件的内容**又正好是有序的**，那 paste 就可以这样做。

~~~linux
# names.txt
adam
john
zach

# jobs.txt
lawyer
youtuber
developer

# Join the two into a CSV

paste -d ',' names.txt jobs.txt > person_data.txt

# Output
adam,lawyer
john,youtuber
zach,developer
~~~

#### 其实都是一些sql语句的变化   下面还有

​       	关于更多 SQL_-esque 变体，请看下面。



