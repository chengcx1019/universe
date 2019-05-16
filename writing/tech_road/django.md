# Django生态

## Django的RESTful实践

采用django-rest-framework,示例参考[github主页](https://github.com/encode/django-rest-framework/tree/master)

可以采用curl指令与api进行交互:

```shell
# get all users
$ curl -H 'Accept: application/json; indent=4' -u admin:password http://127.0.0.1:8000/users/
[
    {
        "url": "http://127.0.0.1:8000/users/1/",
        "username": "admin",
        "email": "admin@example.com",
        "is_staff": true,
    }
]

# create user
$ curl -X POST -d username=new -d email=new@example.com -d is_staff=false -H 'Accept: application/json; indent=4' -u admin:password http://127.0.0.1:8000/users/
{
    "url": "http://127.0.0.1:8000/users/2/",
    "username": "new",
    "email": "new@example.com",
    "is_staff": false,
}

```



### 理解序列化

[序列化与反序列化](https://docs.python-guide.org/scenarios/serialization/)，实际上就是将内存中的结构化数据与可以存储或传输的格式相互转化过程（注意是相互转化，即内存中的结构化数据序列化之后可以被还原），其他格式的数据就包括文本等，在实际运用过程中，python中多为json反序列化为dict，dict序列化为json，或者数据库的model对象序列化为文本，文本反序列化为model对象。

有的方式只能被一种语言支持，如python的pickle，如果需要被多种语言支持需要选择合适的方式，如protobuf.

在转换到语言的原始类型后，当然是有一百万种方式把数据填充到对象实例中

### 认证和权限

TODO

### cassandra+django

[参考文档](http://r4fek.github.io/django-cassandra-engine/guide/management_commands/)

```shell
makemigrations: syncdb --database cassandra
migrate: sync_cassandra
dbshell  --database cassandra

```

> 其中cassandra是settings中DATABASES的key值

基本了解了，目前没有必要集成，复用鹏飞的逻辑就好了。

## django基本

### django model



- [ ] 稳定开发之后，记录每一次的数据库变更，从model的filed变更中抽出sql
- [x] model间外键关联及查询，[many to one](https://docs.djangoproject.com/zh-hans/2.1/topics/db/examples/many_to_one/) 可以为one生成一个对象集合
- [x] Error:Cannot apply DjangoModelPermissionsOrAnonReadOnly on a view that does not set `.queryset` or have a `.get_queryset()` method.
- [ ] 如果是获取某一类资源按条件筛选的部分实体，在restful中应该如何表达：比如筛选出属于某个实验的所有仿真（可以在entities的get方法中添加删选条件）

## 测试工具

[httpie](https://httpie.org/doc)

使用：

`http [flags] [METHOD] URL [ITEM [ITEM]]` or `http --help`



simulation/process/finish_service_processor.py

common/apis.py

### 我关心的几个问题rest framework是否已经考虑到了

respondse封装,需要进行元编程 

返回对象类型变更，添加额外数据字段，去除无需字段

