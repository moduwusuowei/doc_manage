# Python

## "{'item': '30'}"转成字典获取key&value

```
a = "{'item': '30'}"
print(type(a))
```

> <class 'str'>

```
a = "{'item': '30'}"
b = eval(a)
print(b["item"])
```

> 30

```
a = "{'item': '30'}"
b = eval(a)
key = b.keys()
print(key)
print(b.values())
```

> dict_keys(['item'])
> dict_values(['30'])

### 获取keys

```
a = "{'item': '30'}"
b = eval(a)
key = b.keys()
keys = list(key)[0]
print(keys)
```

> item

在这个示例中，我们首先将字符串a使用eval()函数转换为字典b。然后，我们使用b.keys()方法获取字典b的所有键，并将结果赋值给变量keys。最后，我们使用print()函数打印变量keys，即所有的键。

输出结果会是一个类似于dict_keys(['item'])的对象。如果需要将其转换为列表形式，可以使用list()函数进行转换：

```
keys_list = list(keys)
print(keys_list)
```

这样就会打印出['item']，即字典的所有键组成的列表。

请根据的实际需求修改代码，并根据你的字典内容进行相应的操作。如果还有其他问题，请随时提问。

```
a = "{'item': '30'}"
b = eval(a)
key = b.keys()
keys = list(key)[0]
for i in b.keys():
    print(i)
```

> item

### 获取values

```
a = "{'item': '30'}"
b = eval(a)
key = b.keys()
values = b.values()
keys = list(key)[0]
values = list(values)[0]
print(values)
```

> 30

```
a = "{'item': '30'}"
b = eval(a)
key = b.keys()
values = b.values()
keys = list(key)[0]
values = list(values)[0]
# print(values)
for i in b.values():
    print(i)
```

> 30



使用eval()函数来执行字符串中的表达式，并将结果赋值给b变量。eval()函数会将字符串解析为Python表达式，并执行该表达式，从而将字符串转换为字典。

## json报错

：json.decoder.JSONDecodeError: Invalid control character at: line 1 column 130135 (char 130134)

这个错误信息表示在解析 JSON 数据时遇到了一个无效的控制字符。

在 JSON 数据中，某些字符（如换行符 \n，回车符 \r，制表符 \t 等）需要被转义，即在这些字符前面加上一个反斜杠 \。如果这些字符没有被正确地转义，就会导致 JSONDecodeError。

错误信息中的 "line 1 column 130135 (char 130134)" 表示无效的控制字符出现在第一行的第130135个字符处。

为了解决这个问题，你可以检查并确保你的 JSON 数据中所有的控制字符都被正确地转义。如果你不能修改 JSON 数据，你也可以在调用 json.loads() 或 json.load() 函数时，传入 strict=False 参数来允许解析包含无效控制字符的 JSON 数据，例如：

import json

data = json.loads(json_string, strict=False)
这将允许 json.loads() 解析包含无效控制字符的 JSON 数据。但请注意，这可能会导致解析出的数据与原始 JSON 数据不完全相同。