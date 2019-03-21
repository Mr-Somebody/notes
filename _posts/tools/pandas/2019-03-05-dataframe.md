---
title: Pandas DataFrame Operations
category:
  - tools
  - pandas
comments: true
excerpt_separator: <!--more-->
layout: post
tags: [DataFrame, slice, chunk, exception]
---
Experience towards how to manage pandas DataFrames

Topics include:
1. Slice big DataFrame into chunks.([Refer](https://stackoverflow.com/questions/44729727/pandas-slice-large-dataframe-in-chunks))
2. Exception handling when using pandas apply.([Refer](https://stackoverflow.com/questions/48838562/exception-handling-when-using-pandas-apply))
<!--more-->

## Slice big DataFrame into chunks
```python
# Slice by chunk size
n = 200000  #chunk row size
list_df = [df[i:i+n] for i in range(0,df.shape[0],n)]

# Group by name
list_df = []
for n, g in df.groupby('AcctName'):
    list_df.append(g)
```

## Exception handling when using pandas apply
When you are trying to apply a function to pandas dataframe, exeptions may happen. For example, you are trying to convert columns with NaN values into int. In that case, you need to apply customized function to dataframe, instead of using predefined functions. Example:
```python
# May cause exceptions with NaN values
df.apply(int)
# Handle exceptions nicely with self-defined function
def toint(value):
    try:
        return int(value)
    except Exception:
        return -1
df.apply(toint)
```
