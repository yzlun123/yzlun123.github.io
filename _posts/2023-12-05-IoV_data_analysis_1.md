---
layout: post
title: "车联网数据分析（一）：数据准备及分析流程"
date: 2023-12-05
categories: Data_Science
excerpt: 车联网数据分析的准备工作以及分析流程
published: true
---

最近两年从事一些车联网数据分析的工作，做一些总结。  

## 数据准备
数据采集数据流  
```mermaid
graph LR
    A[接入] --> B[清洗及分析建模] --> C[可视化开发] --> D[管理封闭]
```  

## 分析流程
1. 创建domin.github.io的仓库；
2. 用GitHub Codespaces打开仓库，使用GitHub Copilot，询问Jekyll + GitHub Pages部署步骤及代码；
3. bundle exec jekyll serve，发布！  

## 总结
GitHub Copilot这类大语言模型的出现，极大降低了新手的门槛，带来了新的学习模式的改变。  

## 版本记录
2023-12-05，初稿；  
