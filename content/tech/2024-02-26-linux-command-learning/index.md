---
title: linux 命令学习
author: Package Build
date: '2024-02-26'
slug: []
categories: []
tags: []
---
新建xueke.txt的文件
```bash
vi xueke.txt
touch xueke.txt
```

去重 uniq

文件的压缩和解压
```bash
#tar格式解压
tar -xf xueke.tar
#打包成tar格式
tar -cf xueke.tar *.fa
#gz格式解压
gunzip xueke.fa.gz
#压缩成gz格式
gzip xueke.fa
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
```
#将当前目录及其子目录下所有文件后缀为 .c 的文件列出来:
find . -name "*.c"
#搜索 etc 目录下所有以 sh 开头的文件
locate /etc/sh
#忽略大小写搜索当前用户目录下所有以 r 开头的文件
locate -i ~/r
# fastp文件的绝对路径					
which fastp
```

grep

sed

awk