## 青云部署nwr分布式虚拟机测试

* 1.先将硬盘绑定到主机（已绑定），然后将硬盘mount到主机，

```
df -T      查看主机的文件系统类型
fdisk -l      查看硬盘设备的详细信息
```

然后使用下列命令长期看硬盘使用的文件系统：

```
parted /dev/vdc
(parted) print list
```

parted命令还是很简单的。

* 2.将硬盘挂载到主机master上

出现下边错误：

```
cloud@ubuntu01:/$ sudo mount -t nfs 192.168.1.169:/mnt/poos/vms /mnt/vms/
mount: wrong fs type, bad option, bad superblock on 192.168.1.169:/mnt/poos/vms,
       missing codepage or helper program, or other error
       (for several filesystems (e.g. nfs, cifs) you might
       need a /sbin/mount.<type> helper program)
       In some cases useful info is found in syslog - try
       dmesg | tail  or so
```

解决方案：

在终端下输入安装nfs-common即可解决，安装命令为

```
sudo apt-get install nfs-common
```
