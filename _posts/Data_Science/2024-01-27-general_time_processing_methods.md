---
layout: post
title: "编程学习（二）：sql及Python常用时间处理函数汇总"
date: 2024-01-27
categories: Data_Science
excerpt: 在sql及Python数据分析过程中，经常需要对时间进行转换和处理，对此进行了总结。
published: true
---

在sql及Python数据分析过程中，经常需要对时间进行转换和处理，对此进行了总结。

## sql
### implala
{% include mermaid.html content="
flowchart TD
a[string]
b[timestamp]
c[unix_timestamp]

a -- to_timestamp(string dt, string pattern) --> b
b -- from_timestamp(timestamp t, string pattern) --> a
a -- unix_timestamp(string dt, string pattern) --> c
c -- from_unixtime(bigint unixtime, string pattern) --> a

b -- unix_timestamp(timestamp t) --> c
c -- to_timestamp(bigint unixtime) --> b
" %}

### hive
取消timestamp节点后，可复用impala架构。  

### oracle
{% include mermaid.html content="
flowchart TD
a[string]
b[timestamp]

a -- to_date(string dt, string pattern) --> b
b -- to_char(timestamp t, string pattern) --> a
" %}

## Python
### 基于datetime包

### 基于pandas包

## 分析和总结
可视化后，基本都是一些范式。

## 版本记录
2024-01-27，初稿。  
