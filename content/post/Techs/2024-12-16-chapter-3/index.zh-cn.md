---
title: '<X. Shirley Liu> chapter 2 High Throughput Sequencing and Read Mapping'
author: J Song
date: '2024-12-16'
slug: []
categories:
  - 技术文章
tags:
  - Bioinformatics
  - Shirley Liu
description: 2024-12-16-chapter-3
keywords: 2024,12,16,chapter,3
lastmod: '2024-12-16T17:21:44+08:00'
---

>2021年哈佛大学刘小乐教授 Xiaole Shirley Liu 的Introduction to Computational Biology and Bioinformatics 课程笔记，[课程视频](https://www.bilibili.com/video/BV1yS4y1Z721?spm_id_from=333.788.player.switch&vd_source=92d8d2d891c2a36ace24559da5c9537c&p=3)

<!--more-->

## 2.1 Sequencing techologies

- 1st Generation: Sanger sequencing
  - add single-stranded DNA sequence to four test tubes
  - each tube contain all dNTPs + one ddNTP: 例如 tube1 中添加 ddATP，tube2 中添加 ddCTP，and so on，结合 ddNTP 的会停止进一步结合，然后通过看每个 strand 的长度来确认对应的碱基
  
- 2nd Generation: Illumina sequencing cluster generation
  - DNA 序列打成片段，PCR 桥式扩增，然后进行边合成边测序。合成过程是提供带有不同荧光 dNTP ，结合上的引物会阻止延伸，然后洗掉多余的 dNTP，激光扫描收留荧光信号，用试剂去除阻断的基团和荧光基团，在进行下一个碱基的测序反应。通常长度是 150bp，可以同时进行大量 150bp 的反应

- 3rd Generation: single molecule sequencing 读长更长，可达 10kb

## 2.2 Fastq and FASTQC

- FASTQ file
  - format：sequence ID， sequence，quality ID，quality score
  - Phred quality：ASCII of：sequence quality + 33， -10log10 Pr(bp is wrongly sequenced), the bigger the number the better the quality

- Quality control
  - quality is not good w.r.t read(rerun), sample(resample), run(just ignore one spot)
  - common tool: FASTQC
    - per base sequence quality
    - per sequence quality distribution(30 就很好了)
    - nucleotide content per position
    - per sequence gc content ( human genome around 45 )
    
## 2.3 mapping algorithms

- Sequence mapping algorithms
  - Seed:k-mers index table，BLAST algorithm
  - Suffix array
    - Suffix tree:A tree of all the suffixes of the reference; O(n) time to build,n=genome length; O(m) time to search,m=query length; genome index is large, 50G; tools like MUMmer  
    - Suffix array:ith entry conrresponds to the ith smallest suffix; tools like STAR(for RNA sequencing);O(n) time to build,n=genome length; O(mlogn) time to search,m=query length; genome index is large, 15G
    - Borrows-Wheeler transformation & LF mapping: tools like bwa and bowtie
      - burrows-wheeler transformation: reversible permutation used originally in compression
        - build matrix by put the last element of a sequence to the beginning
        - rank rows by the very beginning element
        - only record the last column to get BWT(T) result: characters will tend to cluster together(we could recover original sequence by using LF mapping,like 最后一列第 i 个 a，就是第一列第 i个 a)
    - BW Alignment
      - top & bot delimit the range of rows beginning with progressively longer suffixes of Q(from right to left)
      - if range becomes empty the query suffix does not occur in the text
      - if no match ,instead of giving up, try to "backtrack" to a previous position and try a different base(mismatch, much slower)
      - steps: 1. use BWT to store entire reference geome as a lookup index;2. align tag base by base from the end;3.all active locations are reported;4. if no match is found, then bach up and try a substitution

## 2.4 SAM abd BED

- Alignment output: SAM and BED
  - SAM file
    - @HD - header line
    - @SQ - reference genome information
    - @RG - read group information
    - @PG - program (software) information
    - mapping result
      - read name
      - map: 0 OK, 4 unmapped, 16 mapped reverse strand
      - sequence, quality score, XA (mapper-specific)
      - MD: mismatch infor: 3 match, then C ref, 30 match, then T ref, 3 match
      - NM: number of mismatch
    - BAM: binary compressed SAM format
    - visulize BAM / BED files in genome browsers(UCSC or IGV)
  - BED and BigBED files
    - rarely used to store alignment information, only store positions