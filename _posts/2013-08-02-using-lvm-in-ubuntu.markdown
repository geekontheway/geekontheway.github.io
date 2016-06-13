---
layout: post
title: "Ubuntu配置LVM服务器"
date: 2013-08-02 00:38
comments: true
categories: [ubuntu]
---

安装好系统后，配置主机名，DNS，IP，重启生效。

>root@HOSTNAME:~# vim /etc/network/interfaces

{% highlight ruby %}
auto eth0
iface eth0 inet static
address xxx.xxx.xxx
gateway xxx.xxx.xxx
netmask xxx.xxx.xxx
{% endhighlight %}

>root@HOSTNAME:~# vim /etc/network/hosts

{% highlight ruby %}
127.0.0.1   localhost HOSTNAME
{% endhighlight %}

>root@HOSTNAME:~# vim /etc/hostname

{% highlight ruby %}
HOSTNAME
{% endhighlight %}

>root@HOSTNAME:~# vim /etc/hostname

{% highlight ruby %}
search localdomain
nameserver 114.114.114.114
nameserver 8.8.8.8
{% endhighlight %}

>root@HOSTNAME:~# /etc/init.d/networking restart

2.安装lvm2

{% highlight ruby %}
apt-get install lvm2
{% endhighlight %}

3.配置挂载磁盘为lvm

>未挂载硬盘

{% highlight ruby %}
root@HOSTNAME:~# df -h

Filesystem      Size  Used Avail Use% Mounted on
/dev/sda1        52G  1.6G   48G   4% /
udev             16G  4.0K   16G   1% /dev
tmpfs           6.3G  240K  6.3G   1% /run
none            5.0M     0  5.0M   0% /run/lock
none             16G     0   16G   0% /run/shm
{% endhighlight %}

> 可以看到有安装新硬盘 /dev/sdb, 大小 214.7 GB
{% highlight ruby %}
root@HOSTNAME:~#  fdisk -l

Disk /dev/sda: 64.4 GB, 64424509440 bytes
255 heads, 63 sectors/track, 7832 cylinders, total 125829120 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk identifier: 0x00091105

   Device Boot      Start         End      Blocks   Id  System
/dev/sda1   *        2048   109051903    54524928   83  Linux
/dev/sda2       109053950   125827071     8386561    5  Extended
/dev/sda5       109053952   125827071     8386560   82  Linux swap / Solaris

Disk /dev/sdb: 214.7 GB, 214748364800 bytes
255 heads, 63 sectors/track, 26108 cylinders, total 419430400 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk identifier: 0x00000000

Disk /dev/sdb doesn't contain a valid partition table
{% endhighlight %}

> 对 /deb/sdb 进行分区

{% highlight ruby %}
root@HOSTNAME:~# fdisk /dev/sdb

Device contains neither a valid DOS partition table, nor Sun, SGI or OSF disklabel
Building a new DOS disklabel with disk identifier 0xc3e2b567.
Changes will remain in memory only, until you decide to write them.
After that, of course, the previous content won't be recoverable.

Warning: invalid flag 0x0000 of partition table 4 will be corrected by w(rite)

Command (m for help): n \\创建新的分区
Partition type:
   p   primary (0 primary, 0 extended, 4 free)
   e   extended
Select (default p): p \\创建主分区
Partition number (1-4, default 1):
Using default value 1
First sector (2048-419430399, default 2048):
Using default value 2048
Last sector, +sectors or +size{K,M,G} (2048-419430399, default 419430399):
Using default value 419430399

Command (m for help): p \\查看当前分区

Disk /dev/sdb: 214.7 GB, 214748364800 bytes
255 heads, 63 sectors/track, 26108 cylinders, total 419430400 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk identifier: 0xc3e2b567

   Device Boot      Start         End      Blocks   Id  System
/dev/sdb1            2048   419430399   209714176   83  Linux

Command (m for help): t \\改变分区类型
Selected partition 1
Hex code (type L to list codes): 8e  \\更改为LVM类型分区
Changed system type of partition 1 to 8e (Linux LVM)

Command (m for help): w \\ 保存退出
The partition table has been altered!

Calling ioctl() to re-read partition table.
Syncing disks.

{% endhighlight %}

root@HOSTNAME:~# partprobe \\使用partprobe指令更新内核的中硬盘分区表

root@HOSTNAME:~# pvcreate /dev/sdb1 \\创建新的PV

  Physical volume "/dev/sdb1" successfully created

root@HOSTNAME:~# pvscan \\新创建的PV尚未加入任何VG组

  PV /dev/sdb1                      lvm2 [200.00 GiB]
  Total: 1 [200.00 GiB] / in use: 0 [0   ] / in no VG: 1 [200.00 GiB]

root@HOSTNAME:~# vgdisplay \\查看VG组的详细信息
  "No volume groups found"

{% highlight ruby %}

root@HOSTNAME:~# vgcreate ubuntu /dev/sdb1 \\创建卷组 'ubuntu', 挂载/dev/sdb1

Volume group "ubuntu" successfully created

root@HOSTNAME:~# lvdisplay

  --- Logical volume ---
  LV Name                /dev/ubuntu/bigdata
  VG Name                ubuntu
  LV UUID                rQJZJs-wO2V-pZTy-WcEZ-3rA6-Byo8-LheED7
  LV Write Access        read/write
  LV Status              available
  # open                 0
  LV Size                199.00 GiB
  Current LE             50944
  Segments               1
  Allocation             inherit
  Read ahead sectors     auto
  - currently set to     256
  Block device           252:0
{% endhighlight %}

> 逻辑卷组（VG）创建好了，现在在上边创建逻辑卷

root@HOSTNAME:~# lvcreate  -n bigdata -L 199G ubuntu \\在逻辑卷组

'ubuntu'（VG）上创建大小为199GB的逻辑卷（LV），其名称为bigdata
"Logical volume "bigdata" created"

{% highlight ruby %}

> 如果要删除逻辑卷,  lvremove /dev/ubuntu/bigdata

> 逻辑卷（LV）创建完毕之后，在mount之前，还可以做调整

root@HOSTNAME:~# lvremove /dev/ubuntu/bigdata \\删除逻辑卷

root@HOSTNAME:~# lvextend -L 200G /dev/ubuntu/bigdata \\ 这条命令把名为bigdata的逻辑卷（LV）的空间大小增至200G

root@HOSTNAME:~# lvreduce -L 199G /dev/ubuntu/bigdata  \\ 这条命令把bigdata的逻辑卷（LV）的空间大小缩减至199G

root@HOSTNAME:~# lvrename ubuntu/bigdata superdata \\ 这条命令把ubuntu卷组中(VG)的逻辑卷（LV）bigdata 改名为 superdata

{% endhighlight %}

接下来，就是要在逻辑卷上创建文件系统了，这个文件系统是要和挂载点的文件系统一致的.

{% highlight ruby %}

root@HOSTNAME:~# mkfs.ext4  /dev/ubuntu/bigdata

mke2fs 1.42 (29-Nov-2011)
Filesystem label=
OS type: Linux
Block size=4096 (log=2)
Fragment size=4096 (log=2)
Stride=0 blocks, Stripe width=0 blocks
13041664 inodes, 52166656 blocks
2608332 blocks (5.00%) reserved for the super user
First data block=0
Maximum filesystem blocks=4294967296
1592 block groups
32768 blocks per group, 32768 fragments per group
8192 inodes per group
Superblock backups stored on blocks:
    32768, 98304, 163840, 229376, 294912, 819200, 884736, 1605632, 2654208,
    4096000, 7962624, 11239424, 20480000, 23887872

Allocating group tables: done
Writing inode tables: done
Creating journal (32768 blocks): done
Writing superblocks and filesystem accounting information: done
{% endhighlight %}

做好之后，就可以挂载逻辑卷了.

{% highlight ruby %}

root@HOSTNAME:~# mkdir /bigdata

root@HOSTNAME:~# mount /dev/ubuntu/bigdata /bigdata

{% endhighlight %}

>考虑到服务器重启之后逻辑卷是不会自动挂载的，所以需要修改/etc/fstab文件，sudo vim /etc/fstab,增加

{% highlight ruby %}

/dev/ubuntu/bigdata /bigdata ext4 rw,noatime 0 0

{% endhighlight %}

> 如果要添加新磁盘 /dev/sdb2 到VG ubuntu组

{% highlight ruby %}

vgextend ubuntu /dev/sdb2

{% endhighlight %}

> 如果要将VG组中的free空间划出4G到/分区所在的LV
{% highlight ruby %}
root@HOSTNAME:~# lvextend -L +4G  /dev/ubuntu/bigdata

Extending logical volume root to 8.49 GiB
Logical volume root successfully resized

{% endhighlight %}

>使用resizefs2命令重新加载逻辑卷的大小才能生效

{% highlight ruby %}

resize2fs /dev/ubuntu/bigdata

{% endhighlight %}
