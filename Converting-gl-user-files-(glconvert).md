Convert your existing glftpd users to drftpd 1.x. This feature has been removed from drftpd 2.x. Convert your glftpd users to drftpd 1.x and then upgrade your users to drftpd 2.x.

First copy the glftpdconvert.sh to /drftpd out of the /drftpd/bin directory.

Now edit glftpdconvert.sh with your favorite text editor.

There are three settings here is a basic outline of them

export VMARGS="

-Dglftpd.users=/glftpd/ftp-data/users \ # Where the glftpd users dir is

-Dglftpd.root=/glftpd \ # Where glftpd is installed

-Dglftpd.passwd=/glftpd/etc/passwd" # Where the passwd file is

If glftpd is installed in /glftpd these settings should be fine.

Before running glftpdconvert.sh be sure to purge all deleted users on glftpd or you may get an error. Also move all default.* files from /glftpd/ftp-data/users/ or you may get an error again. Now run ./glftpdconvert.sh and it should add all your glftpd users to /drftpd/users.