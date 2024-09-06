---
title: terminal learning notes
author: J Song
date: '2024-09-03'
slug: []
categories:
  - ~
description: 2024-09-03-terminal-learning-notes
keywords: 2024,09,03,terminal,learning,notes
lastmod: '2024-09-03T17:30:43+08:00'
---

Terminal 自学笔记

<!--more-->
> 参考 [Bioinfotec](https://blog.csdn.net/m0_56572447/article/details/131148134)、[遗落凡尘的萤火](https://blog.csdn.net/weixin_57975238/article/details/138159580?utm_medium=distribute.pc_relevant.none-task-blog-2~default~baidujs_baidulandingword~default-0-138159580-blog-131148134.235^v43^pc_blog_bottom_relevance_base6&spm=1001.2101.3001.4242.1&utm_relevant_index=1) 博文

## iTerm2 软件
- 换主题：下载 itermcolors 文件并在设置中导入


## Terminal shortcut

在 ~/.bashrc 文件中（没有就新建一个）添加行：`alias <shortcut>=“<full command>”`, 例如
```bash
# ALIASES #
###########
alias server3="ssh ...@..."
```

## Terminal 常用命令

### 编辑文件

`vi`：文件编辑（没有文件时会创建新文件），编辑完后按`Esc`退出编辑模式，然后用`:wq`保存并退出或用`:q!`不保存退出。

- 创建、删除目录 `mkdir`,`rmdir` 
```bash
## 在另一个目录下创建一个目录
mkdir -p parent/temp
## 删除目录及其内的文件
rmdir -rf temp
```

### 压缩和提取文件
```bash
# 从.tar 存档中提取文件
tar -xvf archive.tar
# 列出.tar存档中的文件（不提取）
tar -tf archive.tar
# 从 .tar.gz 存档中提取文件
tar -xvf archive.tar.gz
# 从 .tar.bz2 文件中提取文件
tar -xvf archive.tar.bz2
# 创建 tar 存档
tar -cvf archive.tar dir
tar -czvf archive.tar.gz dir
tar -cjvf archive.tar.bz2 dir
# 提取 .gz 压缩文件
gunzip file.gz
# 创建 .gz 压缩文件
gzip file.fastq
# 提取 .bzip2 压缩文件
bunzip2 file.bz2
# 创建 .bzip2 压缩文件
bzip2 file.fastq
# 提取 .zip 存档
unzip archive.zip
# 创建 .zip存档
zip archive.zip file.txt
#创建 .zip存档（对文件夹）
zip -r archive.zip dir
```

### 查看文件

```bash
## 查看 file.txt 文件前 20 行
head -n 20 file.txt 
## 展示 file.txt 文件中的前 10 行，并打印出所有行号
cat -n file.txt | head -10
## 逐页查看，按空格翻页，按回车换行，按 q 退出
more file.txt
## less 上下左右键查看文本内容，回车向下移动一行，空格翻页，q 退出，-N 显示行号，-S 单行显示，可以查看压缩文件（less 或 zless）
less file.txt | head -1
## 统计文本 wc -l 统计行数 -w 统计字符串数 -c 统计字节数（包括不可见字符，如回车）
wc file.txt # 依次为行数、字符串数、字节数
```

## 字符替换
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
## 合并 file1 和 file2 的内容
cat file1.txt file2.txt > merged_file.txt
## 追加 file3 的内容到 merged_file.txt
cat file3.txt >> merged_file.txt
## 将file1和file2左右合在一起,paste在合并文件时,中间是加tab键作为分隔符
paste file1.txt file2.txt
##  -d指定':'为分隔符
paste -d ': ' file1 file2  
## 将file1和file2左右合在一起,paste在合并文件时,并转秩
paste file1 file2  -s     

## paste 还可以合并字符
## 将 1-20 数字用paste排序(每个 - 之间都有一个空格)
seq 20 | paste - - - -      
## 提取第 3 列，排序去重，取出数字相加计算求和
cat Data/example.gtf | cut -f 3 | sort | uniq -c | tr -s ' ' | cut -d ' ' -f 2 | paste -s -d '+' | bc
## bc 的作用可以计算“1+1”这种形式
echo "1+1" | bc 
##先用paste变成一个整体,再排序
less Data/example.fq | paste - - - - | sort -k 2 -r | less -SN  
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

sort：排序

常见参数：

-n：按照数值从小到大进行排序

-V：字符串中含有数值时，按照数值从小到大排序

-r：逆向排序

-k：指定按哪一列排序

-t：指定分隔
```bash
less -S Data/example.gtf | sort -k 3 | less -SN  ##less查看example.gtf,按照第三列排序,用less查看
cat Data/example.gtf | sort -k 3 | less -SN       ##cat查看example.gtf,按照第三列排序,用less查看,当文件比较大时用cat打开的比less快,小文件无差别
 cat Data/example.gtf | sort -k 4 -n | less -SN   ##cat查看example.gtf,按照第4列排序,按照数值来理解,less查看
cat Data/example.gtf | sort -k 4 -n -r | less -SN  ##cat查看example.gtf,按照第4列排序,按照数值来理解,逆向排序,less查看
```
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

### 常用 linux 命令
![linux_command.jpeg](/imgs/linux_command.jpeg)
