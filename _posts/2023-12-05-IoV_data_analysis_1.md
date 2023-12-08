---
layout: post
title: "车联网数据分析（一）：数据采集及数据质量监控"
date: 2023-12-05
categories: Data_Science
excerpt: 车联网数据分析的数据采集及数据质量监控
published: true
---

最近两年从事一些车联网数据分析的工作，做一些总结。  

## 数据采集
{% include mermaid.html content="
flowchart LR
    subgraph 若需要
    E[(数据库)] -. 网络专线 .-> G[车企平台2]
    end
    subgraph 数据采集
    A[车企采集配置管理平台] -- 配置下发 -->  B[车载终端] -- 移动网络 --> C[车企平台] 
    D[整车传感器] -- CAN --> B
    C ----> E[(数据库)] 
    end
" %}  

## 常见问题
  

## 总结
  

## 版本记录
2023-12-05，初稿；  
