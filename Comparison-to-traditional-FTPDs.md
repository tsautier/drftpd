# Editing Comparison to traditional FTPDs

Drftpd is truly distributed and has no working comparison. Msftpd is a work in progress and follows the PRET spec that drftpd has created. Drftpd is the only working implementation of PRET.

RaidenFTPD managed to get halfway toward PRET with what it calls VLS (Virtually Linked Servers). It does a remote mount of a directory, so you can put /movies on another FTP. It's just a convenience so that the ftp client doesn't need to connect to another FTP.