网络上有很多相关教程，根据相关步骤进行操作之后，有些人可以得到预期的结果，但很不幸，我没有，他们只讲应该怎么做，却不讲出现问题了应该怎么办。

如果顺利的话，按照步骤一般可以实现mac原生对NTFS读写的支持：

1. 插上移动硬盘，使用diskutil list查看硬盘的挂载名, Windows_NTFS后面接的就是NAME：

   | TYPE         | NAME         |
   | ------------ | ------------ |
   | Windows_NTFS | yourdiskname |

2. 为了防止你的多个硬盘使用同一个name，同时插入无法同时使用，获取硬盘的UUID：

   ```
   $ diskutil info /Volumes/yourdiskname | grep UUID
   Volume UUID:              C93ED552-D44F-4F58-BFD9-1BC12395A228
   ```
   如果UUID没有输出的话，则说明没有UUID的属性（可能移动硬盘有而一般的U盘没有）,这时使用Name属性：

   ```shell
   $ diskutil info /Volumes/yourdiskname | grep Name
   Volume Name:              yourdiskname
   ```

3. 将挂载指令写入配置文件：

   ```
   echo "UUID=C93ED552-D44F-4F58-BFD9-1BC12395A228 none ntfs rw,auto,nobrowse" | sudo tee -a /etc/fstab
   ```
   同理，对应于Name属性，使用如下指令：

   ```
   echo "LABEL=yourdiskname none ntfs rw,auto,nobrowse" | sudo tee -a /etc/fstab
   ```

   >补充说明 linux指令tee：
   >
   >**功能说明：**读取标准输入的数据，并将其内容输出成文件。
   >
   >**语　　法：**tee\[-ai\]\[—help\]\[—version\]\[文件…\]
   >
   >**补充说明：**tee指令会从标准输入设备读取数据，将其内容输出到标准输出设备，同时保存成文件。
   >
   >**参　　数：**
   >　-a或--append 　附加到既有文件的后面，而非覆盖它． 
   >　-i-i或--ignore-interrupts 　忽略中断信号。 
   >　--help 　在线帮助。 
   >　--version 　显示版本信息。


   则有多个硬盘则使用-a参数继续输出到`/etc/fstab`文件中

完成配置文件的写入后，推出移动硬盘(⌘+E)，然后重新插入，此时硬盘是不会在find的左侧边栏出现的，如果依然出现，重新检查上面的步骤，如果没有出现，shell下查看/Volumns文件夹下硬盘是否成功挂载：`ls /Volumes` ,如果按照预期出现你的硬盘，那么就可以成功的进行硬盘的读写。

> 如果没有出现，不要气馁，硬盘的盘符没有出现在find的左侧边栏，至少可以确认的是上面的步骤你已经成功了，下面我们来分析到底为什么没有成功。

我们需要知道到底发生了什么，使得我们的硬盘不能被正确的挂载（mount），使用指令dmesg(dmesg输出的信息通常包含设备驱动的相关信息)：

```shell
$ sudo dmesg
···NTFS volume is dirty. You should unmount it and run chkdsk···
```

查看输出信息是否包含`NTFS volume is dirty. You should unmount it and run chkdsk`此类信息，如果含此类信息，则说明这个磁盘被标记为dirty，因而没有成功在mac下挂载。

> windows官方对于dirty的解释是这样，`A volume can be marked dirty if the system suffered a`
>
> ` power interruption (power failure, plug pulling, battery removal, power button), ` 
>
> `aborted restart or ungraceful shutdown .`。
>
> 磁盘被标记为dirty通常包含以下情况：1、突然断电；2、中断重启；3、意外关机，通常我们可以理解为硬盘在windows上使用的时候没有正确拔出。

以下操作最好在硬盘数据备份好的情况下进行，通常不会出大的问题，但之后的操作或多或少还是存在误操作导致磁盘无法读取的情况，例如我自己在操作过程中，破坏了磁盘的分区索引，使得磁盘在windows系统直接提示磁盘需要格式化，一定不要信以为真误点格式化，后面会提到补救的方法。

有网友提示用NTFS-3G进行磁盘修复，根据NTFS-3G在[GitHub 上的指引](https://github.com/osxfuse/osxfuse/wiki/NTFS-3G)进行安装（安装前NTFS-3G你需要下载安装[Fuse](https://github.com/osxfuse/osxfuse/releases)），即安装完Fuse后，执行`brew install ntfs-3g`，然后可以使用ntfsfix指令进行磁盘修复。

```shell
$ ntfsfix -help
ntfsfix v2017.3.23 (libntfs-3g)

Usage: ntfsfix [options] device
    Attempt to fix an NTFS partition.

    -b, --clear-bad-sectors Clear the bad sector list
    -d, --clear-dirty       Clear the volume dirty flag
    -h, --help              Display this help
    -n, --no-action         Do not write anything
    -V, --version           Display version information

For example: ntfsfix /dev/hda6
```

修复之前我们需要得到磁盘的标志符（IDENTIFIER），重新回到之前用过的diskutil指令，执行：

```shell
$ diskutil list
```

如果没有出现磁盘的话，我们需要在最初那样以只读模式挂载磁盘，应而需要修改/etc/fstab文件，在UUID里任意添加一位即可(之后需要还原修改)，弹出硬盘再重新插入，执行`diskutil list`，获取磁盘的标志符，之后才能修复该磁盘：

> 执行`diskutil list`，找到如下信息：
>
> **/dev/disk2** (external, physical):
>
> | #:   | TYPE NAME              | NAME         | SIZE    | IDENTIFIER  |
> | ---- | ---------------------- | ------------ | ------- | ----------- |
> | 0:   | FDisk_partition_scheme |              | *2.0 TB | disk2       |
> | 1:   | Windows_NTFS           | yourdiskname | 2.0 TB  | **disk2s1** |
> 找到yourdiskname对应的标志符（IDENTIFIER）disk2s1

```shell
$ sudo ntfsfix /dev/disk2s1
Mounting volume... The disk contains an unclean file system (0, 0).
FAILED
Attempting to correct errors... 
Processing $MFT and $MFTMirr...
Reading $MFT... OK
Reading $MFTMirr... OK
Comparing $MFTMirr to $MFT... OK
Processing of $MFT and $MFTMirr completed successfully.
Setting required flags on partition... OK
Going to empty the journal ($LogFile)... 
```

注意查看该指令是否成功，下面会谈到一种失败的情况，其他的情况暂时不清楚，如果你碰到了，可以联系我changxincheng1019@gmail.com。

> 如果不成功(我自己操作中遇到的问题，你可能不会遇到)则可以会破坏磁盘本身的分区索引，导致磁盘文件无法读取，我试着将磁盘插入windows中，直接提示该磁盘需要格式化，此时需要重建分区索引，使用软件DiskGenius（别的磁盘修复软件应该也可以）重建分区索引并保存，此时windows可以重新读取该磁盘，正确从windows系统弹出磁盘。

如果修改了/etc/fstab，还原/etc/fstab文件的修改，推出磁盘重新插入，shell下查看/Volumns文件夹下硬盘是否成功挂载：`ls /Volumes` ，希望你能看到想要的结果。



