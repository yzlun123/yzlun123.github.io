---
layout: post
title: "数据分析踩坑（一）：sql数据关联及聚合原则"
date: 2024-01-19
categories: Data_Science
excerpt: 在对两表进行关联及聚合运算时，经常出现结果不一致的情况，对原因进行了分析。
published: true
---

在对两表进行关联及聚合运算时，经常出现结果不一致的情况，对原因进行了分析，发现问题出在了先关联再聚合，还是先聚合再关联上，基于sqlite进行了测试。  

结论为当涉及两表关联聚合计算时，应填充空值、分别聚合计算至没有结果表外的维度后，再进行结果表的关联和计算。

## 数据准备
table t1:  

| month | dimension1 | metrics1 | 
| :---: | :---: | :---: | 
| 202305 | a | 4752 | 
| 202305 | b | 6659 | 
| 202305 | c | 325 | 

table t2:  

| month | dimension1 | dimension2 | metrics2 | 
| :---: | :---: | :---: | :---:| 
| 202305 | b | p1 | 239 | 
| 202305 | a | p1 | 51 | 
| 202305 | b | p2 | 3 | 
| 202305 | c | p3 | 9 | 
| 202305 | b | p3 | 176 | 
| 202305 | a | p3 | 25 | 

目标：  
在month、dimension2维度下，对metrics1、metrics2求和，并求sum(metrics2)/sum(metrics1)。

## 先关联再聚合
``` sql
select 
    t1.month, 
    t2.dimension2, 
    sum(t1.metrics1) metrics1, 
    sum(t2.metrics2) metrics2, 
    round(cast(sum(t2.metrics2) as real)/cast(sum(t1.metrics1) as real) * 1000000, 0) pct
from t1 join t2
    on t1.month = t2.month
    and t1.dimension1 = t2.dimension1
group by t1.month, t2.dimension2;
```

输出结果：  

| month | dimension2 | metrics1 | metrics2 | pct |  
| :---: | :---: | :---: | :---: | :---: |  
| 202305 | p1 | 11411 | 290 | 25414.0 |  
| 202305 | p2 | 6659 | 3 | 451.0 |  
| 202305 | p3 | 11736 | 210 | 17894.0 |  

## 先聚合再关联
```sql
with a as (
select 
	month, 
	sum(metrics1) metrics1
from t1
group by month
),
b as (
select 
	month, 
	dimension2, 
	sum(metrics2) metrics2
from t2
group by month, dimension2
)

select 
	b.month, 
	b.dimension2, 
	a.metrics1, 
	b.metrics2,
	round(cast(b.metrics2 as real)/cast(a.metrics1 as real) * 1000000, 0) pct
from a join b
	on a.month = b.month;
```

输出结果：  

| month | dimension2 | metrics1 | metrics2 | pct | 
| :---: | :---: | :---: | :---: | :---: | 
| 202305 | p1 | 11736 | 290 | 24710.0 | 
| 202305 | p2 | 11736 | 3 | 256.0 | 
| 202305 | p3 | 11736 | 210 | 17894.0 | 

## 分析和总结
dimension1维度用不到，先关联再聚合时，t2表中并不是每个dimension2维度都有对应的dimension1维度，导致关联获取的数据不全，影响最终结果，先聚合再关联避免了这个问题。

## 版本记录
2024-01-19，初稿。  
