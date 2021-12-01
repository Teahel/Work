inode节点中，记录了文件的类型、大小、权限、所有者、文件连接的数目、创建时间与更新时间等重要的信息，还有一个比较重要的内容就是指向数据块的指针。

一般情况不需要特殊配置，如果存放文件很多，需要配置。有时磁盘空间有剩余但是不能存放文件，可能是由于inode节点数量不做了。

查看inode的数量：
```
[root@wap6 change]# df -i
Filesystem            Inodes   IUsed   IFree IUse% Mounted on
/dev/mapper/VolGroup-lv_root
                     2990080  207511 2782569    7% /
tmpfs                 496727       3  496724    1% /dev/shm
/dev/xvda1            128016      44  127972    1% /boot
/dev/xvdb1           5242880 5242880       0  100% /home/de
```
可以看到每个分区的inode总数量，已使用的，空闲的。

如果需要调整inode节点的数量需要进行以下几步：

```
1、卸载文件系统 
umount /dev/xvdb1
2、建立文件系统，指定inode节点数 
mkfs.ext4 /dev/xvdb1 -N 18276352 
3、修改fstab文件 
vi /etc/fstab 
/dev/sda6               /data0                  ext3    defaults        1 2 
4、挂载文件系统 
mount -a 
5、查看修改后的inode参数 
dumpe2fs -h /dev/xvdb1 | grep node 
```
[注意]调整inode数会格式化磁盘，执行前应确定磁盘上没有重要数据或是先备份数据　
