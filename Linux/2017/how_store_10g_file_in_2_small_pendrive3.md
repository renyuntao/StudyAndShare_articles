## 如何将一个10G的文件保存到两个8G的U盘中(Part 3)?
<!--
2017-05-27
-->

在日常的工作学习中，我们经常需要使用移动存储介质来存储或在设备之间传递一些文件, 其中，U盘应该是我们最长使用的移动存储介质了。在某些情况下，需要存储或传递的文件比较大，比我们手上的任意一个U盘的容量都要大，但要比我们手中的两个U盘的容量之和小。 此时，我们可以借助一些技巧，将要传递的大文件保存到这两个U盘中去。   

在之前的两篇文章[如何将一个10G的文件保存到两个8G的U盘中(Part 1)](how_store_10g_file_in_2_small_pendrive1.html)和[如何将一个10G的文件保存到两个8G的U盘中(Part 2)](how_store_10g_file_in_2_small_pendrive2.html)中，我们已经介绍了通过split割大文件和将两个U盘做RAID 0的方法，实现了将一个10G的文件保存到两个8G的U盘中的目的。在这篇文章中，将通过另外的一种方法 —— LVM —— 来实现同样的目的。

注意: 本文所介绍的方法皆在Ubuntu 16.04系统中测试并通过, 但是在大多数的Linux系统中，本文所介绍的方法同样适用。  

## 原理简介

LVM是Logical Volume Manager(逻辑卷管理员)的简写, 它是Linux环境下对磁盘分区进行管理的一种机制, 通过LVM, 可以将多个分区或磁盘组合成一个逻辑大磁盘。因此，我们可以通过LVM将两个8G的U盘组合成一个逻辑上的磁盘，从而能够存储10G的大文件(这一点和RAID 0有些相似)。    

LVM涉及到几个比较重要的概念: **PV**, **PE**, **VG**, **LV**. 简单地说, **PV**相当于普通磁盘上的分区，**PE**类似于文件系统里面的block, **VG**就是LVM组合起来的大磁盘, 而**LV**则相当于**VG**这个大磁盘上的分区。关于这几个概念的详细介绍, 你可以查看[这里](http://baike.baidu.com/link?url=5CqZ4qmdIDms2nIr0kK5K7UZJkrqGSWn9iaJfHWpFiHxwjER9mNdo4NSyUXjJfC_ypmAJ77AWiR1Y9Jr4ivNga)和[这里](https://en.wikipedia.org/wiki/Logical_Volume_Manager_(Linux)).   

## 操作流程

1. 检查系统中是否安装有**lvm2**:  
   
   ```bash
   which pvcreate
   ```
   如果上面的命令没有任何输出，则表明没有安装lvm2, 针对不同的系统，你需要使用不同的命令来安装mdadm.    

   **Debian/Ubuntu:**   

   ```bash
   sudo apt-get install lvm2
   ```

   **CentOS:**   

   ```bash
   yum install lvm2
   ```

2. 将两个U盘插到电脑上, 使用lsblk或fdisk -l命令来查看这两个U盘在系统中对应的设备名，这里假设两个U盘在系统中对应的设备名为 **/dev/sdb** 和 **/dev/sdc**, 磁盘分区分别为**/dev/sdb1**和**/dev/sdc1**,下面将针对这两个设备和分区进行操作。   
   **注意: 你在进行下面的操作时，应该将设备名和分区替换为你的系统中两个U盘对应的设备名和分区**   

3. 卸载U盘
   在继续指向下面的操作之前，需要先将U盘卸载:   

   ```bash
   $ sudo umount /dev/sdb1 /dev/sdc1
   ```

4. 查看U盘的分区表格式  
   
   ```bash
   $ sudo parted /dev/sdb print
   $ sudo parted /dev/sdc print
   ```
   之后, 会显示类似下面的输出:   

   ```
   磁盘 /dev/sdb: 7969MB
   Sector size (logical/physical): 512B/512B
   分区表：msdos
   Disk Flags:
   ...
   ```
   输出结果中, 如果分区表的值是**msdos**, 则U盘为MRB分区表, 如果分区表的值是**gpt**, 则U盘是**GPT**分区表.   

5. 修改U盘分区的System ID
   如果你的U盘是MRB分区表, 则使用fdisk来修改U盘的System ID; 如果你的U盘是**GPT**分区表, 则使用gdisk来修改U盘的System ID. 由于当前的U盘大多是MBR分区表, 因此下面将使用fdisk来修改U盘的System ID, 下面以修改**/dev/sdb1**这个分区的System ID为例, 首先执行下面的命令进入到fdisk的交互模式:   

   ```bash
   $ sudo fdisk /dev/sdb
   ```
   之后按下**t**键来告诉fdisk要修改U盘分区的System ID, 之后fdisk会提示你输入System ID, 由于LVM的System ID表示为8e, 因此，你只要输入**8e**并按回车即可; 接下来按下**w**键保存退出。   
   **/dev/sdc1**的操作方法同上。   

6. PV阶段
   执行如下命令创建PV:   

   ```bash
   sudo pvcreate -y /dev/sd{b1,c1}
   ```
   这样就创建了两个PV: /dev/sdb1和/dev/sdc1

7. VG 阶段   
   执行下面的命令创建VG   

   ```bash
   sudo vgcreate myVG /dev/sd{b1,c1}
   ```
   上面的命令创建了一个包含两个PV(/dev/sdb1和/dev/sdc1)的名为myVG的逻辑磁盘.   

8. LV阶段  
   执行下面的命令创建LV:   

   ```bash
   sudo lvcreate --size 14G --name myLV myVG
   ```
   上面的命令在VG这个逻辑磁盘上创建了一个名为myLV的分区, 该分区的大小是14G.  

9. 格式化   
   同普通磁盘一样, 创建好分区以后，需要格式化才能使用, 下面对上一步创建的LV分区进行格式化:    

   ```bash
   $ sudo mkfs.xfs /dev/myVG/myLV
   ```

10. 挂载   
    要使用这个分区, 首先要将这个分区挂载到一个目录下:   

	```bash
	$ sudo mkdir /mnt/lvm
	$ sudo mount /dev/myVG/myLV /mnt/lvm
	```
	上面的命令首先在/mnt/目录下创建了一个子目录lvm, 之后将LV分区挂载到了该目录下.   

完成了上面的操作之后, 你就可以将10G的大文件放到/mnt/lvm/目录下, 从而保存到这两个U盘中去了.   

## 拔出U盘

当你将文件保存到这两个U盘中去, 在你拔出U盘之前，需要作如下操作:   

1. 卸载文件系统
   
   ```bash
   sudo umount /mnt/lvm
   ```

2. 将LV,VG设置为关闭状态

   ```bash
   $ sudo lvchange -an /dev/myVG/myLV
   $ sudo vgchagne -an myVG
   ```

3. 安全移除U盘

   ```bash
   $ sudo udisksctl power-off -b /dev/sdb
   $ sudo udisksctl power-off -b /dev/sdc
   ```

完成上面的3步操作后, 即可将U盘拔出了。   

## 重新挂载LV分区

当想要将上面创建的LV分区重新挂载到电脑上时, 只需要重新将这两个U盘插入到系统中, 并执行下面的命令进行挂在即可:   

```bash
$ sudo mount /dev/myVG/myLV /mnt/lvm
```

## LVM的关闭

你可能只是做个测试，或者其它某些原因，你不再需要这个LVM, 那么你可以通过下面的操作来销毁上面创建的LVM文件系统:   

1. 卸载上面创建的LVM文件系统
   
   ```bash
   $ sudo umount /mnt/lvm
   ```

2. 使用lvremove移除LV   

   ```bash
   $ sudo lvremove /dev/myVG/myLV
   ```

3. 使用vgchange关闭VG   

   ```bash
   $ sudo vgchange -an myVG
   ```

4. 使用vgremove移除VG
   
   ```bash
   $ sudo vgremove myVG
   ```

5. 使用pvremove移除PV
   
   ```bash
   $ sudo pvremove /dev/sd{b1,c1}
   ```

6. 最后使用fdisk将U盘分区的System ID修改回来
7. 重新格式化U盘的分区   

完成上面的操作后, 你就又可以将这两个U盘当作普通的U盘来使用了.    

## 结束语

在一些情况下，我们需要用U盘来存储或传递一个大文件，而当这个文件的大小大于我们手上的任意一个U盘的容量大小时，我们就不能单靠一个U盘来存储或传递这个大文件了，此时就需要借助多个U盘来存储或传递这个大文件了。这篇文章中介绍了如果通过LVM来使用两个8G的U盘来存储一个10G的文件, 同时，这篇文章也是 **如何将一个10G的文件保存到两个8G的U盘中?** 系列的最后一片文章, 你可以通过下面的链接来查看之前的两篇文章.    

[如何将一个10G的文件保存到两个8G的U盘中(Part 1)?](https://www.techforgeek.info/how_store_10g_file_in_2_small_pendrive1.html)     
[如何将一个10G的文件保存到两个8G的U盘中(Part 2)?](https://www.techforgeek.info/how_store_10g_file_in_2_small_pendrive2.html)   
