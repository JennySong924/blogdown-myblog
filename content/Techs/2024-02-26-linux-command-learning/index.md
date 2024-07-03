---
title: linux 命令学习
author: Package Build
date: '2024-02-26'
slug: []
categories: 
  - 技术文章
  - 编程
tags: 
  - linux
---
新建xueke.txt的文件
```bash
vi xueke.txt
touch xueke.txt
```

去重 uniq

文件的压缩和解压
```bash
tar -xf xueke.tar       #tar格式解压
tar -cf xueke.tar *.fa  #打包成tar格式
gunzip xueke.fa.gz      #gz格式解压
gzip xueke.fa           #压缩成gz格式
```

任务管理
```bash
top             # 显示进程信息
top -c		# 显示完整命令
top -b          # 以批处理模式显示程序信息
htop		# 查看CPU，内存的使用情况
kill 12345      # 杀死进程
kill -9 123456  # 彻底杀死进程
```

文件查找
```bash
#将当前目录及其子目录下所有文件后缀为 .c 的文件列出来:
find . -name "*.c"
#搜索 etc 目录下所有以 sh 开头的文件
locate /etc/sh
#忽略大小写搜索当前用户目录下所有以 r 开头的文件
locate -i ~/r
# fastp文件的绝对路径					
which fastp
#案例一：搜索Data目录下以点fna结尾的文件；
find  ../Data -name *.fna

#案例二：搜索系统中最近5分钟内编辑过的文件；
find / -amin 5

#案例三：查找大于100M的文件；
find ./ -size 100M

案例四：按照文件类型搜索；
find  ./ -type 文件类型
c 的档案
d: 目录 
b: 区块装置档案 ，
p: 具名贮列
f: 一般档案
l: 符号连结
s: socket

#案例五：搜索文件，直接处理；
find ./temp/ -name *.fna -exec rm '{}' \;
```

grep 
```bash
#案例一：统计fasta文件中序列的条数；
grep -c ">"  gene.ffn
#案例二：输出满足条件的序列；
grep -A  "3 gi 29732 34486" lastz.axt
#案例三：筛选出不满足条件的内容；
ps -fx | grep -v "S"
```

sed
```bash
#案例一：输出固定的行
sed -n '1307p'  seq.fna   #输出文件第1307行；
sed -n '100,200' seq.fna  #输出文件第100到200行；

#案例二：替换操作
sed -e 's/gi/GI/' seq.fna  #将文件中gi全部替换为大写GI；
sed -i 's/gi/GI/g' seq.fna   #在原文件上进行替换，并且进行全部替换；
sed -i.bak 's#GI#gi#' seq.fna  #在原文件上进行替换，并进行备份；
sed -e 's/gi/GI/2；s/ref/REF/2' seq.fna   #只将第二次出现的gi和ref进行替换；
sed -f sed.list cds.list    #根据文件中的模式进行替换，可同时进行多条件替换；
sed -n 's/gi/GI/p' seq.fna  #打印发生替换的行；

#案例三：删除空白行；
sed -e '/^\s*$/d'  seq.fna  #删除文件中的空白行；

#案例四：行寻址
sed -n '/ref/p' seq.fna   #输出文件中包含ref关键字的行；
sed  '100,2000s/GI/gi/g' seq.fa  #则只替换100行到2000行的内容；
sed  '100,2000！s/GI/gi/g' seq.fa  #加感叹号取反，在这个范围之外的执行操作；

#案例五：删除操作
sed -e '/>/d' seq.fna #删除包含ref的行；
sed -e 's/:.*//g' seq.fna   #删除冒号之后的所有内容；

#案例六：对应替换，类似于tr的功能
sed -e 'y/ATCG/atcg/' seq.fna  #修改大小写
sed -e '/>/!y/ATCG/atcg/' seq.fna  #DNA序列反向互补配对，并修改大小写
```

awk
```bash
#案例1：输出一个列表任意行；
awk '{print $1}' blast_m8.out  #输出blast m8 格式结果的第一行；
awk -F ":" '{print $1,$NF}' passwd.list   #通过-F修改默认分隔符为冒号，输出第一行与最后一行；

#案例2：格式转换
awk '{print"@" $1"\n"$10"\n""+\n"$11""}' all.sam  #将短序列比对上的reads输出出来，生成fastq文件；

#案例3：过滤blast结果
awk '{if ($3>= && $4>=) print $0}'  blast_m8.out  #过滤blast比对结果，将identity 大于80，并且比对长度大于100bp的结果输出；

#案例4：比较
awk '$8>$1' input.txt #输出第8列大于第10列的行。

#案例5：匹配输出
awk '$0~ /wang/{print $0}' passwd.list   #利用正则表达式，将秘密表中姓wang的账户都输出出来；

#案例6：格式化输出
awk 'BEGIN{print "The Program Begin\n"}{if ($3>= && $4>=) print $0}END{print " The Program End\n"}' input.txt  #利用BEGIN和END关键字生成报告；

#案例7：修改字段和记录分隔符
awk 'BEGIN{OFS="\t"}{print $2,$4,$5}' input.txt   #在BEGIN中设定字段分隔符和记录分隔符；

#案例8：awk编程计算
awk '{x+=$3}END{print x/NR}' input.txt   #计算第三列的平均值，最后在END将其输出出来。

#案例9：awk编程比较大小
awk   'BEGIN { max= ;print "max=" max} {max=($1 >max ?$1:max); print $1,"Now max is "max}' input.txt  #取得文件最后一个域的最大值。 

#案例10：awk编程求和
awk '{print $0,$3+$4}' input.txt  #计算第3列和第4列的和。

#案例11：输出固定行内容
awk 'NR>=&&NR<=' input.txt  #输出第20到第80行内容。

#案例12：合并文件
awk 'BEGIN{while((getline<"file1")>)l[$1]=$0}$1 in l{print $0"\t"l[$1]}' file2  #将两个文件按列合并起来，类似jion命令的功能。

#案例13：去重复
awk '!($0 in a) {a[$0];print}' input.txt  # 打印不重复的行，类似uniq的功能;
awk '!($2 in l){print;l[$2]=}' input.txt #计算第二列内容非冗余的次数，类似于uniq的功能;

#案例14：统计字符
awk '{for(i=;i!=NF;++i)c[$i]++}END{for (x in c) print x,c[x]}' input.txt 计算每个字符出现的次数，类似wc的功能。

#案例15：替换
awk '{sub(/test/, "no", $0);print}' input.txt 进行替换，类似sed的功能，

#案例16：fastq转换为fasta
awk '{getline seq;getline plus;getline qual;sub("@",">",$0);print $0 "\n"seq}'  test.fastq；
```