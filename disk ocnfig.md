[TOC]
[MARKDOWN 用法](https://www.cnblogs.com/shawWey/p/8931697.html)

# 磁盘管理
## 挂载
1. umount
2. mount 准备挂的卷如/dev/sdc2 准备放的地方
## 格式化
1. mkfs 格式化
4. mkfs.ex4 格式化为ex4
## fdisk

## gdisk
## swapon

>用法：
 swapon [选项] [<指定>]
允许将设备和文件用于分页和交换。
选项：
 -a, --all                启用 /etc/fstab 中的所有交换区
 -d, --discard[=<policy>] 如果设备支持，启用 swap 丢弃
 -e, --ifexists           自动跳过不存在的设备而不提示
 -f, --fixpgsz            必要时重新初始化交换区
 -o, --options <列表>     以英文逗号分隔的 swap 选项
 -p, --priority <优先级>  指定交换设备的优先级
 -s, --summary            显示已使用交换设备的摘要(已废弃)
     --show[=<列>]        以可自定义的表格形式打印摘要
     --noheadings         不打印表头(与 --show 合用)
     --raw                使用原生输出格式(与 --show 合用)
     --bytes              在 --show 输出中以字节数显示交换区大小
 -v, --verbose                 详尽模式
 -h, --help               display this help
 -V, --version            display version

 >\>> swapon -s
文件名				类型		大小	已用	权限
/swapfile                              	file    	2097148	0	-2

提高优先级,优先级越高越好
>swapon --pri=2 /dev/sdb3

## mkswap
>mkswap /dev/sdb3

# LVM逻辑卷管理 前面fdisk gdisk可以扔掉了

## 名词定义

- PV: physical volume

- VG:  volume group

- LG: logical volume
- LVM: 物理卷管理

## 思路

### 组建

定义物理磁盘

> pvscan #扫描物理卷

> pvcreate /dev/sdb4 #创建物理卷

> pvdisplay #查看物理硬盘

多个物理磁盘合成一个卷组

> vgscan vgdisplay

> vgcreate vg0 /devsdb4 /dev/sdc2 #吧两块盘合并成为一个vg0

从卷组创建逻辑卷

> lvcreate -n 卷名 -L 空间大小如200M VG名如vg0 

可以对逻辑卷各种操作

### 扩展

扩展vg

> vgextend 空磁盘如/dev/sdd 要放到的卷组vg0

扩展LV

> lvextend -n 目标lv名如lv0 -L +大小如3G

把LV扩充让文件系统读出来

> xfs_growfs /root/bb

> resize2fs 目标挂载点#ext4的操作

### 删除

