Introduction
DrFTPD is a Distributed FTP server written in java, it's unique because it doesn't handle transfers like normal FTP servers. DrFTPD is set up with a master and a collection of file transfer slaves that handle the file transfers, you can have as many file transfer slaves as you like. Some names that could be used to describe this is ftp site merger, ftp cluster, ftp grid or multi site bnc, but the only accurate term is "distributed ftp daemon."
What is unique with DrFTPD is that it works with existing FTP client software, you can use the FTP client application you're used to and make site-to-site (FXP) transfers with normal FTP servers. The only exception to DrFTPD is with passive (PASV) mode. For this the client needs to support the PRET command. PRET is already supported in several of the most widely used FTP clients. You can often do without PASV mode unless you are behind a firewall which you don't have access to or you need to FXP with another DrFTPD server or a server which doesn't support PASV.
If you merge 10 100mbit sites, you don't get a 1gbit site but you get a 10x100mbit site. What this means is that the aggregate bandwidth is 1000 mbit but a single transfer will never go above 100mbit.

Filesystem
DrFTPD's approach to the file system and file transfers is what makes it unique. Each file can, and will, end up on a different transfer slave.

DrFTPD uses transfer slaves for all file storage and transfers, it supports but doesn't require a file transfer slave to be run locally. The master therefore uses very little bandwidth. FTP control connection, data connections for file listings and instructions to the slaves, are the only operations that consume bandwidth on the master.

The master has a filelist that keeps track of which slaves have which files and information about those files. A file can exist on multiple slaves for redundancy and more bandwidth.

When a slave is started, it gathers a filelist and sends the entire list to the master. The master merges this list with it's existing file list and makes sure that it's in-sync with it's existing file list by adding and removing files to it's own list.

Because the master doesn't have any files locally, modifications to the virtual filesystem cannot be done easily from outside of the drftpd application.

Neither the master or the slaves need root privileges. The virtual filesystem contained on the master of which slaves files reside on is the authoritative source for information about the files. Items like lastModified, size, user, and group, are all kept on the master. The slave does however require exclusive write access to the storage area, otherwise it will become unsynced with the master filelist and errors can occur.

The slave is kept thin/dumb and isn't told anything about users. It simply processes the instructions that are given to the master and knows nothing about the ftp protocol. This is an advantage as it simplifies administration of the slaves.

Comparison to traditional FTPDs
Drftpd is truly distributed and has no working comparison. Msftpd is a work in progress and follows the PRET spec that drftpd has created. Drftpd is the only working implementation of PRET.

RaidenFTPD managed to get halfway toward PRET with what it calls VLS (Virtually Linked Servers). It does a remote mount of a directory, so you can put /movies on another FTP. It's just a convenience so that the ftp client doesn't need to connect to another FTP.