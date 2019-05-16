# 基于spark分析平台体系结构

[TOC]

> 环境：hadoop HDFS作存储4节点（节点ESXI虚拟化）spark集群；
>
> 搭建过程不断完善更新

## 数据存储

### HDFS操作

HDFS的文件操作涉及到多个不同的文件系统，如何在不同的文件系统进行文件交互是关键，本地文件系统添加前缀`file:`或`file://`，HDFS添加前缀`hdfs`或`hdfs://`,如将本地的/opt/testhdfs.txt文件存入HDFS的根目录下:

```bash
hadoop fs -put file:/opt/testhdfs.txt hdfs:/
```

后面包含以下几部分内容，hadoop fs 指令概览，基本操作（包括上传文件（文件夹），下载文件，拷贝文件，移动文件，删除文件，读取文件，创建文件，获取逻辑空间大小，获取物理空间信息），spark存取HDFS。

1. hadoop fs 指令

   Usage: hadoop fs [generic options]

   ​	[-appendToFile <localsrc> ... <dst>]

   ​	[-cat [-ignoreCrc] <src> ...]

   ​	[-checksum <src> ...]

   ​	[-chgrp [-R] GROUP PATH...]

   ​	[-chmod [-R] <MODE[,MODE]... | OCTALMODE> PATH...]

   ​	[-chown [-R] [OWNER][:[GROUP]] PATH...]

   ​	[-copyFromLocal [-f] [-p] [-l] <localsrc> ... <dst>]

   ​	[-copyToLocal [-p] [-ignoreCrc] [-crc] <src> ... <localdst>]

   ​	[-count [-q] [-h] <path> ...]

   ​	[-cp [-f] [-p | -p[topax]] <src> ... <dst>]

   ​	[-createSnapshot <snapshotDir> [<snapshotName>]]

   ​	[-deleteSnapshot <snapshotDir> <snapshotName>]

   ​	[-df [-h] [<path> ...]]

   ​	[-du [-s] [-h] <path> ...]

   ​	[-expunge]

   ​	[-find <path> ... <expression> ...]

   ​	[-get [-p] [-ignoreCrc] [-crc] <src> ... <localdst>]

   ​	[-getfacl [-R] <path>]

   ​	[-getfattr [-R] {-n name | -d} [-e en] <path>]

   ​	[-getmerge [-nl] <src> <localdst>]

   ​	[-help [cmd ...]]

   ​	[-ls [-d] [-h] [-R] [<path> ...]]

   ​	[-mkdir [-p] <path> ...]

   ​	[-moveFromLocal <localsrc> ... <dst>]

   ​	[-moveToLocal <src> <localdst>]

   ​	[-mv <src> ... <dst>]

   ​	[-put [-f] [-p] [-l] <localsrc> ... <dst>]

   ​	[-renameSnapshot <snapshotDir> <oldName> <newName>]

   ​	[-rm [-f] [-r|-R] [-skipTrash] <src> ...]

   ​	[-rmdir [--ignore-fail-on-non-empty] <dir> ...]

   ​	[-setfacl [-R] [{-b|-k} {-m|-x <acl_spec>} <path>]|[--set <acl_spec> <path>]]

   ​	[-setfattr {-n name [-v value] | -x name} <path>]

   ​	[-setrep [-R] [-w] <rep> <path> ...]

   ​	[-stat [format] <path> ...]

   ​	[-tail [-f] <file>]

   ​	[-test -[defsz] <path>]

   ​	[-text [-ignoreCrc] <src> ...]

   ​	[-touchz <path> ...]

   ​	[-truncate [-w] <length> <path> ...]

   ​	[-usage [cmd ...]]

2. hadoop fs基本操作

   - 文件列表

     `hadoop fs -ls [-R] hdfs:///  # -R递归打印`


   - 上传文件put、copyFromLocal

     - put

       将本地/home/hadoop/sparkdir上传到HDFS

       `hadoop fs -put file:///home/hadoop/sparkdir hdfs:///   `

     - copyFromLocal下载文件

   - 下载文件get、copyToLocal

   - 拷贝文件cp

   - 创建文件夹mkdir

   - 移动文件mv

   - 删除文件rm

   - 读取文件cat

   - 创建文件touchz

   - 获取逻辑空间大小du

     ```python
     hadoop fs -du  hdfs:/// 	#显示HDFS根目录中各文件和文件夹大小
     hadoop fs -du -h  hdfs:///  #以最大单位显示HDFS根目录中各文件和文件夹大小
     hadoop fs -du -s  hdfs:///  #仅显示HDFS根目录大小。即各文件和文件夹大小之和
     ```

   - 获取物理空间信息count

     `hadoop fs -count -h hdfs://`

### spark存取HDFS

文件：

```python
textFile = sc.textFile("hdfs://...")
```

### saprk读取关系型数据库

1. mysql

```python
def mysqlsource():
    jdbcDF = sqlContext.read \
        .format("jdbc") \
        .option("url", "jdbc:mysql://202.204.54.75:3306/qinggang") \
        .option("dbtable", "auth_user") \
        .option("user", "root") \
        .option("password", "123456") \
        .load()
    return jdbcDF
```

2. oracle

```python
def oraclesource():
    ojdbcDF = sqlContext.read \
        .format("jdbc") \
        .option("url", "jdbc:oracle:thin:@202.204.54.75:1521:orcl") \
        .option("dbtable", "qg_user.RELATION_COF_MIDDLE_INPUT") \
        .option("user", "qg_user") \
        .option("password", "123456") \
        .option("driver","oracle.jdbc.driver.OracleDriver") \
        .load()
    return ojdbcDF
```

### NoSQL数据库

## RDD操作

Sparkit-learn待考虑

RDD缓存

cache

## spark实现机器学习

## 可视化

在网站系统中集成spark

