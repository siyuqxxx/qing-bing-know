# CentOS7 挂载新磁盘

[TOC]

# 参考资料

[centos7 添加硬盘](https://www.jianshu.com/p/edd227309a8c)
[文件系统EXT3，EXT4和XFS的区别](https://blog.csdn.net/sdd220/article/details/76216164)
[linux之fstab文件详解](https://blog.csdn.net/richerg85/article/details/17917129)

# 检查新添加的硬盘信息

```sh
# 查看磁盘名称
fdisk -l

# 查看磁盘分区情况
lsblk -f
```

控制台输出如下：

```sh
[root@localhost ~]# fdisk -l

磁盘 /dev/sda：34.4 GB, 34359738368 字节，67108864 个扇区
Units = 扇区 of 1 * 512 = 512 bytes
扇区大小(逻辑/物理)：512 字节 / 512 字节
I/O 大小(最小/最佳)：512 字节 / 512 字节
磁盘标签类型：dos
磁盘标识符：0x000b99a9

   设备 Boot      Start         End      Blocks   Id  System
/dev/sda1   *        2048     2099199     1048576   83  Linux
/dev/sda2         2099200    67108863    32504832   8e  Linux LVM

磁盘 /dev/sdb：8589 MB, 8589934592 字节，16777216 个扇区
Units = 扇区 of 1 * 512 = 512 bytes
扇区大小(逻辑/物理)：512 字节 / 512 字节
I/O 大小(最小/最佳)：512 字节 / 512 字节


磁盘 /dev/mapper/centos-root：31.1 GB, 31130124288 字节，60801024 个扇区
Units = 扇区 of 1 * 512 = 512 bytes
扇区大小(逻辑/物理)：512 字节 / 512 字节
I/O 大小(最小/最佳)：512 字节 / 512 字节


磁盘 /dev/mapper/centos-swap：2147 MB, 2147483648 字节，4194304 个扇区
Units = 扇区 of 1 * 512 = 512 bytes
扇区大小(逻辑/物理)：512 字节 / 512 字节
I/O 大小(最小/最佳)：512 字节 / 512 字节

[root@localhost ~]# lsblk -f
NAME            FSTYPE      LABEL UUID                                   MOUNTPOINT
sda                                                                      
├─sda1          xfs               61b8199f-9404-4ab3-8cb0-22255ce00042   /boot
└─sda2          LVM2_member       qxwSa1-kMXU-VY8y-lFIK-T6Yf-cWRN-y13aFT 
  ├─centos-root xfs               59438b25-3d1c-4dfc-9689-dfa0e3c909d4   /
  └─centos-swap swap              eb06d375-fe33-4ce6-a38a-eb4dd0f3c01f   [SWAP]
sdb                                                                      
sr0        
```

# 硬盘分区

结合上面使用命令 `fdisk -l`  查看磁盘名称的结果，可以得知 /dev/sdb
是一块新添加且没有分区的硬盘；

此时，使用 `fdisk /dev/sdb` 对新磁盘进行分区

```sh
[root@localhost ~]# fdisk /dev/sdb
欢迎使用 fdisk (util-linux 2.23.2)。

更改将停留在内存中，直到您决定将更改写入磁盘。
使用写入命令前请三思。

Device does not contain a recognized partition table
使用磁盘标识符 0x5aef59e7 创建新的 DOS 磁盘标签。

命令(输入 m 获取帮助)：n                             // 输入N表示新建一个分区
Partition type:
   p   primary (0 primary, 0 extended, 4 free)
   e   extended
Select (default p): 
Using default response p
分区号 (1-4，默认 1)：
起始 扇区 (2048-16777215，默认为 2048)：
将使用默认值 2048
Last 扇区, +扇区 or +size{K,M,G} (2048-16777215，默认为 16777215)：
将使用默认值 16777215
分区 1 已设置为 Linux 类型，大小设为 8 GiB

命令(输入 m 获取帮助)：w                             // 保存分区
The partition table has been altered!

Calling ioctl() to re-read partition table.
正在同步磁盘。
```

## 检查结果
```sh
# 查看磁盘名称
fdisk -l

# 查看磁盘分区情况
lsblk -f
```

控制台输出如下：

```sh
[root@localhost ~]# lsblk -f
NAME            FSTYPE      LABEL UUID                                   MOUNTPOINT
sda                                                                      
├─sda1          xfs               61b8199f-9404-4ab3-8cb0-22255ce00042   /boot
└─sda2          LVM2_member       qxwSa1-kMXU-VY8y-lFIK-T6Yf-cWRN-y13aFT 
  ├─centos-root xfs               59438b25-3d1c-4dfc-9689-dfa0e3c909d4   /
  └─centos-swap swap              eb06d375-fe33-4ce6-a38a-eb4dd0f3c01f   [SWAP]
sdb                                                                      
└─sdb1                                                                   
sr0                                                                      
[root@localhost ~]# fdisk -l

磁盘 /dev/sda：34.4 GB, 34359738368 字节，67108864 个扇区
Units = 扇区 of 1 * 512 = 512 bytes
扇区大小(逻辑/物理)：512 字节 / 512 字节
I/O 大小(最小/最佳)：512 字节 / 512 字节
磁盘标签类型：dos
磁盘标识符：0x000b99a9

   设备 Boot      Start         End      Blocks   Id  System
/dev/sda1   *        2048     2099199     1048576   83  Linux
/dev/sda2         2099200    67108863    32504832   8e  Linux LVM

磁盘 /dev/sdb：8589 MB, 8589934592 字节，16777216 个扇区
Units = 扇区 of 1 * 512 = 512 bytes
扇区大小(逻辑/物理)：512 字节 / 512 字节
I/O 大小(最小/最佳)：512 字节 / 512 字节
磁盘标签类型：dos
磁盘标识符：0x5aef59e7

   设备 Boot      Start         End      Blocks   Id  System
/dev/sdb1            2048    16777215     8387584   83  Linux

磁盘 /dev/mapper/centos-root：31.1 GB, 31130124288 字节，60801024 个扇区
Units = 扇区 of 1 * 512 = 512 bytes
扇区大小(逻辑/物理)：512 字节 / 512 字节
I/O 大小(最小/最佳)：512 字节 / 512 字节


磁盘 /dev/mapper/centos-swap：2147 MB, 2147483648 字节，4194304 个扇区
Units = 扇区 of 1 * 512 = 512 bytes
扇区大小(逻辑/物理)：512 字节 / 512 字节
I/O 大小(最小/最佳)：512 字节 / 512 字节  
```

# 格式化新分区

使用 `mkfs.xfs /dev/sdb1` 将新分区格式化为 xfs 文件系统

```sh
[root@localhost ~]# mkfs.xfs /dev/sdb1
meta-data=/dev/sdb1              isize=512    agcount=4, agsize=524224 blks
         =                       sectsz=512   attr=2, projid32bit=1
         =                       crc=1        finobt=0, sparse=0
data     =                       bsize=4096   blocks=2096896, imaxpct=25
         =                       sunit=0      swidth=0 blks
naming   =version 2              bsize=4096   ascii-ci=0 ftype=1
log      =internal log           bsize=4096   blocks=2560, version=2
         =                       sectsz=512   sunit=0 blks, lazy-count=1
realtime =none                   extsz=4096   blocks=0, rtextents=0
```

## 查看格式化结果

```sh
[root@localhost ~]# lsblk -f
NAME            FSTYPE      LABEL UUID                                   MOUNTPOINT
sda                                                                      
├─sda1          xfs               61b8199f-9404-4ab3-8cb0-22255ce00042   /boot
└─sda2          LVM2_member       qxwSa1-kMXU-VY8y-lFIK-T6Yf-cWRN-y13aFT 
  ├─centos-root xfs               59438b25-3d1c-4dfc-9689-dfa0e3c909d4   /
  └─centos-swap swap              eb06d375-fe33-4ce6-a38a-eb4dd0f3c01f   [SWAP]
sdb                                                                      
└─sdb1          xfs               0804178e-8a0a-40ca-a9e6-4b59b00b9f69   
sr0
```

# 挂载新分区到文件系统

```sh
# 创建挂载目录 
mkdir /opt/data

# 设置新硬盘开机自动挂载 
echo "/dev/sdb1 /opt/data xfs defaults 0 0">>/etc/fstab

# 重载 mount 使配置生效 
mount -a
```