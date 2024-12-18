---
title: <X. Shirley Liu> chapter 3 RNA-seq
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
  - model alternative splicing
  - find gene mutations or gene fusions
  - do not have to know the genome sequence or predict genes
  - digital representation of gene expression
  - good detection range(>10^5) from low to high expression level