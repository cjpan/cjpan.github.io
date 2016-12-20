---
title: 用Pandas操作Excel文件
date: 2016-12-20 20:16:36
tags: [pandas, excel, python]
categories: [tips]
---
最近工作中在做字库，源是英文，翻译成中日韩文，然后做成字库做到设备里去，比较无聊的事情。
翻译的部门提供了译文的Excel文件，其中列出了屏幕上显示的消息字符串的译文以及每个消息的字体的大小，
比如:

|Id |  Messages  |  Font Size   |
|:-----:|:------:|:-----------:|
|1  | 查找设备      | 14Reg|
|1  | 装置を検索しています | 14Reg|
|2  | 未找到设备 | 16Reg|
|2  | 装置が見つかりません  | 16Reg|
|3  | 早上好 | 14Reg|
|3  | おはようございます  | 14Reg|
|4  | 程序 | 14Reg|
|4  | プログラム  | 14Reg|

因为嵌入式设备的资源限制，要从通用字库中筛选出只在消息字符串中用到的字加入设备的字库文件，Font大小不同的字需要加入不同大小的字库文件。所用到的字库工具要求输入一个筛选文件，其中包含的是所需的字，以Unicode的形式顺序储存，如`0x1234`。所以一个工作就是从Excel文件中遍历所有字符串，把字符串里的不同的字挑出来做成一个列表转成unicode写到筛选文件里去。

本着“超过15分钟的重复劳动都要自动化！”的精神，于是写了个简单的Python程序来做这个事情。逻辑本身很不难，但从Excel中读取数据的部分对我来说是新的。正好手头机器上已经装了pandas，就直接在pandas上做了，在这里记录总结一下。
(pandas是Python的一个数据处理包，[主页](http://pandas.pydata.org/)）

## 读入Excel文件
导入pandas模块：
```
import pandas as pd
```
读入待处理的excel文件，存到data的变量中：
```
data = pd.read_excel('foo.xls')
```
默认读入的是第一个sheet，如果要选择sheet的话，可以：
```
data = pd.read_excel('foo.xls', sheetname=1) # 2nd sheet
```
或者
```
data = pd.read_excel('foo.xls', sheetname='Sheet1') # 'Sheet1' sheet
```
又或者
```
data = pd.read_excel('foo.xls', sheetname=[0, 1, 'Sheet5'] ) # Multi sheets
```
读入单个sheet后的data的类型是pandas的DataFrame类型，简单来讲DataFrame是一个二维表结构，包含了行列的索引。
如果读入的是多个sheet，则pandas是值为DataFrame类型的一个dictionary，这个应该可以理解。
read_excel()也可以用来读取csv文件。csv文件也可以用read_csv()来做。

## 操作Excel数据
显示列的名字：
```
data.columns
```
读取某一列：
```
data['Id']
```
读取某几列：
```
data[['Messages', 'Font Size']]
```
条件筛选：
```
data[data['Font Size'] == '14Reg']
```
把字体大小是14Reg的消息都读出来：
```
msgs_reg14 = data[data['Font Size'] == '14Reg']['Messages']
```
或者，把消息中字体大小是14Reg的选出来也是可以的：
```
msgs_reg14 = data['Messages'][data['Font Size'] == '14Reg']
```

之后的遍历消息字符串的逻辑很普通，额外的就是一些细节的处理，就不提了。

## 写入Excel文件
这次工作中不需要修改再保存Excel文件，但是也研究了一下：
```
out = pd.ExcelWriter('output.xls')
new_df.to_excel(out)
out.save()
```

## 一般示例
```
import pandas as pd

# read sheet in
data = pd.read_excel('foo.xls', sheetname=1)
# view table types
print data.dtypes
# view a column
print data['A']

'''
# create a new column and calculate values
for i in data.index:
    data['C'][i] = data['A'][i] + data['B'][i]
'''

# write Excel file
out = pd.ExcelWriter('output.xls')
new_df.to_excel(out)
out.save()
```
## 更多参考
[pandas官方文档](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.read_excel.html)
