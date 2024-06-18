# Java开发-面试题-0007-GPT和MBR的区别

**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**



问题描述：最近由于数据库存储数据路径所在的磁盘即将满了所以需要扩容，然后发现了一个问题，就是数据库存储路径所在的磁盘是MBR格式的，目前数据量已经达到2TB的最大容量了，虽然扩容达到了2.5T但是由于MBR磁盘格式的最大容量为2TB的限制，导致有0.5TB的磁盘容量没法利用上。

重点：**MBR磁盘格式大小最多管理2TB的磁盘。**

解决方案：将磁盘替换成GPT格式（做好数据迁移备份，整到GPT磁盘格式的磁盘上）



```shell
[root@0004 bin]# df -hT
Filesystem                 Type      Size  Used Avail Use% Mounted on
devtmpfs                   devtmpfs  7.5G     0  7.5G   0% /dev
tmpfs                      tmpfs     7.7G   13M  7.7G   1% /dev/shm
tmpfs                      tmpfs     7.7G   17M  7.7G   1% /run
tmpfs                      tmpfs     7.7G     0  7.7G   0% /sys/fs/cgroup
/dev/mapper/rootvg-lv_root xfs        80G   12G   69G  15% /
tmpfs                      tmpfs     7.7G   20M  7.7G   1% /tmp
/dev/vdb1                  xfs       2.0T  2.0T  1.1G 100% /data
/dev/vda1                  vfat     1022M  5.8M 1017M   1% /boot/efi
tmpfs                      tmpfs     1.6G     0  1.6G   0% /run/user/0


[postgres@0004 bin]$ lsblk -f
NAME               FSTYPE      LABEL UUID                                   FSAVAIL FSUSE% MOUNTPOINT
sr0                                                                                        
vda                                                                                        
├─vda1             vfat              9E3B-C6CA                              1016.3M     1% /boot/efi
└─vda2             LVM2_member       86S1TU-9eNq-9MSn-PU31-4em1-pwce-Q7f2BJ                
  ├─rootvg-lv_root xfs               43f9f569-e454-41f0-8bb8-7c3ecc132f6d     68.4G    14% /
  └─rootvg-lv_swap swap              ac03572e-0999-4914-8ef9-62dc49f05f1a                  [SWAP]
vdb                                                                                        
└─vdb1             xfs               e81800dd-92f5-45c1-8d90-eab18af8573a      1.1G   100% /data


[root@0004 bin]# blkid /dev/vdb1
/dev/vdb1: UUID="e81800dd-92f5-45c1-8d90-eab18af8573a" TYPE="xfs" PARTUUID="30e0c1bd-01"

[root@0004 bin]# mount | grep  /dev/vdb1
/dev/vdb1 on /data type xfs (rw,relatime,attr2,inode64,noquota)

[root@0004 bin]#  file -s /dev/vdb1
/dev/vdb1: SGI XFS filesystem data (blksz 4096, inosz 512, v2 dirs)

```





**GPT（GUID分区表） (GUID Partition Table) 和MBR（主引导记录） (Master Boot Record)**是两种用于管理硬盘和SSD等存储设备数据存储的分区方案。以下是它们的主要区别：

1. **分区限制**：
   - **MBR**：最多支持4个主分区。为克服这一限制，可以将其中一个主分区设为扩展分区，扩展分区可以包含额外的逻辑分区。
   - **GPT**：理论上支持无限数量的分区，实际限制在大多数系统上通常为128个分区。
2. **磁盘大小支持**：
   - **MBR**：使用32位寻址，**最多管理2 TB的磁盘**。
   - **GPT**：使用64位寻址，可管理超过2 TB的磁盘。
3. **数据冗余**：
   - **MBR**：在单个位置存储分区和引导数据。如果这些数据损坏，分区表会受影响。
   - **GPT**：在磁盘上存储多个分区和引导数据副本，提高了数据完整性和恢复能力。
4. **兼容性**：
   - **MBR**：与大多数旧系统和软件兼容，包括BIOS。
   - **GPT**：需要UEFI引导，大多数现代系统都支持。在不用于引导时，向后兼容BIOS。
5. **引导数据**：
   - **MBR**：在第一个扇区包含少量可执行代码，称为引导加载程序。
   - **GPT**：不以相同方式包含可执行代码；UEFI处理引导过程。
6. **操作系统支持**：
   - **MBR**：几乎所有操作系统都支持，包括较旧的操作系统。
   - **GPT**：大多数现代操作系统都支持，包括Windows（从Vista开始）、macOS和Linux。











![](https://github.com/CodeZeng1998/Java-Developer-Work-Note/blob/main/Interview/image/0007.png?raw=true)

上图由 Pic 生成

关键词：Beach by the seaside, sunset, snow capped mountains





**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**