
## linux就该这么学

[在线文档](https://www.linuxprobe.com/docs/LinuxProbe.pdf)

## 鸟哥的linux私房菜



> centos-study root-ccx+931019+Ysfh；changxin ccx+Ysfh）

- Unix的前身是由贝尔实验室（Bell lab.）的Ken Thompson利用组合语言写成的， 后来在1971-1973年间由Dennis Ritchie以C程序语言进行改写，才称为Unix。

SD （Berkeley Software Distribution）

1984年由Richard Stallman提倡GNU计划，倡导自由软件（Free software）

GPL授权模式， 任何GPL（General Public License）软件均不可单纯仅贩卖其软件，也不可修改软件授权。

柱面**-**磁头**-**扇区（英语：Cylinder-head-sector，简称为**CHS**）

​	初设计就是在类似盘片同心圆上面切出一个一个的小区块，这些小区块整合成一个圆形，让机器手臂上的磁头去存取。 这个小区块就是磁盘的最小物理储存单位，称之为扇区 （sector），那同一个同心圆的扇区组合成的圆就是所谓的磁道（track）。 由于磁盘里面可能会有多个盘片，因此在所有盘片上面的同一个磁道可以组合成所谓的柱面 （cylinder）

磁盘分区

MBR （Master Boot Record）

第一个扇区 512Bytes 会有这两个数据：

- 主要开机记录区（Master Boot Record, MBR）：可以安装开机管理程序的地方，有446 Bytes
- 分区表（partition table）：记录整颗硬盘分区的状态，有64 Bytes

由于分区表所在区块仅有64 Bytes容量，因此最多仅能有四组记录区，每组记录区记录了该区段的启始与结束的柱面号码。

几个重点信息

- 其实所谓的“分区”只是针对那个64 Bytes的分区表进行设置而已！
- 硬盘默认的分区表仅能写入四组分区信息
- 这四组分区信息我们称为主要（Primary）或延伸（Extended）分区
- 分区的最小单位“通常”为柱面（cylinder）
- 当系统要写入磁盘时，一定会参考磁盘分区表，才能针对某个分区进行数据的处理

MBR 主要分区、延伸分区与逻辑分区的特性我们作个简单的定义：

- 主要分区与延伸分区最多可以有四笔（硬盘的限制）
- 延伸分区最多只能有一个（操作系统的限制）
- 逻辑分区是由延伸分区持续切割出来的分区；
- 能够被格式化后，作为数据存取的分区为主要分区与逻辑分区。延伸分区无法格式化；
- 逻辑分区的数量依操作系统而不同，在Linux系统中SATA硬盘已经可以突破63个以上的分区限制；

GPT （GUID partition table）

​	早期磁盘第一个扇区里面含有的重要信息我们称为MBR （Master Boot Record） 格式，但是由于近年来磁盘的容量不断扩大，造成读写上的一些困扰， 甚至有些大于 2TB 以上的磁盘分区已经让某些操作系统无法存取。因此后来又多了一个新的磁盘分区格式，称为 GPT （GUID partition table）！ 这两种分区格式与限制不太相同啦！

​	逻辑区块位址（Logical Block Address, LBA，默认为 512Bytes）

Linux的设备都是以文件的型态存在

****

Bios和UEFI

简单的说，整个开机流程到操作系统之前的动作应该是这样的：

1. **BIOS**：开机主动执行的固件，会认识第一个可开机的设备；
2. **MBR**：第一个可开机设备的第一个扇区内的主要开机记录区块，内含开机管理程序；
3. 开机管理程序（**boot loader**）：一支可读取核心文件来执行的软件；
4. 核心文件：开始操作系统的功能...

文件系统与目录树的关系（挂载**)**

**	**所谓的“挂载”就是利用一个目录当成进入点，将磁盘分区的数据放置在该目录下； 也就是说，进入该目录就可以读取该分区的意思。这个动作我们称为“挂载”，那个进入点的目录我们称为“挂载点”。 由于整个Linux系统最重要的是根目录，因此根目录一定需要挂载到某个分区的。

那分区的文件名又是什么？** **

正常的实体机器大概使用的都是 /dev/sd[a-] 的磁盘文件名，至于虚拟机环境下面，为了加速，可能就会使用 /dev/vd[a-p] 这种设备文件名喔

如何进行磁盘分区？

磁盘分区有哪些限制？目前的 BIOS 与 UEFI 分别是啥？

MSDOS 与 GPT 又是啥

使用者和群组

root /etc/passwd

个人密码 /etc/shadow

群组名称 /etc/group

Ls -al

[权限] [链接] [拥有者] [群组] [文件大小] [修改日期] [文件名]

- 第一个字符代表这个文件是“目录、文件或链接文件等等”：

- - 当为[** d** ]则是目录，例如[上表](applewebdata://37D7C481-A229-407A-823E-7E99879F4C04/part0007.html#table2.1.1)文件名为“.config”的那一行；
  - 当为[** -** ]则是文件，例如[上表](applewebdata://37D7C481-A229-407A-823E-7E99879F4C04/part0007.html#table2.1.1)文件名为“initial-setup-ks.cfg”那一行；
  - 若是[** l** ]则表示为链接文件（link file）；
  - 若是[** b** ]则表示为设备文件里面的可供储存的周边设备（可随机存取设备）；
  - 若是[** c** ]则表示为设备文件里面的序列埠设备，例如键盘、鼠标（一次性读取设备）。

- 

- 接下来的字符中，以三个为一组，且均为“rwx” 的三个参数的组合。其中，[ r ]代表可读（read）、[ w ]代表可写（write）、[ x ]代表可执行（execute）。 要注意的是，这三个权限的位置不会改变，如果没有权限，就会出现减号[ - ]而已。

- - 第一组为“文件拥有者可具备的权限”，以“initial-setup-ks.cfg”那个文件为例， 该文件的拥有者可以读写，但不可执行；
  - 第二组为“加入此群组之帐号的权限”；
  - 第三组为“非本人且没有加入本群组之其他帐号的权限”。

改变文件属性与权限

chgrp ：改变文件所属群组

chown ：改变文件拥有者

chmod ：改变文件的权限, SUID, SGID, SBIT等等的特性

chmod 754 filename 或 chmod u=rwx,g=rx,o=r filename

linux目录结构：

Filesystem Hierarchy Standard （FHS）

目录与路径

cd是Change Directory

pwd是Print Working Directory  [-P]显示真实路径，而非链接

**mkdir [-mp] **目录名称

选项与参数：

-m ：设置文件的权限喔！直接设置，不需要看默认权限 （umask） 的脸色～

-p ：帮助你直接将所需要的目录（包含上层目录）递回创建起来！

It's much better to create a custom.sh shell script in

/etc/profile.d/ to make custom changes to your environment, as this

will prevent the need for merging in future updates.

九章**vim**

断行符

win:CRLF;Linux:LF

转换文件编码：

iconv -f 原本编码 -t 新编码 filename [-o newfile]

iconv -f big5 -t utf8 vi.big5 -o vi.utf8

十章**bash**

- 变量的设置与使用
- bash 操作环境的创建
- 数据流重导向的功能
- 管线命令

type which 查看指令来源

[ctrl]+u/[ctrl]+k     

分别是从光标处向前删除指令串 （[ctrl]+u） 及向后删除指令串 （[ctrl]+k）。    

[ctrl]+a/[ctrl]+e     

分别是让光标移动到整个指令串的最前面 （[ctrl]+a） 或最后面 （[ctrl]+e）。  

读取变量

echo $variablename

locate列出文件名数据

export variablename 将变量声明为环境变量

分割 排序 统计次数

last | cut -d ' ' -f 1 | sort | uniq -c



## 常用Linux 命令

[系统管理员应该知道的 20 条 Linux 命令](https://my.oschina.net/editorial-story/blog/1499026)

> 应该还有一个给指令分类别的层级，比如网络相关，文件处理等

#### curl

[参考文档](https://curl.haxx.se/)