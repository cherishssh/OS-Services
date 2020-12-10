`作者信息`
```
# Author: SunShaoHua
# Date:   2020-12-09 17:01:30
# Email:  cherishaohua@foxmail.com
# Address: ShangHai/China
```


###目录

#### 运行fdisk -l 命令查看实例是否有数据盘
进入云虚拟机内执行 `fdisk -l` 
```
略
```

#### 如果执行命令后，发现新增数据盘/dev/vdb

运行 `fdisk /dev/vdb` 
```
对数据盘进行分区。依次输入n、p、1、回车、回车、wq
**************************************************************************
Welcome to fdisk (util-linux 2.23.2).

Changes will remain in memory only, until you decide to write them.
Be careful before using the write command.

Command (m for help): n                              #/ "n"是开始分区 /#                                              
Partition type:
   p   primary (2 primary, 0 extended, 2 free)       #/ 这里的"p"主分区 /#
   e   extended                                      #/ 这里的"e"扩展分区 /#
Select (default p): p                                #/ 这里我分一个主分区 /#
Partition number (1,2, default 1): 1                 #/ 选择主分区编号为1 /#
                                                     #/ 一路回车 /#
#/---------此处省略----------/#
Command (m for help): wq                              #/ "w"是存盘退出 /#
```

#### 格式化分区
执行命令 `mkfs.ext4 /dev/vdb1`
```
略
```

#### 挂载文件系统
运行命令 `mount /dev/vdb1 /New `  ;New为需要挂载的目录名称
```
略
```

#### 查询磁盘分区的UUID,如输出以下
`blkid /dev/vdb1`
```
/dev/vdb1: UUID="bb84333a-6a0d-4285-a14c-cf8b5da88d61" TYPE="ext4"
```


#### 写入文件/etc/fstab,在文件末尾添加一行
```
UUID=bb84333a-6a0d-4285-a14c-cf8b5da88d61  /NEW(需要挂载的目录)  ext4  defaults  0  0
```

##### 参数解释：
```
UUID=bb84333a-6a0d-4285-a14c-cf8b5da88d61：要挂载的磁盘分区的UUID
/NEW：挂载目录
ext4：分区格式为ext4
defaults：挂载时所要设定的参数(只读，读写，启用quota等)，输入defaults包括的参数有(rw、dev、exec、auto、nouser、async)
0：使用dump是否要记录，0为不需要，1为需要
2：2是开机时检查的顺序，boot系统文件为1，其他文件系统都为2，如不要检查就为0
```

#### 挂载文件系统 
运行命令 `mount /dev/vdb1 /NEW` ;/NEW 为需要挂载的系统目录

#### 检查系统磁盘使用情况
运行命令 ` df -Th`
