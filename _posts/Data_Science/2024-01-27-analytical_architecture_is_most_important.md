---
layout: post
title: "数据分析踩坑（二）：架构设计放在第一位"
date: 2024-01-19
categories: Data_Science
excerpt: 
published: false
---

在对两表进行关联及聚合运算时，经常出现结果不一致的情况，对原因进行了分析，发现问题出在了先关联再聚合，还是先聚合再关联上，基于sqlite进行了测试。  

结论为当涉及两表关联聚合计算时，应填充空值、分别聚合计算至没有结果表外的维度后，再进行结果表的关联和计算。

## 数据准备
{% include mermaid.html content="

" %}



## 分析和总结


## 版本记录
2024-01-19，初稿。  
