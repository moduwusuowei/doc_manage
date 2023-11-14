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