# Slave roots
***

First, a little background on techniques used when multiple disks are involved. The problem with multiple disks is that you get multiple filesystems, and dividing the contents of your site across multiple roots is messy for you and inconvenient for your users.

A common solutions to this it to split your contents across the filesystems by making multiple directories such as pub1, pub2, pub3, etc. There are many more approaches which will not discuss here.

# LVM, RAID, etc
***

The most common solution to this is to use LVM or RAID that merges your partitions or devices to act like one large virtual partition. This is a good solution, as long as you never remove or lose a drive to failure.

If you need to remove a drive, you have to go through a lengthy process of moving data between the drives in the virtual disk and shrinking the filesystem, during this lengthy process, your disk volume is inaccessible.

Flowman reports that it took him 4 days to split up his 1.2tb LVM volume into 1 filesystem per disk. (Remove disk from virtual disk, move data off virtual disk onto removed disk, repeat.)

With RAID, AFAIK, you can't reduce the array.

If you have a disk crash, you risk loosing the entire virtual drive. It's more or less the equalivent of removing a platter from your harddrive and hoping that it will still work after you power it up.

# RAID5
***

RAID5 is very good, but all drives in the array need to be the same size. but you can't grow/reduce the array Although hardware raid solutions do not support growing raid arrays, you can grow raid arrays with most software raid solutions. RAID5 handles one drive breaking down without data loss at the cost of one of the drives in the array.

# One filesystem per disk
***

In my opinion, the best approach is to run one filesystem per disk. This way you can add and remove disks freely without loosing any more data than what was on the disk you removed.

# VFS
***

To solve the problem with dividing content, you use a VFS for merging the listings from several drives, so listing vfs://virtual/pub might merge the file listings from /mnt/drive{1,2,3}/site/pub and display them as one. If you use *nix you can also use symlinks instead of a vfs layer.

# Symlink script
***

This approach was partially inspired by a script that used one incoming drive and multiple archiving drives, and when the incoming drive was getting full, it moved data from incoming drive to archiving drives and symlinked the contents back so that it appeared as one volume. An often overlooked fact is that this scheme allows you to use one or more FAST incoming drives (i.e SCSI) and let the archive consist of slower and cheaper drives (IDE, Firewire). A very clever idea which is widely used.

# DrFTPD
***

The DrFTPD has a VFS-like layer (simply called RootBasket) that works like the VFS layer suggested above. When files are uploaded, it fills the drives sequentially until it gets full and then starts filling the next one.

This helps keeping data uploaded at the same time on the same drive, so that a disk failure doesn't affect one file per day for several weeks or months, but perhaps data from one week or month in case of a disk failure.