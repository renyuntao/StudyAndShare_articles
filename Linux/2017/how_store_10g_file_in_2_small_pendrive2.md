# 如何将一个10G的文件保存到两个8G的U盘中(Part 2)?
<!--
2017-05-07
--><br /><br />

在日常工作学习中，我们经常需要使用移动存储介质来存储或传递一些文件，其中，U盘应该是我们最常使用的移动存储介质了。在某些情况下，需要存储或传递的文件比较大，比我们手上的任意一个U盘的容量都要大，但要比我们手中的两个U盘的容量之和小。此时，要在U盘中存储或传递这个大文件，就需要借助一些技巧，将大文件保存到两个U盘中去。在[如何将一个10G的文件保存到两个8G的U盘中(Part 1)?](how_store_10g_file_in_2_small_pendrive1.html)一文中，介绍了通过切割大文件的方法，将大文件保存到两个U盘中，在这篇文章中，将介绍如何将两个U盘做 **[RAID 0](https://en.wikipedia.org/wiki/Standard_RAID_levels#RAID_0)** 来保存大文件。   

注意: 本文所介绍的方法皆在Ubuntu 16.04系统中测试并通过, 但是在大多数的Linux系统中，本文所介绍的方法同样适用。    

## 原理简介

**RAID 0**即是将两块以上的磁盘组合在一起，使其看起来就如同一块容量是多个磁盘之和的大磁盘。例如，将两个8G的U盘做**RAID 0**, 就会得到一个看起来容量为16G的磁盘，这样，一个10G的大文件就可以存储到这个由两个8G的U盘组成的16G的磁盘中去了。    

关于**RAID 0**的更多介绍，你可以查看[这里](https://en.wikipedia.org/wiki/Standard_RAID_levels#RAID_0)和[这里](http://baike.baidu.com/link?url=W8YHfvED1fn3SkU_BBhHdxwUV3k45QrFk3kn8Zv5dvX2YUx07D3fOVmJXxT03QyYPQuP80CYrXCOpY95gjWN2xxRkILeg0B6sU91lfq4IyG)。    

## 创建RAID 0

1. 检查系统中是否安装有**mdadm**:    

   ```bash
   $ which mdadm
   ```
   如果上面的命令没有任何输出，则表明没有安装**mdadm**, 针对不同的系统，你需要使用不同的命令来安装**mdadm**.
   
   **Ubuntu/Debian:**   

   ```bash
   $ sudo apt-get install mdadm
   ```

   **CentOS:**    

   ```bash
   $ sudo yum install mdadm
   ```
2. 将两个U盘插到电脑上, 使用**lsblk**或**fdisk -l**命令来查看这两个U盘在系统中对应的设备名，这里假设两个U盘在系统中对应的设备名为 **/dev/sdb1** 和 **/dev/sdc1**, 下面将针对这两个设备进行操作。   
   **注意: 你在进行下面的操作时，应该将设备名替换为你的系统中两个U盘对应的设备名。**    
3. 卸载U盘    
   在做**RAID 0**之前，需要先将U盘卸载:

   ```bash
   $ sudo umount /dev/sdb1 /dev/sdc1
   ```
4. 执行下面的命令将两个U盘做**RAID 0**:   
   
   ```bash
   $ sudo mdadm --create /dev/md0 --level=0 --raid-devices=2 /dev/sdb1 /dev/sdc1
   ```
   之后系统中就会多出一个设备 —— **/dev/md0**, 这个就是由两个U盘组成的**RAID 0**, 使用**sudo fdisk -l /dev/md0**命令来查看，会发现该设备的容量是两个U盘的容量之和。  
5. 保存RAID配置
   
   ```bash
   $ sudo mdadm --detail --scan > /etc/mdadm/mdadm.conf
   $ sudo DEVICE /dev/sdb1 /dev/sdc1 >> /etc/mdadm/mdadm.conf
   ```
6. 格式化/dev/md0并挂载
   
   ```bash
   sudo mkfs.xfs -f /dev/md0
   sudo mkdir /mnt/raid0
   sudo mount /dev/md0 /mnt/raid0
   ```
   这样，/dev/md0就挂载到了/mnt/raid0目录下了，此时，你就可以将一个10G的大文件保存到这个由两个U盘组成的**RAID 0**中去了.   

## 拔出U盘

将10G的大文件存储到上面创建的**RAID 0**中去后，在拔出U盘之前，需要做下面的操作:     

1. 卸载**RAID 0**
   
   ```bash
   sudo umount /dev/md0
   ```
2. 停用RAID

   ```bash
   sudo mdadm -S /dev/md0
   ```
3. 安全移除U盘

   ```bash
   sudo udisksctl power-off -b /dev/sdb
   sudo udisksctl power-off -v /dev/sdc
   ```

执行完上面的操作之后，就可以将U盘拔出了。    

当你再次将这两个U盘插到电脑上时，系统会自动对这两个U盘做RAID 0, 你需要做的只是将RAID 0设备(/dev/md0)进行挂载即可。  

## 销毁RAID 0

你可能只是做个测试，或者其它某些原因，你不再需要这个RAID 0, 那么你可以通过下面的操作来销毁上面创建的RAID 0:

1. 将做RAID 0的两个U盘插到电脑上
2. 在系统中执行下面的命令销毁RAID 0:
   
   ```bash
   sudo mdadm -S /dev/md0
   sudo mdadm --remove /dev/md0
   sudo mdadm --zero-superblock /dev/sdb1 /dev/sdc1
   ```
3. 格式化/dev/sdb1, /dev/sdc1    

   ```bash
   sudo mkfs.fat -F 32 /dev/sdb1
   sudo mkfs.fat -F 32 /dev/sdc1
   ```

执行完上面的操作之后，你的这两个U盘就又可以作为普通U盘来使用了。    

## 结束语

在一些情况下，我们需要用U盘来存储或传递一个大文件，而当这个文件的大小大于我们手上的任意一个U盘的容量大小时，我们就不能单靠一个U盘来存储或传递这个大文件了，此时可以借助多个U盘来存储或传递这个大文件了。这篇文章中介绍了通过将两个U盘做RAID 0的方法，来达到用两个U盘来存储大文件的目的；在接下来的一段时间里，我还会通过另外的一篇文章来介绍通过多个U盘来存储或传递大文件的其它方法，同时，希望你可以持续关注 **[TechForGeek](https://www.techforgeek.info)**.
