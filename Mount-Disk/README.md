### 目录
<!-- MarkdownTOC -->

- [运行fdisk -l 命令查看实例是否有数据盘](#%E8%BF%90%E8%A1%8Cfdisk--l-%E5%91%BD%E4%BB%A4%E6%9F%A5%E7%9C%8B%E5%AE%9E%E4%BE%8B%E6%98%AF%E5%90%A6%E6%9C%89%E6%95%B0%E6%8D%AE%E7%9B%98)
- [如果执行命令后，发现新增数据盘/dev/vdb](#%E5%A6%82%E6%9E%9C%E6%89%A7%E8%A1%8C%E5%91%BD%E4%BB%A4%E5%90%8E%EF%BC%8C%E5%8F%91%E7%8E%B0%E6%96%B0%E5%A2%9E%E6%95%B0%E6%8D%AE%E7%9B%98devvdb)
- [格式化分区](#%E6%A0%BC%E5%BC%8F%E5%8C%96%E5%88%86%E5%8C%BA)
- [挂载文件系统](#%E6%8C%82%E8%BD%BD%E6%96%87%E4%BB%B6%E7%B3%BB%E7%BB%9F)
- [查询磁盘分区的UUID,如输出以下](#%E6%9F%A5%E8%AF%A2%E7%A3%81%E7%9B%98%E5%88%86%E5%8C%BA%E7%9A%84uuid%E5%A6%82%E8%BE%93%E5%87%BA%E4%BB%A5%E4%B8%8B)
- [写入文件/etc/fstab,在文件末尾添加一行](#%E5%86%99%E5%85%A5%E6%96%87%E4%BB%B6etcfstab%E5%9C%A8%E6%96%87%E4%BB%B6%E6%9C%AB%E5%B0%BE%E6%B7%BB%E5%8A%A0%E4%B8%80%E8%A1%8C)
    - [参数解释：](#%E5%8F%82%E6%95%B0%E8%A7%A3%E9%87%8A%EF%BC%9A)
- [挂载文件系统](#%E6%8C%82%E8%BD%BD%E6%96%87%E4%BB%B6%E7%B3%BB%E7%BB%9F-1)
- [检查系统磁盘使用情况](#%E6%A3%80%E6%9F%A5%E7%B3%BB%E7%BB%9F%E7%A3%81%E7%9B%98%E4%BD%BF%E7%94%A8%E6%83%85%E5%86%B5)

<!-- /MarkdownTOC -->

`作者信息`
```
# Author: SunShaoHua
# Date:   2020-12-09 17:01:30
# Email:  cherishaohua@foxmail.com
# Address: ShangHai/China
```

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

****

<H2><Center>打赏作者喝杯咖啡</Center></H2>
<H3><center>如果觉得我的文章对您有用,请随意打赏.您的支持将鼓励我继续创作！</center></H3>
- [x]微信还是支付宝？

<img src="https://gitee.com/cherishssh/images/raw/master/Image/Wechat.jpeg" height="340" width="235"> <img src="https://gitee.com/cherishssh/images/raw/master/Image/WechatAL.jpeg" height="340" width="235">


<!-- <img src="https://raw.githubusercontent.com/cherishssh/OS-Services/main/Image/Wechat.jpeg" height="320" width="235"> 
<img src="https://raw.githubusercontent.com/cherishssh/OS-Services/main/Image/WechatAL.jpeg" height="320" width="235">  -->
<!-- ![](https://gitee.com/cherishssh/OS-Services/raw/main/Wechat.jpeg) -->
<!-- https://gitee.com/cherishssh/images/raw/master/Image/WechatAL.jpeg -->