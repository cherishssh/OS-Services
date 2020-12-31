### 目录
<!-- MarkdownTOC -->

- [需求](#%E9%9C%80%E6%B1%82)
- [查看](#%E6%9F%A5%E7%9C%8B)
    - [运行`fdisk -l`](#%E8%BF%90%E8%A1%8Cfdisk--l)
- [配置](#%E9%85%8D%E7%BD%AE)
    - [创建分区](#%E5%88%9B%E5%BB%BA%E5%88%86%E5%8C%BA)
    - [格式化分区](#%E6%A0%BC%E5%BC%8F%E5%8C%96%E5%88%86%E5%8C%BA)
- [手动挂载分区](#%E6%89%8B%E5%8A%A8%E6%8C%82%E8%BD%BD%E5%88%86%E5%8C%BA)
    - [查看挂载信息](#%E6%9F%A5%E7%9C%8B%E6%8C%82%E8%BD%BD%E4%BF%A1%E6%81%AF)
- [自动挂载分区](#%E8%87%AA%E5%8A%A8%E6%8C%82%E8%BD%BD%E5%88%86%E5%8C%BA)
    - [写入文件/etc/fstab,在文件末尾添加一行](#%E5%86%99%E5%85%A5%E6%96%87%E4%BB%B6etcfstab%E5%9C%A8%E6%96%87%E4%BB%B6%E6%9C%AB%E5%B0%BE%E6%B7%BB%E5%8A%A0%E4%B8%80%E8%A1%8C)
    - [参数解释：](#%E5%8F%82%E6%95%B0%E8%A7%A3%E9%87%8A%EF%BC%9A)

<!-- /MarkdownTOC -->

`作者信息`
```
# Author: SunShaoHua
# Date:   2020-12-31 17:01:30
# Email:  cherishaohua@foxmail.com
# Address: ShangHai/China
```


### 需求
| 新增磁盘 | 容量 | 新挂载点 |
|----------|------|----------|
| /dev/sdb | 20G  | /backup  |

### 查看
#### 运行`fdisk -l` 
进入云虚拟机内执行 `fdisk -l` 命令查看实例是否有数据盘 ,看到新增磁盘 `/dev/sdb` ,进行下一步
```
[root@localhost ~]# fdisk -l

磁盘 /dev/sda：21.5 GB, 21474836480 字节，41943040 个扇区
Units = 扇区 of 1 * 512 = 512 bytes
扇区大小(逻辑/物理)：512 字节 / 512 字节
I/O 大小(最小/最佳)：512 字节 / 512 字节
磁盘标签类型：dos
磁盘标识符：0x000c40e9

   设备 Boot      Start         End      Blocks   Id  System
/dev/sda1   *        2048     2099199     1048576   83  Linux
/dev/sda2         2099200    41943039    19921920   8e  Linux LVM

磁盘 /dev/sdb：21.5 GB, 21474836480 字节，41943040 个扇区
Units = 扇区 of 1 * 512 = 512 bytes
扇区大小(逻辑/物理)：512 字节 / 512 字节
I/O 大小(最小/最佳)：512 字节 / 512 字节

[root@localhost ~]# 

```

### 配置

#### 创建分区
运行 `fdisk /dev/sdb` ,进行磁盘分区

```
[root@localhost ~]# fdisk /dev/sdb
欢迎使用 fdisk (util-linux 2.23.2)。

更改将停留在内存中，直到您决定将更改写入磁盘。
使用写入命令前请三思。

Device does not contain a recognized partition table
使用磁盘标识符 0xc2cf068b 创建新的 DOS 磁盘标签。

命令(输入 m 获取帮助)：n                                 #/ 输入 "n" 新建 /#  
Partition type:
   p   primary (0 primary, 0 extended, 4 free)
   e   extended
Select (default p): p                                 #/ 输入 "p" 主分区 /#
分区号 (1-4，默认 1)：1                                 #/ 输入主分区编号为 "1" /#
                                                      #/ 一路回车 /#
#/---------此处省略----------/#
分区 1 已设置为 Linux 类型，大小设为 20 GiB

命令(输入 m 获取帮助)：wq                                #/ "wq"是存盘退出 /#
```

#### 格式化分区
执行命令 `mkfs.ext4 /dev/sdb1` ,进行分区格式化, 正常 为输出以下信息
```
[root@localhost ~]# mkfs.ext4 /dev/sdb1
mke2fs 1.42.9 (28-Dec-2013)
文件系统标签=
OS type: Linux
块大小=4096 (log=2)
分块大小=4096 (log=2)
Stride=0 blocks, Stripe width=0 blocks
1310720 inodes, 5242624 blocks
262131 blocks (5.00%) reserved for the super user
第一个数据块=0
Maximum filesystem blocks=2153775104
160 block groups
32768 blocks per group, 32768 fragments per group
8192 inodes per group
Superblock backups stored on blocks: 
    32768, 98304, 163840, 229376, 294912, 819200, 884736, 1605632, 2654208, 
    4096000

Allocating group tables: 完成                            
正在写入inode表: 完成                            
Creating journal (32768 blocks): 完成
Writing superblocks and filesystem accounting information: 完成   

[root@localhost ~]# 
```

### 手动挂载分区
运行命令 `mount /dev/sdb1 /backup`  ,backup为需要挂载的系统目录名称; 

#### 查看挂载信息
运行命令 `df -Th` ,查看挂载信息;

```
[root@localhost ~]# df -Th
文件系统                类型      容量  已用  可用 已用% 挂载点
/dev/mapper/centos-root ext4       17G  1.1G   16G    6% /
devtmpfs                devtmpfs  898M     0  898M    0% /dev
tmpfs                   tmpfs     910M     0  910M    0% /dev/shm
tmpfs                   tmpfs     910M  9.6M  901M    2% /run
tmpfs                   tmpfs     910M     0  910M    0% /sys/fs/cgroup
/dev/sda1               ext4     1014M  146M  869M   15% /boot
/dev/sdb1               ext4       20G   45M   19G    1% /backup
[root@localhost ~]#
```

### 自动挂载分区
查询磁盘分区的UUID,如输出以下
`blkid /dev/sdb1`
```
/dev/vdb1: UUID="bb84333a-6a0d-4285-a14c-cf8b5da88d61" TYPE="ext4"
```

#### 写入文件/etc/fstab,在文件末尾添加一行
```
UUID=bb84333a-6a0d-4285-a14c-cf8b5da88d61  /backup  ext4  defaults  0  0
```

#### 参数解释：
```
UUID=bb84333a-6a0d-4285-a14c-cf8b5da88d61：要挂载的磁盘分区的UUID
/backup：挂载目录
ext4：分区格式为ext4
defaults：挂载时所要设定的参数(只读，读写，启用quota等)，输入defaults包括的参数有(rw、dev、exec、auto、nouser、async)
0：使用dump是否要记录，0为不需要，1为需要
2：2是开机时检查的顺序，boot系统文件为1，其他文件系统都为2，如不要检查就为0
```


****

<H2><Center>打赏作者喝杯咖啡</Center></H2>
<H3><center>如果觉得我的文章对您有用,请随意打赏.您的支持将鼓励我继续创作！</center></H3>
- [x]微信还是支付宝？

<img src="https://gitee.com/cherishssh/images/raw/master/Image/Wechat.jpeg" height="340" width="235"> <img src="https://gitee.com/cherishssh/images/raw/master/Image/WechatAL.jpeg" height="340" width="235">


<!-- <img src="https://raw.githubusercontent.com/cherishssh/OS-Services/main/Image/Wechat.jpeg" height="320" width="235"> 
<img src="https://raw.githubusercontent.com/cherishssh/OS-Services/main/Image/WechatAL.jpeg" height="320" width="235">  -->
<!-- ![](https://gitee.com/cherishssh/OS-Services/raw/main/Wechat.jpeg) -->
<!-- https://gitee.com/cherishssh/images/raw/master/Image/WechatAL.jpeg -->