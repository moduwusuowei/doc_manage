Tablib是一个用于处理电子表格（如 Excel，CSV，JSON）的Python 库。它提供了一种简单而强大的方式来操作和处理数据。利用Tablib，我们可以轻松地读取、写入、过滤和转换各种类型的电子表格数据。Tablib 具有一致且易于使用的 API，以在不同的数据格式之间进行无缝转换。比如，Tablib可以将数据从Excel表格导入为Python对象，然后将其转换为JSON或CSV格式，并进行相应的操作和分析。此外Tablib还支持对数据进行排序、筛选和合并等常见操作。Tablib官方仓库地址为：[tablib](https://github.com/jazzband/tablib)，Tablib官方文档地址为：[tablib-doc](https://tablib.readthedocs.io/en/stable/)。

Tablib需要在Python3.6+版本下安装，安装命令如下：

> pip install tablib

```python
import tablib
# 查看版本
tablib.__version__
'3.4.0'
```



目录

- 1 Tablib使用
  - [1.1 表格创建](https://www.cnblogs.com/luohenyueji/p/17867000.html#11-表格创建)
  - [1.2 数据导入与导出](https://www.cnblogs.com/luohenyueji/p/17867000.html#12-数据导入与导出)
  - [1.3 多表管理](https://www.cnblogs.com/luohenyueji/p/17867000.html#13-多表管理)
  - [1.4 进阶使用](https://www.cnblogs.com/luohenyueji/p/17867000.html#14-进阶使用)
- [2 参考](https://www.cnblogs.com/luohenyueji/p/17867000.html#2-参考)



# 1 Tablib使用

## 1.1 表格创建

**创建数据结构**

```python
# 创建表
data = tablib.Dataset()
type(data)
tablib.core.Dataset
# 查看表格名字，默认为空
data.title 
# 设置表的名字
data.title = 'data'
data.title
'data'
```

**数据添加**

```python
# 添加行
data.append(['John', 28])
data.append(['Tom', 16])
data.append(['Jane', 32])
# 查看数据,list格式
data.dict
# 或者
# print(data)
[['John', 28], ['Tom', 16], ['Jane', 32]]
# 添加标题行
data.headers = ['Name', 'Age']
# data.dict
print(data)
Name|Age
----|---
John|28 
Tom |16 
Jane|32 
# 添加列
# 需要和当前行数一致
data.append_col(['USA', 'UK','UK'], header='Country')
# data.dict
print(data)
Name|Age|Country
----|---|-------
John|28 |USA    
Tom |16 |UK     
Jane|32 |UK     
```

**选择行或列**

```python
# 选择第一行
data[0]
('John', 28, 'USA')
# 选择第一行第三列
data[0][2]
'USA'
# 选择列
data['Age']
[28, 16, 32]
# 获得表头
data.headers
['Name', 'Age', 'Country']
# 基于索引获得列
data.get_col(0)
['John', 'Tom', 'Jane']
```

**删除行或列**

```python
# 删除列
del data['Country']
# 删除行
# del data[:-1]
print(data)
Name|Age
----|---
John|28 
Tom |16 
Jane|32 
```

**行列高级操作**

```python
# 表格转置
transposed_data = data.transpose()
print(transposed_data)
Name|John|Tom|Jane
----|----|---|----
Age |28  |16 |32  
# 读取数据维度
data.width,data.height
(2, 3)
# 按照字段排序
# 年龄从大到小排序
data = data.sort("Age",reverse=True)
print(data)
Name|Age
----|---
Jane|32 
John|28 
Tom |16 
# 计算平均年龄
ages = data['Age']
float(sum(ages)) / len(ages)
25.333333333333332
# 移除第一行
tmp = data.lpop()
print(data)
Name|Age
----|---
John|28 
Tom |16 
# 第一行添加数据
data.lpush(list(tmp))
print(data)
Name|Age
----|---
Jane|32 
John|28 
Tom |16 
# 在最左侧插入一列数据
new_column = ['Engineer', 'Doctor','Doctor']
data.lpush_col(new_column, header='Profession')
print(data)
Profession|Name|Age
----------|----|---
Engineer  |Jane|32 
Doctor    |John|28 
Doctor    |Tom |16 
# 移除最后一行
# data.pop()
# print(data)
# 移除重复行
# 创建数据集
data = tablib.Dataset()
data.headers = ['Name', 'Age']
data.append(['Alice', 25])
data.append(['Alice', 30])
data.append(['Alice', 25])  # 重复行
 
# 去除重复行，必须所有列值一样
data.remove_duplicates()
 
print(data)
Name |Age
-----|---
Alice|25 
Alice|30 
```

**表格合并**

```python
# 创建两个表格
data1 = tablib.Dataset()
data1.headers = ['Name', 'Age']
data1.append(['Alice', 25])
data1.append(['Bob', 30])
 
data2 = tablib.Dataset()
data2.headers = ['Name', 'Occupation']
data2.append(['Alice', 'Engineer'])
data2.append(['Bob', 'Doctor'])
# 按行合并
# 使用stack方法合并两个表格
stacked_data = data1.stack(data2)
print(stacked_data)
Name |Age     
-----|--------
Alice|25      
Bob  |30      
Alice|Engineer
Bob  |Doctor  
# 按列合并
# 两个表格行数需要一致
# 使用stack_cols方法合并两个表格的列
stacked_cols_data = data1.stack_cols(data2)
print(stacked_cols_data)
Name |Age|Name |Occupation
-----|---|-----|----------
Alice|25 |Alice|Engineer  
Bob  |30 |Bob  |Doctor    
```

## 1.2 数据导入与导出

**数据导出**

Tablib使得用户可以根据具体需求将数据灵活地导出到不同的环境中，并与其他工具进行无缝集成和交互。**转换的结果是这些格式的对象表示而不是存为本地文件**。这些格式包括但不限于：

- CSV：常见的电子表格格式，每个字段由逗号分隔。
- JSON：一种常见的数据交换格式，以键值对的形式存储数据。
- Excel：电子表格格式，需要额外安装库，可以包含多个工作表，并支持公式和图表等功能。
- YAML：一种易读的数据序列化格式，常用于配置文件。
- HTML：用于创建网页的标记语言。
- Pandas DataFrame：Pandas是另一个Python库，用于数据处理和分析。Tablib支持将数据导出为Pandas DataFrame。

Tablib提供了两种方式将数据导出为其他格式，一种是调用export函数，一种是调用自带属性。如下所示data.export('csv') 和data.csv都可以用于获取Dataset数据的CSV表示：

```haskell
data.export('csv')
data.csv
```

具体示例代码如下：

```python
# 创建表格
data = tablib.Dataset()
data.headers = ['Name', 'Age']
data.append(['John', 28])
data.append(['Tom', 16])
data.append(['Jane', 32])
# 导出为csv字符流
data_csv = data.export('csv')
print(type(data_csv))
 
# 导出数据到本地csv文件
with open('data.csv', 'w') as f:
    f.write(data_csv)
<class 'str'>
# 导出为json字符串
data_json = data.export('json')
type(data_json)
 
# 将json字符串解析为Python对象
import json
data_json = json.loads(data_json)
print(data_json)
[{'Name': 'John', 'Age': 28}, {'Name': 'Tom', 'Age': 16}, {'Name': 'Jane', 'Age': 32}]
# 将数据集对象保存为json文件
with open('data.json', 'w') as f:
    f.write(data.export('json'))
# 保存为yaml文件
with open('data.yml', 'w') as f:
    f.write(data.export('yaml'))
# 将数据集保存为xls文件，注意使用wb模式
# 需要安装额外库
# pip install xlrd
# pip install xlwt
with open('data.xls', 'wb') as f:
    f.write(data.export('xls'))
 
with open('data.xlsx', 'wb') as f:
    f.write(data.export('xlsx'))
# 转换为html
# 需要安装MarkupPy库
# pip install MarkupPy 
with open('data.html', 'w') as f:
    f.write(data.export('html'))
# 转换为pandas的dataframe
# 需要安装Pandas库
df = data.export('df')
df.head()
```

|      | Name |  Age |
| ---: | ---: | ---: |
|    0 | John |   28 |
|    1 |  Tom |   16 |
|    2 | Jane |   32 |

**数据导入**

我们可以使用tablib库导入多种格式的文件，以初始化tablib的数据对象。如下所示：

```python
with open('data.csv', 'r') as fh:
    imported_data = tablib.Dataset().load(fh)
print(imported_data)
Name|Age
----|---
John|28 
Tom |16 
Jane|32 
```

对于表格类格式，如csv格式，也可以不导入标题行，即不将第一行作为标题行，如下所示：

```python
with open('data.csv', 'r') as fh:
    # headers=False不导入标题行
    imported_data = tablib.Dataset().load(fh,headers=False)
print(imported_data)
Name|Age
John|28 
Tom |16 
Jane|32 
```

对于支持多表的xls、xlsx，当前默认打开第一个表，注意使用rb模式。多表管理见下一节。

```python
with open('data.xls', 'rb') as fh:
    imported_data =  tablib.Dataset().load(fh, 'xls')
print(imported_data)
 
Name|Age 
----|----
John|28.0
Tom |16.0
Jane|32.0
```

## 1.3 多表管理

在Tablib中，Databook是一种数据结构，用于组织和管理多个数据表（Data Table。Databook提供了一种方便的方式来操作和处理多个数据表

**创建Databook**

```python
# 创建Databook
databook = tablib.Databook()
# 创建第一个数据表
data_table1 = tablib.Dataset()
 
# 设置数据表的列和数据
data_table1.headers = ['Name', 'Age']
data_table1.append(['John', 25])
data_table1.append(['Alice', 30])
# 设置表名
data_table1.title = "table1"
 
# 添加数据表到 Databook
databook.add_sheet(data_table1)
# 创建二个数据表
data_table2 = tablib.Dataset()
 
# 设置数据表的列和数据
data_table2.headers = ['Name', 'Age']
data_table2.append(['Jane', 34])
data_table2.append(['Mike', 14])
# 设置表名
data_table2.title = "table2"
 
# 添加数据表到 Databook
databook.add_sheet(data_table2)
# 可以利用现有表一次性创建Databoook
tablib.Databook((data_table1, data_table2))
<databook object>
```

**查看databook**

```python
# 查看子表数量
databook.size
2
# 查看各表
databook.sheets()
[<table1 dataset>, <table2 dataset>]
```

根据索引获得表

```python
for index,table in enumerate(databook.sheets()):
    print(f" \ntable{index}")
    print(table)
table0
Name |Age
-----|---
John |25 
Alice|30 
 
table1
Name|Age
----|---
Jane|34 
Mike|14 
```

**保存与导入**

databook支持保存xlsx和xls文件，但是导入仅支持xlsx文件。

```python
# 保存为xlsx文件
with open('databook.xlsx', 'wb') as f:
    f.write(databook.export('xlsx'))
# 多表导入
with open(r'databook.xlsx', 'rb') as fh:
    databook = tablib.Databook().load(fh, 'xlsx')
print(databook.sheets())
[<table1 dataset>, <table2 dataset>]
```

## 1.4 进阶使用

**动态列**

Talblib允许在数据表格中随意创建和管理动态列。这些列不需要预先定义，可以根据需要随时添加、删除和修改。如下所示根据随机函数设置列：

```python
# 导入数据
with open('data.csv', 'r') as fh:
    data = tablib.Dataset().load(fh)
print(data)
Name|Age
----|---
John|28 
Tom |16 
Jane|32 
import random
 
# 随机设置分数
def random_grade(row):
    # 根据传入的行设置不同数据标准
    if int(row[1]) > 30:
        return (random.randint(59,100)/100.0)
    else:
        return (random.randint(60,99)/100.0)
 
data.append_col(random_grade, header='Grade')
print(data)
Name|Age|Grade
----|---|-----
John|28 |0.65 
Tom |16 |0.99 
Jane|32 |0.79 
```

**数据过滤**

Tablib提供了filter方法，以根据数据集的标签(tags)来过滤数据。

```python
fruits = tablib.Dataset()  
 
fruits.headers = ['name', 'color'] 
# 添加tags为fruit与sour的行
fruits.append(['tomato', 'red'], tags=['fruit', 'sour']) 
fruits.append(['strawberry', 'red'], tags=['fruit', 'sweet' ]) 
fruits.append(['corn', 'yellow'], tags=['vegetable', 'sweet']) 
 
# 转换为其他格式，tags属性不会跟随转换
print(fruits.yaml)
- {color: red, name: tomato}
- {color: red, name: strawberry}
- {color: yellow, name: corn}
# 过滤出标签为vegetable的数据
fruits.filter(['vegetable']).df  
```

|      | name |  color |
| ---: | ---: | -----: |
|    0 | corn | yellow |

```python
# 过滤出标签为vegetable或sweet的数据
fruits.filter(['vegetable', 'sweet']).df  
```

|      |       name |  color |
| ---: | ---------: | -----: |
|    0 | strawberry |    red |
|    1 |       corn | yellow |

```python
# 先过滤出标签为fruit,再过滤为sour的数据
fruits.filter(['fruit']).filter(['sour']).df  
```

|      |   name | color |
| ---: | -----: | ----: |
|    0 | tomato |   red |

**分割符**

Tablib提供了append_separator函数，以在excel表格中添加分隔符，如下所示：

```python
# Daniel和Suzie的测试数据
daniel_tests = [
    ('11/24/09', 'Apple', 'Red'),
    ('05/24/10', 'Banana', 'Yellow')
]
 
suzie_tests = [
    ('11/24/09', 'Orange', 'Orange'),
    ('05/24/10', 'Grapes', 'Purple')
]
 
# 创建新的数据集
tests = tablib.Dataset()
tests.headers = ['Date', 'Fruit Name', 'Color']
 
# 添加分隔符
tests.append_separator('Fruits A')  
for test_row in daniel_tests:
   tests.append(test_row)
 
# 添加分隔符
tests.append_separator('')  
for test_row in suzie_tests:
   tests.append(test_row)
 
# 将数据集写入磁盘，以xls格式存储
with open('fruits.xls', 'wb') as f:
    f.write(tests.export('xls'))
```

通过展示xls数据，可以看到在某些行添加了空行数据。

```python
 
# 导入pandas库
import pandas as pd
 
# 从xls文件中读取数据，并将其存储在DataFrame中
pd.read_excel('fruits.xls', keep_default_na=False)
 
```

|      |     Date | Fruit Name |  Color |
| ---: | -------: | ---------: | -----: |
|    0 | Fruits A |            |        |
|    1 | 11/24/09 |      Apple |    Red |
|    2 | 05/24/10 |     Banana | Yellow |
|    3 |          |            |        |
|    4 | 11/24/09 |     Orange | Orange |
|    5 | 05/24/10 |     Grapes | Purple |

**格式化列**

Tablib提供add_formatter函数用于向Dataset对象添加自定义格式化程序，以便在导出数据时按照指定格式进行格式化。

```python
# 创建一个空的 Dataset 对象
data = tablib.Dataset()
 
# 添加数据到 Dataset
data.headers = ['name','age','role']
data.append(['John', 28, 'Developer'])
data.append(['Amy', 25, 'Designer'])
 
# 定义一个自定义的格式化函数
def custom_formatter(val):
    if isinstance(val, int):
        return f'Age: {val}'
    elif isinstance(val, str):
        return val.upper()
    else:
        return str(val)
 
# 添加自定义格式化函数到 Dataset
# 第一个参数可以为列号
data.add_formatter(0,custom_formatter)
# 如果有列名，也可以指定列名
data.add_formatter('age',custom_formatter)
 
# 导出数据并应用自定义格式化函数
data.df
```

|      | name |     age |      role |
| ---: | ---: | ------: | --------: |
|    0 | JOHN | Age: 28 | Developer |
|    1 |  AMY | Age: 25 |  Designer |

**创建子表格**

```python
# 创建表格
data = tablib.Dataset()
data.headers = ['Name', 'Age','Profession']
data.append(['Alice', 25, 'Doctor'])
data.append(['Bob', 30, 'Doctor'])
data.append(['Jack', 28, 'Engineer'])
# subset方法用于从现有的数据集中选择子集
# rows表示行号，rows=[0, 2]表示选择第0行和第2行
sub_data = data.subset(rows=[0, 2], cols=['Name', 'Profession'])
sub_data.df
```

|      |  Name | Profession |
| ---: | ----: | ---------: |
|    0 | Alice |     Doctor |
|    1 |  Jack |   Engineer |

# 2 参考

- [tablib](https://github.com/jazzband/tablib)
- [tablib-doc](https://tablib.readthedocs.io/en/stable/)。

本文来自博客园，作者：[落痕的寒假](https://www.cnblogs.com/luohenyueji/)，转载请注明原文链接：https://www.cnblogs.com/luohenyueji/p/17867000.html