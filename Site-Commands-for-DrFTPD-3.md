## Site Commands
This document describes the in-ftp raw commands available in DrFTPD 3.0.

What commands are available to you is largely dependent on the state of your [ftpcommands.conf](https://web.archive.org/web/20170302122242/http://www.drftpd.org/index.php?title=Ftpcommands.conf&action=edit&redlink=1) file.

NOTE: If you edit [ftpcommands.conf](https://web.archive.org/web/20170302122242/http://www.drftpd.org/index.php?title=Ftpcommands.conf&action=edit&redlink=1), you will need to do a site reload from ftp or !reload form irc.

All the commands from 2.0 function in 3.0 These may or may not exist in 2.0, however they aren't list in the 2.0 site command list in wiki

### SITE BANALL
`SITE BANALL {time in minutes} {reason}`
Bans all users for specified time and displays reason for ban

### SITE BANS
Displays list of banned users with time remaining and reason if set.

### SITE CHOWN
`SITE CHOWN [-r] {user[:{group}] {file}`

Example:  `SITE CHOWN -R drftpd:drftpd`  will change all files in existing dir and recursively to owner of drftpd in group drftpd

### SITE DEBUG
Toggles if you can see stacktraces on command errors

### SITE HELP
Displays a list of available commands

### SITE KICKALL
Kicks all logged in users

### SITE INDEXSTATUS
Displays the # of inodes in the vfs.

### SITE INDEXSTATUS FULL - displays additional statistics

```
Example output
200- max hits                 : 50
200- last search engine update: Sep 20, 2010 9:28:57 PM CST
200- inodes                   : 906003
200- backend                  : Apache Lucene ([http://lucene.apache.org](https://web.archive.org/web/20170302122242/http://lucene.apache.org/))
200- last backup              : Sep 21, 2010 1:29:07 PM CST
200- last optimization        : Sep 21, 2010 2:35:51 PM CST
200- cached inodes            : 23
200- ram usage                : 55.4KB
200- size                     : 265.2MB
```

### SITE FIXLINKS
Fixes symlinks in current directory

### SITE FIXSIZE
Recursively walks through and validates the size information of the current working directory

### SITE MASTERUPTIME
Displays Uptime of Master

### SITE RELOADPLUGIN
Syntax: `SITE RELOADPLUGIN class.path.to.plugin`

This command unloads then loads a plugin class file. For example, site reloadplugin org.drftpd.plugins.SiteBot would unload then load and execute the SiteBot plugin.

This command is equal to doing an unload then load

### SITE REBUILDINDEX
Recreate the index of the current VFS

### SITE REQDEL
### SITE SEARCHFILE
### SITE SEARCHDIR
### SITE SLAVEUPTIME
### SITE SLAVESUPTIME
### SITE STATUS
```
SITE STATUS Usage:
SITE STATUS <os, vm, memory, threads, gc, all>
SITE STATUS OS - Will show Operational System Info
SITE STATUS VM - Will show VM Info.
SITE STATUS MEMORY - Will show Memory Info.
SITE STATUS THREADS - Will show Threads Info.
SITE STATUS GC - Will show Garbage Collector Info.
SITE STATUS CLASSES - Will show Classes Info.
SITE STATUS ALL - shortcut to display ALL Info.
```

### SITE SWHO
### SITE TIME
Displays the current server time from Master

### SITE UPDATESEARCHER
### SITE VERS
Displays version of DRFTPD that is currently running.