> 最近语言的切换太多了，之前还在学习 Rust 和 Go，工作的线上系统是 Java ， 偶尔需要使用python，每次使用需要某些模块需要时间回忆，特开此页记之，温故知新。通常会直接以代码为例进行说明。



## python 语法助记

python常见模块的基本使用方法汇总。



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



