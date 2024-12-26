---
title: <X.Shirley Liu> chapter 3 - RNA-seq
author: J Song
date: '2024-12-17'
slug: []
categories:
  - 技术文章
tags:
  - Shirley Liu
  - Bioinformatics
description: 2024-12-17-chapter-3
keywords: 2024,12,17,chapter,3
lastmod: '2024-12-17T13:28:45+08:00'
---

>2021年哈佛大学刘小乐教授 Xiaole Shirley Liu 的Introduction to Computational Biology and Bioinformatics 课程笔记，[课程视频](https://www.bilibili.com/video/BV1yS4y1Z721?spm_id_from=333.788.player.switch&vd_source=92d8d2d891c2a36ace24559da5c9537c&p=3)

<!--more-->

## 3.1 applications

- RNA-seq protocol
  - 收集细胞中所有的 RNA，移除混杂的 DNA，还可以进一步剔除含量特别高的 rRNA 或 tRNA，或筛选出更能代表细胞类型特征的 mRNA
  - 把长 RNA 打断成小片段（几百 bp），用不同的引物进行反转录，获得 cDNA（双链 DNA）
  - PCR 扩增，选择具有相似长度的片段，放入测序仪测序，测到 cDNA 的末端，双端测序
- application
  - 检测特定条件下所有基因的表达情况（特定条件包括发育阶段，不同组织，正常或患病，药物治疗，基因微扰等）
  - 发现新的基因或转录本
  - model alternative splicing（同一个基因可能在表达时，只使用了部分外显子，而非全部）
  - find gene mutations or gene fusions
  - do not have to know the genome sequence or predict genes
  - digital representation of gene expression
  - good detection range(>10^5) from low to high expression level
- RNA quality control
  - degraded RNA are hard to be made into a sequencing library
  - check RNA quality: index - DV200: % RNA > 200 nt(因为大多数基因长度超过 200), DV200 > 30% is recommended
- experimental design
  - Ribo-minus(remove too abundant t/rRNA transcripts)
  - polyA(mRNA after splicing, enrich for exons, no tRNA/rRNA, most popular)
  - strand specific(directionality of RNA, useful for nocel IncRNA(long non-coding RNA))
  - sequencing:
    - SE or PE: PE getting more popular 
    - Depth: 20-50M for differential expression, deeper for transcript assembly or splicing
    - Read length:longer for transcript assembly, splicing, or mutation calls
  - assessing biological variation requires replicates:
    - technical(rarely used):same RNA, library prep and sequencing separately
    - Bio1: Cells grown in different dishes, processed on different days
    - Bio2: Cells/tissues from different lab animals(genetically identical, similar age and lifestyle)
    - Bio3: Cells/tissues from different human individuals
  - How many replicates are good enough:
    - 1 only for exploratory assays, not good for publications
    - >= 2 OK for cell line samples
    - >= 3 preferred for animal samples
    - more for human samples
    
## 3.2 Alignment

- BWA?: for DNA sequence
- prefer splice-aware aligners
- TopHat(BW),HISAT(BW),STAR(Suffix Array)...

- Splice aware alignment (TopHat)
  - need genome index file
  - transcript annotation file is optional but not required
  - map to exons first
  - create junction database
  - map unmapped reads to junctions
  - for longer reads, map shorter segments (~25bp)
- BAM file for PE RNA-seq alignemnt from STAR
  - col1: read id
  - col2: binary encoding flags, e.g. first bit indicate PE, so all PE samples have odd number
  - col3,4: chromosome and the beginning location of the read
  - col6: cigar string: '27M1099N48M' means read matches 27bp at first, spans a 1099bp intron, then maps 48bp on the next exon
  - col7,8,9: the location of its mate and how long the two mates spanned on the reference genome
  - optional fields:
    - NH: how many places this read is aligned to;
    - nM: the number of mismatches for the pair;
    - XS: optional field, strand of the underlying transcript generating the read for HISAT

## 3.3 RNA-seq QC (RSeQC): FASTQC Read Quality

- overal mappability of reads, ideally > 50%, higher the better
- could trim first few bases of every read
- insert size and read distributions
- TIN and medTIN
  - TIN(transcript integrity number) on each transcript
  - medTIN(median TIN score across all the transcripts) to measure the RNA integrity at sample level (>50%)
  
## 3.4 Expression index

- RPKM vs FPKM
  - RPKM = reads per kilobase million
    - for single end RNA-seq
    - normalized for differences in sequencing depth; normalized for gene size
  - FPKM = fragments per kilobase million
    - for paired end RNA-seq
  - TPM
    - normalized for gene length; normalized for sequencing depth
    
- RPKM (Reads per kilobase of transcript per million reads of library)
  - Total reads/1M, divide by gene length in KB
  - corrects for coverage, gene length
  - TopHat / Cufflinks
  - FPKM (Fragments), PE libraries, ~ RPKM/2
- TPM (transcripts per million) RSEM (Li et al, Bioinfo 2011)
  - Divide read count by gene length in KB (RPK) FIRST, divide by scaling factor (sum of RPK across all genes/1M)
  - proportion of reads mapped to a gene in each sample is comparable
- CPM (count per million)

- RSEM for quantification
  - Input: FASTQ or BAM files, reference transcript annotation file
  - Output: transcript-level gene expression (read count, TPM, FPKM) calculated on effective transcript length
  - Effective length: given the sequence composition of these transcripts, you'd expect a priori to sample more reads from them $$\tilde{l}_i=l_i-\mu +1$$
    $l_i$ is the length of transcript, $\mu$ is the average fragment length
- Isoform Inference
  - geven known set of isoforms
  - estimate x to maximize the likelihood of observing n

- pseudoalignment
  - RSEM is considered the best quantification approach
  - Kallisto (Bray et al, Nat Biotech 2016)
  - Salmon (Patro et al, Nat Meth 2017)
  - Do not provide "full" alignment (i.e. no exact base-by-base alignment)
  - Need reference transcript annotation files
  - Find all transcripts (and positions) that a read is compatible with
  - Salmon also corrects for sequence-specific and GC biases
  - Can run either from FASTQ or BAM files
  - Can map 10M reads in a few min, trade off between speed and accuracy

