---
title: terminal learning notes
author: J Song
date: '2024-09-03'
slug: []
categories:
  - 技术文章
  - 编程
description: 2024-09-03-terminal-learning-notes
keywords: 2024,09,03,terminal,learning,notes
lastmod: '2024-09-25T01:30:43+08:00'
---

Terminal 自学笔记

<!--more-->
> 参考 [Bioinfotec](https://blog.csdn.net/m0_56572447/article/details/131148134)、[遗落凡尘的萤火](https://blog.csdn.net/weixin_57975238/article/details/138159580?utm_medium=distribute.pc_relevant.none-task-blog-2~default~baidujs_baidulandingword~default-0-138159580-blog-131148134.235^v43^pc_blog_bottom_relevance_base6&spm=1001.2101.3001.4242.1&utm_relevant_index=1) 、[Odette George](https://medium.com/@odettegeorge/bioinformatics-working-with-awk-and-bioawk-c6587c330575)、[Omics tutorials](https://omicstutorials.com/using-awk-script-in-bioinformatics-analysis/) 博文

## 操作和函数

### 编辑文件

`vi`：文件编辑（没有文件时会创建新文件），编辑完后按`Esc`退出编辑模式，然后用`:wq`保存并退出或用`:q!`不保存退出。

- 创建、删除目录 `mkdir`,`rmdir` 
```bash
mkdir -p parent/temp  ## 在另一个目录下创建一个目录
rmdir -rf temp  ## 删除目录及其内的文件
```

### 压缩和提取文件
```bash
tar -xvf archive.tar      # 从.tar 存档中提取文件
tar -tf archive.tar       # 列出.tar存档中的文件（不提取）
tar -xvf archive.tar.gz   # 从 .tar.gz 存档中提取文件
tar -xvf archive.tar.bz2  # 从 .tar.bz2 文件中提取文件
tar -cvf archive.tar dir  # 创建 tar 存档
tar -czvf archive.tar.gz dir
tar -cjvf archive.tar.bz2 dir
gunzip file.gz  # 提取 .gz 压缩文件
gzip file.fastq # 创建 .gz 压缩文件
bunzip2 file.bz2  # 提取 .bzip2 压缩文件
bzip2 file.fastq  # 创建 .bzip2 压缩文件
unzip archive.zip # 提取 .zip 存档
zip archive.zip file.txt  # 创建 .zip存档
zip -r archive.zip dir  #创建 .zip存档（对文件夹）
```

### 查看文件

```bash
head -n 20 file.txt   ## 查看 file.txt 文件前 20 行
cat -n file.txt | head -10  ## 展示 file.txt 文件中的前 10 行，并打印出所有行号
more file.txt ## 逐页查看，按空格翻页，按回车换行，按 q 退出
## less 上下左右键查看文本内容，回车向下移动一行，空格翻页，q 退出，-N 显示行号，-S 单行显示，可以查看压缩文件（less 或 zless）
less file.txt | head -1
## 统计文本 wc -l 统计行数 -w 统计字符串数 -c 统计字节数（包括不可见字符，如回车）
wc file.txt # 依次为行数、字符串数、字节数
```

### 字符替换
tr：字符替换

常见用法: tr ‘’ ‘’

常见参数： 

-d：删除指定字符 

-s：缩减连续重复字符
```bash
cat readme.txt | tr 'a' 'A'              ##将readme.txt里的小写a都替换成A
cat readme.txt | tr ' ' '\t'              ##将readme.txt里的空格都替换成tab键
cat readme.txt | tr 'a-z' 'A-Z'      ##将readme.txt里的小写都替换成大写
cat readme.txt | tr -d ' '                ##将readme.txt里的空格都删掉
cat readme.txt | tr -d '\n'            ##将readme.txt里的回车键'\n'都删掉
cat Data/example.gtf | cut -f 3 | sort | uniq -c               
cat Data/example.gtf | cut -f 3 | sort | uniq -c | tr -s ' '     ## tr -s缩减
```

### 合并文件
行合并：cat

列合并：paste（默认分隔符为`\\t`)
```bash
cat file1.txt file2.txt > merged_file.txt ## 合并 file1 和 file2 的内容
cat file3.txt >> merged_file.txt  ## 追加 file3 的内容到 merged_file.txt
paste file1.txt file2.txt ## 将file1和file2左右合在一起,paste在合并文件时,中间是加tab键作为分隔符
paste -d ': ' file1 file2 ##  -d指定':'为分隔符
paste file1 file2  -s ## 将file1和file2左右合在一起,paste在合并文件时,并转秩     

## paste 还可以合并字符
seq 20 | paste - - - -  ## 将 1-20 数字用paste排序(每个 - 之间都有一个空格)     
cat Data/example.gtf | cut -f 3 | sort | uniq -c | tr -s ' ' | cut -d ' ' -f 2 | paste -s -d '+' | bc ## 提取第 3 列，排序去重，取出数字相加计算求和
echo "1+1" | bc ## bc 的作用可以计算“1+1”这种形式 
less Data/example.fq | paste - - - - | sort -k 2 -r | less -SN  ##先用paste变成一个整体,再排序  
```

### 提取特定列

cut： 用于取出分隔符分隔的文本中的几列

常见参数：

-d 指定分隔符，默认\t；

-f 输出哪几列（字段fields）输出第几列，可以是很多列，用,隔开
```bash
cat Data/example.gtf | less -SN  ##less查看文本内容,单行显示
cat Data/example.gtf | cut -f 1  ## 取出第一列
cat Data/example.gtf | cut -f 1 | less -SN ## 取出第一列,用less查看
cat Data/example.gtf | cut -f 1,3-5,7 |head  ## 取出第1, 3,4,5,7列(乱序也能正常选取),用head查看
cat file | cut -d ',' -f 1  ##假设file是个CSV文件,将分隔符指定为逗号,进行选取
less -S Data/example.gtf | cut -d 'h' -f 1  ##指定h为分隔符,进行选取
```

### 排序
[sort](https://en.wikipedia.org/wiki/Sort_(Unix))：排序

常见参数：

-n：按照数值从小到大进行排序

-V：字符串中含有数值时，按照数值从小到大排序

-r：逆向排序

-k：指定按哪一列排序

-t：指定分隔

-u: 去掉重复
```bash
less -S Data/example.gtf | sort -k 3 | less -SN  ##less查看example.gtf,按照第三列排序,用less查看
cat Data/example.gtf | sort -k 3 | less -SN       ##cat查看example.gtf,按照第三列排序,用less查看,当文件比较大时用cat打开的比less快,小文件无差别
cat Data/example.gtf | sort -k 4 -n | less -SN   ##cat查看example.gtf,按照第4列排序,按照数值来理解,less查看
cat Data/example.gtf | sort -k 4 -n -r | less -SN  ##cat查看example.gtf,按照第4列排序,按照数值来理解,逆向排序,less查看
sort -k2,2 -t $'\t' phonebook ## 指定分隔符为 tab
```
可以按多列排序
```bash
$ cat quota
fred 2000
bob 1000
an 1000
chad 1000
don 1500
eric 500

$ sort -k2,2n -k1,1 quota
eric 500
an 1000
bob 1000
chad 1000
don 1500
fred 2000
```
Here the first sort is done using column 2. -k2,2n specifies sorting on the key starting and ending with column 2, and sorting numerically. If -k2 is used instead, the sort key would begin at column 2 and extend to the end of the line, spanning all the fields in between. -k1,1 dictates breaking ties using the value in column 1, sorting alphabetically by default. Note that bob, and chad have the same quota and are sorted alphabetically in the final output.

### 去重
uniq：去除重复行  ##uniq比较”懒”，只能去除相邻的重复行! 因此记得要跟sort连用!

常见参数：

-c：统计每个字符串连续出现的行数
```bash
cat Data/example.gtf | cut -f 3 | head -20 | uniq          ##uniq 单独用起不到去重的作用
cat Data/example.gtf | cut -f 3 | head -20 | sort | uniq      ##sort与uniq连用才可以
cat Data/example.gtf | cut -f 3 | head -20 | sort | uniq -c   ##统计每个字符串连续出现的行数
cat Data/example.gtf | cut -f 3 | head -20 | sort | uniq -c| cat -A   ##统计每个字符串连续出现的行数, 打印所有内容，包括特殊字符，如制表符
```


### [grep](https://www.geeksforgeeks.org/grep-command-in-unixlinux/)
```bash
grep -i "UNix" file.txt ## 不区分大小写 case insensitive search
grep -c "unix" file.txt ## 只显示行数 number of lines that matches the given string
grep -l "unix" * ## 查找包含该字符的文件 files that contains the given string，也可跟特定文件，空格区隔
grep -w "unix" file.txt ## 全词查找 by default, grep matches the given string/pattern even if it is found as a substring in a file. The -w option to grep makes it match only the whole words. 
grep -o "unix" file.txt ## 只显示匹配的字符串 by default, grep displays the entire line which has the matched string. We can make the grep to display only the matched string by using the -o option. 
grep -n "unix" file.txt ## 额外显示行数
grep -v "unix" file.txt ## 显示不匹配的行
grep "^unix" file.txt ## 匹配以 unix 开头的行
grep "os$" file.txt ## 匹配以 os 结尾的行
grep –e "Agarwal" –e "Aggarwal" –e "Agrawal" file.txt ## specifies expression with -e option
grep -f pattern.txt file.txt ## 匹配 pattern.txt 文件中的字符（每行一个）
grep -A1 "unix" file.txt ## 显示匹配行以及匹配行之后的 1 行
grep -B2 "unix" file.txt ## 显示匹配行以及匹配行之前的 2 行
grep -C3 "unix" file.txt ## 显示匹配行以及匹配行前后 3 行
grep -R unix /home/geeks ## 显示/home/geeks 文件夹下的哪些文件中包含 unix
```

### [sed](https://www.geeksforgeeks.org/sed-command-in-linux-unix-with-examples/) 
- 替换
```bash
sed 's/unix/linux/' geekfile.txt ## 把文件中每行的第 1 个 unix 替换为 linux
sed 's/unix/linux/2' geekfile.txt ## 把文件中每行的第 2 个 unix 替换为 linux
sed 's/unix/linux/g' geekfile.txt ## 把文件中所有 unix 都替换为 linux
sed 's/unix/linux/3g' geekfile.txt ## 把文件中每行的第 3 个以及以后的 unix 都替换为 linux
echo "Welcome To The Geek Stuff" | sed 's/\(\b[A-Z]\)/\(\1\)/g' ## 把每个词第一个字母用括号括起来
sed '3 s/unix/linux/' geekfile.txt ## 只替换第 3 行第一个 unix
sed '1,3 s/unix/linux/' geekfile.txt ## 只替换第 1 到第 3 行第一个 unix
sed '2,$ s/unix/linux/' geekfile.txt ## 替换第 2 行以及之后的所有行，每行第一个 unix
sed 's/unix/linux/p' geekfile.txt ## 替换，并且匹配的行 print 两次
sed -n 's/unix/linux/p' geekfile.txt ## 只显示替换的行
```

- 删除
```bash
sed 'nd' filename.txt ## 删除第 n 行
sed '$d' filename.txt ## 删除最后一行
sed 'x,yd' filename.txt ## 删除第 x 到第 y 行
sed 'n,$d' filename.txt ## 删除第 n 到最后一行
sed '/pattern/d' filename.txt ## 删除有 pattern 的行
sed '/pattern/i\
the new line
' geekfile.txt ## 在匹配行前插入一行
sed '/pattern/a\
the new line' geekfile.txt ## 在匹配行后插入一行
```

- script
```bash
#!/bin/bash
input="input.txt"
output="output.txt"
sed 's/old/new/g' "$input" > "$output"
```

### [正则表达式](https://www.yiibai.com/sed/sed_regular_expressions.html)

- `^`  行开始
- `$` 行尾
- `.` 单点字符，匹配除行字符结尾的任何单个字符, 如 `sed -n '/^..t$/p'` (可匹配到一个或多个)
- `[]` 字符集，匹配括号中的任意一个字符
```bash
echo -e "Call\nTall\nBall" | sed -n '/[CT]all/ p'
```
- `[^]` 匹配不在括号中字符的字符，如 `sed -n '/[^CT]all/ p'` 不会匹配到 Call 和 Tall
```bash
echo -e "Call\nTall\nBall" | sed -n '/[^CT]all/ p'
```
- `[-]` 匹配字符范围，`sed -n '/[C-Z]all/ p'`
- `\?` 匹配 0 个或 1 个前面的字符，`sed -n '/Behaviou\?r/ p` 可以匹配到 Behaviour 和 Behavior
```bash
echo -e "Behaviour\nBehavior" | sed -n '/Behaviou\?r/ p'
```
- `\+` 匹配 1 次或多次前面的字符
```bash
echo -e "111\n22\n123\n234\n456\n222"  | sed -n '/2\+/ p'
```
- `*` 匹配任意次数（0 次或多次）
```bash
echo -e "ca\ncat" | sed -n '/cat*/ p'
```
- `{n}` 完全一致的出现 n 次匹配字符
```bash
echo -e '1000\n100\n10101010' | sed -n '/^[0-9]\{3\}$/ p'
```
- `{n,}` 最少出现 n 次
```bash
echo -e '1000\n100\n10101010' | sed -n '/^[0-9]\{3,\}$/ p'
```
- `{m,n}` 出现 m 到 n 次
```bash
echo -e '1000\n100\n10101010' | sed -n '/^[0-9]\{4,8\}$/ p'
```
- 有些符号在匹配时需要多加一个 `\`，包括：`\\,\n,\r,\dnnn`
- `[:alnum:]` 字母和数字字符，任意个数，不匹配制表符
```bash
echo -e "One\n123\n\t" | sed -n '/[[:alnum:]]/ p'
```
- `[:alpha:]` 只匹配字母字符，任意个数
```bash
echo -e "One\n123\n\t" | sed -n '/[[:alpha:]]/ p'
```
- `[:digit:]` 只匹配小数（整数也可以）
```bash
echo -e "One\n123\n\t" | sed -n '/[[:digit:]]/ p'
echo -e "One\n123.12\n\t" | sed -n '/[[:digit:]]/ p'
```
- `[:blank:]` 任何空格或制表符
- `[:lower:]` 小写字母
- `[:upper:]` 大写字母
```bash
echo -e "one\nTWO\n\t" | sed -n '/[[:upper:]]/ p'
```
- `[:punct:]` 标点符号，包括不是空格或字母数字的字符
- `[:space:]` 空格
- `\b` 表示边界 （苹果电脑自带的 sed 不支持，可用 `^` 和 `$` 代替）
```bash
echo -e "these\nthe\nthey\nthen" | sed -n '/\bthe\b/ p'
```
- `\B` 表示非边界 （苹果电脑自带的 sed 不支持，可用 . 代替）
```bash
echo -e "these\nthe\nthey" | sed -n '/the\B/ p'
```
- `\s` 单个空格字符 （苹果电脑自带的 sed 不支持）
```bash
echo -e "Line\t1\nLine2" | sed -n '/Line\s/ p'
```
- `\S` 单个非空格，`\w` 单个字符，`\W` 单个非字符，`\`` 模式空间开始 （苹果电脑自带的 sed 均不支持）



### [awk](https://www.geeksforgeeks.org/awk-command-unixlinux-examples/)
#### 1. AWK Operations: 
(a) Scans a file line by line \
(b) Splits each input line into fields \
(c) Compares input line/fields to pattern \
(d) Performs action(s) on matched lines 

#### 2. Syntax:
```bash
awk options 'selection _criteria {action}' input-file > output-file
awk '{if (condition) action}' file
awk BEGIN{sum=0};{s+=$2-$3};END{print "mean: " s/NR} file
```
options
```
-f program-file : Reads the AWK program source from the file 
                  program-file, instead of from the 
                  first command line argument.
-F fs            : Use fs for the input field separator
-v : 用于指定变量
```

examples
```bash
## match 第一列为 10 的，并提取第二列和第三列之差大于 10 的
awk '$1 ~ /10/ && $3-$2 > 1 {print $1 "\t" $2 "\t" $3}' example.bed
awk '{if ($1 == 10 && $3-$2 > 1) print $1 "\t" $2 "\t" $3}' example.bed
## 计算第 2 列减第 3 列之差的平均数
awk BEGIN{sum=0};{s+=$2-$3};END{print "mean: " s/NR} example.bed
```


#### 3. Built-In Variables In Awk

Awk’s built-in variables include the field variables—\$1, \$2, \$3, and so on (\$0 is the entire line) — that break a line of text into individual words or pieces called fields. 

- NR: NR command keeps a current count of the number of input records. Remember that records are usually lines. Awk command performs the pattern/action statements once for each record in a file. 
- NF: NF command keeps a count of the number of fields (columns) within the current input record. 
- FS: FS command contains the field separator character which is used to divide fields on the input line. The default is “white space”, meaning space and tab characters. FS can be reassigned to another character (typically in BEGIN) to change the field separator. 
- RS: RS command stores the current record separator character. Since, by default, an input line is the input record, the default record separator character is a newline. 
- OFS: OFS command stores the output field separator, which separates the fields when Awk prints them. The default is a blank space. Whenever print has several parameters separated with commas, it will print the value of OFS in between each parameter. 
- ORS: ORS command stores the output record separator, which separates the output lines when Awk prints them. The default is a newline character. print automatically outputs the contents of ORS at the end of whatever it is given to print. 

#### 4. Example
```bash
cat employee.txt
# ajay manager account 45000
# sunil clerk account 25000
# varun manager sales 50000
# amit manager account 47000
# tarun peon sales 15000
# deepak clerk sales 23000
# sunil peon sales 13000
# satvik director purchase 80000
```
- 查找特定 pattern
```bash
awk '/manager/ {print}' employee.txt 
```
output:
```bash
ajay manager account 45000
varun manager sales 50000
amit manager account 47000 
```
- 提取特定列
```bash
awk '{print $1,$4}' employee.txt
```
output:
```bash
ajay 45000
sunil 25000
varun 50000
amit 47000
tarun 15000
deepak 23000
sunil 13000
satvik 80000
```
- 用 NR built-in variables 提取特定列
```bash
awk '{print NR,$0}' employee.txt
```
output:
```bash
1 ajay manager account 45000
2 sunil clerk account 25000
3 varun manager sales 50000
4 amit manager account 47000
5 tarun peon sales 15000
6 deepak clerk sales 23000
7 sunil peon sales 13000
8 satvik director purchase 80000 
```
- 用 NF built-in variables 提取特定列，`$NF`指代最后一列
```bash
awk '{print $1,$NF}' employee.txt 
```
output:
```bash
ajay 45000
sunil 25000
varun 50000
amit 47000
tarun 15000
deepak 23000
sunil 13000
satvik 80000 
```
- 提取 3-6 行
```bash
awk 'NR==3, NR==6 {print NR,$0}' employee.txt 
```
output:
```bash
3 varun manager sales 50000
4 amit manager account 47000
5 tarun peon sales 15000
6 deepak clerk sales 23000 
```

- others
```bash
echo "hello world" | awk '/hello/ {print "Match found"}' ## Match found
echo "hello world" | awk '{sub(/hello/, "hi"); print}' ## hi world
echo "hello hello world" | awk '{gsub(/hello/, "hi"); print}' ## hi hi world
echo "a,b,c" | awk '{n = split($1, a, /,/); for (i=1; i<=n; i++) {print a[i]}}' 
## a
## b
## b
echo "hello world" | awk '{match($0, /hello/); print substr($0, RSTART, RLENGTH)}' ## hello
awk '{getline line < "input.txt"; print line > "output.txt"}'
awk '{if($0 !~ /^#/) print "chr"$0; else print $0}' infile.no_chr.vcf > infile.vcf
grep '^#' in.vcf > out.vcf && grep -v '^#' in.vcf | sort -V -k1,1 -k2,2n >> out.vcf
find ${indir} -type f -name "*tier3.bed" -print0 | wc -l --files0-from=- ## Count the total number of lines in the selected files of a dir
find ${indir} -type f -name "*tier3.bed" -exec cat {} + | wc -l ## Grab the body of a file excluding the header
```

## 其他
```bash
base=$(basename $infile .txt) # Extract the base filename without the extension in a shell script
line_count=$(wc -l < data.txt) # Count the number of lines in a file
```

example of a shell script
```bash
#!/bin/bash

# Define variables
indir="/path/to/input/files"
outdir="/path/to/output/files"

# Create output directory if it doesn't exist
mkdir -p $outdir

# Process each file in the input directory
for infile in $indir/*.txt; do
 # Extract the base filename without the extension
 base=$(basename $infile .txt)

 # Perform operations on the input file using a pipeline
 # For example, filter lines based on a condition, calculate summary statistics, and generate a report
 awk '{if ($1 > 5) print $0}' $infile > $outdir/$base.filtered
 awk '{sum += $1} END {print sum}' $infile > $outdir/$base.sum
 awk '{count[$1]++} END {for (i in count) print i, count[i]}' $infile | sort -rn > $outdir/$base.report
done
```

## 常用 linux 命令 cheat sheet
![linux_command.jpeg](/imgs/linux_command.jpeg)
