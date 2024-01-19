---
layout: post
title: "pandas agg方法浅谈"
date: 2023-12-24
categories: Data_Science
excerpt: 数据分析使用pandas，在聚合groupby的情况下，经常使用agg方法来计算多个统计值，如极值、平均值、分位值等，在计算对象是单列、多列条件下，如何使用。
published: true
---

数据分析使用pandas，在聚合groupby的情况下，经常使用agg方法来计算多个统计值，如极值、平均值、分位值等，在计算对象是单列、多列条件下，如何使用。  

## 数据准备
使用鸢尾花数据，转换为pandas dataframe使用。

``` python
# import libraries
from sklearn import datasets
import pandas as pd

# load data
iris = datasets.load_iris()
df0 = pd.DataFrame(iris.data, columns=iris.feature_names)
df1 = pd.DataFrame(iris.target, columns=['target'])
df = df0.merge(df1, left_index=True, right_index=True)
```

## 对单列数据聚合处理
```python
# only one columns
result_df = df.groupby(['target'], as_index=False)['sepal length (cm)'].agg(
    Mean = 'mean',
    Std = 'std',
    Min = 'min',
    TenQuantile= lambda x: x.quantile(0.1)
)

# another way
result_df = df.groupby(['target'], as_index=False)['sepal length (cm)'].agg(
    {
        'Mean': 'mean',
        'Std': 'std',
        'Min': 'min',
        '10% Quantile': lambda x: x.quantile(0.1),
    }
)

print(result_df)
```

## 对多列数据聚合处理
```python
# multiple columns
result_df = df.groupby(['target'], as_index=False).agg(
    sl_mean = ('sepal length (cm)', 'mean'), 
    sl_std = ('sepal length (cm)', 'std'), 
    sl_min = ('sepal length (cm)', 'min'), 
    sl_tenquantile = ('sepal length (cm)', lambda x: x.quantile(0.1)),
    sw_mean = ('sepal width (cm)', 'mean'), 
    sw_std = ('sepal width (cm)', 'std'), 
    sw_min = ('sepal width (cm)', 'min'), 
    sw_tenquantile = ('sepal width (cm)', lambda x: x.quantile(0.1)),
)

# another way
cols = ['sepal length (cm)', 'sepal width (cm)']
result_df = df.groupby(['target'], as_index=False).agg(
    {
        'sepal length (cm)': [('Mean', 'mean'), ('Std', 'std'), ('Min', 'min'), ('10% Quantile', lambda x: x.quantile(0.1))],
        'sepal width (cm)': [('Mean', 'mean'), ('Std', 'std'), ('Min', 'min'), ('10% Quantile', lambda x: x.quantile(0.1))],
    }
)

print(result_df)
```

## 总结及存在问题
如果不聚合直接计算统计值，可以借助构建dictionary再转换为dataframe，方法如下：    
```python
# build a dictionary
result_dict = {
    'Mean': df[cols].mean(),
    'Std': df[cols].std(),
    'Min': df[cols].min(),
    '10% Quantile': df[cols].quantile(0.1),
}

# convert to dataframe
result_df = pd.DataFrame(result_dict)

print(result_df)
```

如果聚合的话，这种方法会报错，还没能解决。  

## 版本记录
2023-12-24，初稿  
2024-01-19，增加另外一种方式，更简明
