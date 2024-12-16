---
title: '<X.Shirley Liu> chapter 1 - History of Bioinformatics'
author: J Song
date: '2024-12-16'
slug: []
categories:
  - 技术文章
tags:
  - Bioinformatics
  - Shirley Liu
description: 2024-12-16-chapter-2-history
keywords: 2024,12,16,chapter,2,history
lastmod: '2024-12-16T16:00:23+08:00'
---

>2021年哈佛大学刘小乐教授 Xiaole Shirley Liu 的Introduction to Computational Biology and Bioinformatics 课程笔记，[课程视频](https://www.bilibili.com/video/BV1yS4y1Z721?spm_id_from=333.788.player.switch&vd_source=92d8d2d891c2a36ace24559da5c9537c&p=3)

<!--more-->

## 1.1 The protein sequence and structure wave

- 1955：Sanger sequenced bovine insulin - 牛胰岛素蛋白测序
- 1970: Needleman-Wunsch algorithm - pairwise sequence alignment，用于对比两个序列并且评分量化他们的相似性
- 1973：PDB - protein data bank项目，存储了大量的蛋白序列，以及大量生物大分子（三维）结构
- 1990：BLAST - 找到数据库里所有与你的序列相似的序列（the hit sequence），基于序列相似性和意志的蛋白功能结构，可以推测新序列功能，是快速搜寻算法
- 1994-：CASP - 仅通过蛋白序列来预测结构，这是个比赛每两年举办一次，用新蛋白序列进行功能预测，等到蛋白质的功能结构之后，可以比较谁预测的最准
- 1994：BLOCKS database - 一个蛋白家族里面的蛋白质有结构功能相似的小型功能域，这个数据库用来建立储存这些功能域，Global distance test 来衡量真实结构和预测结构的差距
- 2018：AlphaFold - Google deepMind 开发了 AI 算法预测蛋白结构

## 1.2 The gene expression wave

- 1977: Northern blot 技术，测量单个细胞的基因表达
- 1995：Standford 开发了 Microarray 技术，用很小的探针进行检测，可以检测很多基因，方法：将正常细胞和癌症细胞取样，培养，提取 mRNA，然后逆转录为 cDNA，hyberdise cDNA probes to oligo sequences on microarray，再 scan 信号颜色区分是在哪个细胞内进行了表达
- ~2000：商业化、产业化，e.g. 区分急性淋巴细胞白血病（ALL）和急性骨髓细胞白血病（AML）两种 leukemia
- 2020：RASL-seq or Luminex assays：成本大幅度降低，profile the expression of ~1K gene at ~$5/sample; BROAD ConnectivityMap，1m profiles from perturbations of multiple cell types
- scRNA-seq：单细胞测序技术

## 1.3 The DNA sequencing wave

- 1953：DNA structure - DNA 双螺旋结构
- 1972：Recombinant DNA - 切下 DNA 一小段然后和其他 DNA 缝起来产生一个新 DNA
- 1977：Sanger sequencing
- 1985：PCR - 聚合酶链式反应，扩增基因片段
- 1988：NCBI 
- 1990：BLAST - 比对序列算法
- 1990-2003：NIH 启动了 human genome 项目

## 1.4 Sequencing development

- sequencing in 2001：Production - rooms of equipment，sample preparations 35 people 3~4weeks；Sequencing - 15-40 runs per day，1-2Mb per instrument per day，120Mb total capacity per day
- sequencing in 2007：Production - 1x Cluster Station, 1 people, 1 day；Sequencing - 1x Genome Analyzer, 1 run per 3~5 days, 0.5Gb per day per instrument
- sequencing Illumina
- big data challenge

## 1.5 Bioinformatics vs Computational biology

- Bioinformatics = the creation of tools(algorithms, databases) that solve problems. The goal is to build useful tools that work on biological data. It is about engineering.
- Computational biology = the study of biology using computational techniques. The goal is to learn new biology, knowledge about living systems. It is about discovery.
- Levels of Bioinfo / Comp Bio
  - Level 0: Modeling for modeling's sake
  - Level 1:(entry) Use published tools to analyze data and generate hypotheses for experimentalists
  - Level 2:(Bioinfo) Develop algorithms and databases for data analyses on new technologies, data integration and reuse
  - Level 3:(CompBio) Make biological discoveries from public data integration and modeling
  - Level X: Integrative studies from big consortia

## 1.6 Is this class for me?

- Computer: R and python
- Biology: molecular biology and genomics
- Statistics: hypothesis testing, distributions, intuition
- Major questions and major solutions in computational biology
- Hands on skills in computational biology and bioinformatics
- It is not for statistical theories or deductions

## 1.7 Course organization
- Module I: RNA-seq analysis and sample classification
  - RNA-seq, diff expr, FDR, clustering, GO, batch effect removal, dimension reduction, supervised learning
- Module II: Transcriptional and epigenetic gene regulation
  - Motifs, CHIP-seq, DNA methylation, histone marks, HMM, ATAC-seq, HiC
- Module III: GWAS interpretation and single-cell analysis
  - LD, GWAS catalog, eQTL, scRNA-seq, scATAC-seq
- Module IV: Cancer genomics and cancer immunology
  - TCGA, susvival analysis, drug screens, cancer immunology, CRISPR screens