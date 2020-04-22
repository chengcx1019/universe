> 最近语言的切换较多，之前在理解 Rust 和 Go 的特性，工作的线上系统使用 Java ， 偶尔需要使用python，使用某些功能需要时间回忆，特开此页记之，温故知新。通常会直接以代码为例进行说明。



## python 语法助记

python常见模块的基本使用方法汇总。

***

### Json

Json 模块主要是四个功能，从 json 字符串反序列化为 python 数据类型，把 python 数据类型序列化为json 字符串，将 json 序列化到文件，从文件加载 json 。

```python
import json

# 从 json 字符串反序列化为 python 数据类型
json_str = '{ "say_hi": "hello world"}'
json_dict = json.loads(json_str)

# 把 python 数据类型序列化为json 字符串
json_str_again = json.dumps(json_dict)

# 将 json 序列化到文件
with open('same_file', 'w') as f:
  json.dump(json_dict, f)

# 从文件加载 json
with open('same_file') as f:
  json_dict_from_file = json.load(f)

# 以更易读的方式输出 json 字符串
print(json.dumps(json_dict_from_file, indent = 4, sort_keys=True))
```

***

### Re

正则表达式本身是比较完整的有体系的一部分内容，这里不深究具体的正则表达式的书写，而只是一个通用的匹配及获取匹配结果的操作。我们假设从你已经获得了有效的 pattern 开始。

**findall** : 返回所有匹配的模式，如果没有任何匹配，则返回空列表

```python
import re

string = '1993 hello 12 world 12'
pattern = '\d+'
result = re.findall(pattern, string)
# result: ['1993', '12', '12']
```

**search** : 返回一个 `Match` 对象，否则返回 `None`

```python
import re

string = "1993 12 12"
pattern = "(\d{4}) (\d{2} (\d{2})) "
match = re.search('\Acoronavirus', string)

if match:
    print("matched")
else:
    print("pattern not found")  
# Output: matched
match.groups()
# Output: ('1993', '12', '12')  tuple 类型
match.group()
# Output: '1993 12 12'
match.group(1)
# Output: '1993'
match.group(0,1,2,3)
# Output: ('1993 12 12', '1993', '12', '12')
```





