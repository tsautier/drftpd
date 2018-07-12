Filesystem
DrFTPD's approach to the file system and file transfers is what makes it unique. Each file can, and will, end up on a different transfer slave.

DrFTPD uses transfer slaves for all file storage and transfers, it supports but doesn't require a file transfer slave to be run locally. The master therefore uses very little bandwidth. FTP control connection, data connections for file listings and instructions to the slaves, are the only operations that consume bandwidth on the master.

The master has a filelist that keeps track of which slaves have which files and information about those files. A file can exist on multiple slaves for redundancy and more bandwidth.

When a slave is started, it gathers a filelist and sends the entire list to the master. The master merges this list with it's existing file list and makes sure that it's in-sync with it's existing file list by adding and removing files to it's own list.

Because the master doesn't have any files locally, modifications to the virtual filesystem cannot be done easily from outside of the drftpd application.

Neither the master or the slaves need root privileges. The virtual filesystem contained on the master of which slaves files reside on is the authoritative source for information about the files. Items like lastModified, size, user, and group, are all kept on the master. The slave does however require exclusive write access to the storage area, otherwise it will become unsynced with the master filelist and errors can occur.

The slave is kept thin/dumb and isn't told anything about users. It simply processes the instructions that are given to the master and knows nothing about the ftp protocol. This is an advantage as it simplifies administration of the slaves.