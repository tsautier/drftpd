FAQ
Q: Where can I get DrFTPD?
A: Download the latest available version from https://github.com/drftpd-ng/drftpd3.

Q: I created an user and they cant login?
A: Make sure you have a valid hostmask (ident@ip) for the user. Also check the "shutdown" section in perms.conf

Q: Where Can I post error logs?
A: https://github.com/drftpd-ng/drftpd3/issues

Q: Where is the .exe to install drftpd?
A: There isnt an .exe for drftpd read the Installation_of_DrFTPd_3 for further details.

Q: Where Can I get cool mods/plugins?
A: http://drftpd.org/forums use the board search funktion to find whatever you need. ne sure to register and login; only registred users are able to see downloadable plugins/files.

Q: What are .diff or .patch files?
A: What Is A Diff

Q: Where Can I get help?
A: First search the forum, google, ask a friend, then last resort use #drftpd on efnet.

Q: Why cant I view files to download on the forum?
A: You will need to register on the forum prior to download/posting.

Q: JRE cannot Initialize when trying to run master/slave
A: Java path not set in profile. Either add the path to profile, or make a symbolic link to java in /usr/bin

Q: Connection Refused when trying to start slave
A: Could be 2 things, The ip you are trying to connect to is wrong, or the master bind port is wrong

Q: Cannot Find drftpd.key
A: You didn't run genkey.sh (*nix) or genkey.bat (windows)

Q: Will This Work For Windows
A: Yes the only difference is the .sh commands to the service commands in windows

Q: Can you write specific commands for starting Master Slave in windows
A: No, Don't have the time or patience for windows

Q: What version of Java do I need?
A: Depends on which version of DrFTPD you run. But if you install 1.7 from http://java.sun.com/j2se/downloads.html then both master and slave will work with all versions of DrFTPD.

Q: HELP! I can't transfer files, I keep getting Failed errors!
A: drftpd.key that you created on your master _NEEDS_ to be on all your slaves too

Q: How do I enable SSL for my site?
A: Edit perms.conf

userrejectsecure !*

userrejectinsecure *

denydiruncrypted *

denydatauncrypted *

Q: Where are permissions stored?
A: Permissions are specified in perms.conf, perms.conf is a glftpd.conf style. See perms.conf


Troubleshooting
Installation
On windows I get the following error "jvm 1 | java.lang.NoClassDefFoundError
org/tanukisoftware/wrapper/WrapperSimpleApp"
Add the following to wrapper-slave.conf wrapper-master.conf and wrapper.conf (if you don't have a wrapper.conf then don't worry about this)
Working Dir
wrapper.working.dir=../
Put it at the begining of the file.
Site Usage
Why are some files appended by .conflict my ftp? What are these *.conflict files?
These files are left by the archive plugin, they come about when the same file name exists on at least two slaves, but the size varies.
fix: Try just deleting the .conflict file. The full size, regularly named file should still exist on the original slave that it was archived from.


Why are some files appended -OFFLINE?
These files are located on slaves that are currently offline. When the slave comes back online, the files will once again be available. If the slave is permanently gone, use the delslave command to permanently remove it, changing all -OFFLINE files to -MISSING files.

Why are some files appended -MISSING?
Files marked -MISSING are files which are included in the .sfv file, but are not located on the site yet for whatever reason.