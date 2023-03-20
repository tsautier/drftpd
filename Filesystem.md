## DrFTPD Virtual Distributed Filesystem

What makes DrFTPD unique.

The DrFTPD master handles no file transfers(STOR,RETR) itself. It uses it's own VFS to keep track of files, storing file attributes such as size, modification time, ownership, checksum, etc. locally.

Because the master keeps a local list of files and their attributes, most of the communcation between the master and slaves are triggered by a transfer event. With some exceptions:

* parsing .sfv files once for checking completeness of directories.
* parsing .mp3 files for id3 tags
* parsing .zip files for .diz files
* Checking out the amount of free diskspace (this is done needlessly much at the moment, free space should be cached).
* Processing checksums (the slave does this and sends only the checksum back, not the file itself).

## Slaves

The slaves are actually rather dumb, doing only what the master tells them to. They have no knowledge of file ownership or users whatsoever, even if the underlying filesystem would support file ownership. This has the advantage of not requiring any root or administrator priviledges, running the master & slave servers as an unpriviledged user is encouraged and will not bring any side-effects.

## Uploads

When a file is uploaded to DrFTPD, a file entry is created in the VFS and the master instructs a slave to receive the transfer. Which slave is used depends on [SlaveSelection](https://web.archive.org/web/20170303084904/http://www.drftpd.org/index.php/SlaveSelection).

## Syncing/Merging

When a slave is started, it gathers a list of all files on it for synchronization with the master. The slave only performs synchronization with the master at startup. Because no notification is sent to the master when changes are made to the slave file storage, the slave is brought out of sync when local changes are performed to the slave file storage. This is bad and will cause problems if you do it!! You have been warned.

## Slave VFS

The slave contains a very simple vfs-like filesystem to distribute files over several root, you can see this as a kind of application level RAID-0.

Each file goes to a different root, this has the advantage that in the case of hardware failure, you can just remove the disk without loosing your multi-terrabye RAID-0 volume.

The volumes are filled in order, filling the first up to a certain amount first, then filling the next volume, and so on, until it finally comes back to the first volume again.

This is as of yet not implemented and the volume with most free space is always used. Possibly SlaveRootSelection(like SlaveSelection) could be used on the slave side.