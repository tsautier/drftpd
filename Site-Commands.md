## Site Commands

This document describes the in-ftp raw commands available in DrFTPD 2.0. What commands are available to you is largely dependent on the state of your [commandhandlers.conf](https://web.archive.org/web/20170301173731/http://www.drftpd.org/index.php?title=Commandhandlers.conf&action=edit&redlink=1) file. NOTE: If you edit [commandhandlers.conf](https://web.archive.org/web/20170301173731/http://www.drftpd.org/index.php?title=Commandhandlers.conf&action=edit&redlink=1), you will need to restart your master process for those changes to take effect.

SITE ADDIP
Syntax: SITE ADDIP <user> <ident@ip#1 ... ident@ip#10>

Example: SITE ADDIP drftpd drftpd@127.0.0.1 *@10.10.*

This would add the ident@ip drftpd@127.0.0.1 and mask *@10.10.* to user drftpd.

Note: if you don't know what ident/ipmask is, you need to read your internet-for-dummies book first.

SITE ADDSLAVE
Syntax: SITE ADDSLAVE <slavename>

Adds a slave. example: SITE ADDSLAVE drftpd

This would add the slave 'drftpd' (see [Site_Commands#SITE_SLAVE](https://web.archive.org/web/20170301173731/http://www.drftpd.org/index.php/Site_Commands#SITE_SLAVE))

SITE ADDUSER
Syntax: SITE ADDUSER <user> <password> [<ident@ip#1> ... <ident@ip#5>]

Adds a user. You can have wild cards for users that have dynamic ips Examples: *@192.168.1.* , frank@192.168.*.* , bob@192.*.*.*

example: SITE ADDUSER drftpd mypassword *@127.0.0.1

This would add the user 'drftpd' with the password 'mypassword' add the ip mask '*@127.0.0.1'. (see [Site_Commands#SITE_ADDIP](https://web.archive.org/web/20170301173731/http://www.drftpd.org/index.php/Site_Commands#SITE_ADDIP))

SITE BAN
Syntax: SITE BAN [username] [minutes] [reason]. Bans the specified user for a given number of minutes, a reason can also optionally be supplied.

example: SITE BAN todd 30 just.for.kicks

SITE BLOWFISH
Syntax: SITE BLOWFISH [#channelName] This returns the defined blowfish key in irc.conf if the SiteBot is loaded and the user has permissions to view the key. (irc.channel.#.perms=)

SITE CHANGE
Syntax: SITE CHANGE <user> <arguments> ...

site change allows changing variables for the user.

SITE CHANGE wkly_allotment
Syntax: site change <user> wkly_allotment <credits>

The amount of credits that this user will be given once a week. User's credits are replaced, not added to.

SITE CHANGE group
Syntax: SITE CHANGE <user> GROUP <primary-group>

Changes the primary group of the user. Primary group is used in file ownership and sitebot announcements.

SITE CHANGE group_slots
Syntax: SITE CHANGE <user> group_slots <slots> <leech slots>

Number of users a gadmin is allowed to add. If you specify a second argument, it will be the number of leech accounts the gadmin can give (done by "site change user ratio 0")

Note: The number of slots includes leech slots, so if you'd like to set 2 leech and 2 ratio slots for user 'bob', you would issue the command "site change bob group_slots 4 2"

SITE CHANGE num_logins
Syntax: SITE CHANGE <user> num_logins <num_logins> [num_logins_ip]

SITE CHANGE ratio
Syntax: SITE CHANGE <user> ratio <value>

Upload/Download ratio. 0 = Unlimited (Leech)

SITE CHANGE credits
Syntax: SITE CHANGE <user> credits <credits>

Credits left to download.

FXP commands:

A. General command (1 Giga = 1000000000): SITE CHANGE %d[user] credits %d[credits]

B. Change credits to 100G: SITE CHANGE %d[user name] credits 100000000000

C. Change credits to 200G: SITE CHANGE %d[user name] credits 200000000000

SITE CHANGE max_sim
Syntax: SITE CHANGE <user> max_sim <max_sim_down> <max_sim_up>

SITE CHGRP
SYNTAX: site chgrp <user> <group> [<group> ...]

to add secondary groups to a user (add group again to remove)

SITE CHOWN
Syntax: SITE CHOWN <user>[.<group>] <file>

Changes ownership of a file/directory.

Example: SITE CHOWN drftpd.drftpd music

Sets ownership of file/folder music to user and group drftpd.

SITE CHPASS
Syntax: SITE CHPASS <user> <password>

ex. SITE CHPASS drftpd newpassword

This would change the password to 'newpassword' for the user 'Drftpd'

FXP command: SITE CHPASS %d[user] %d[new pass]

SITE DELIP
Syntax: SITE DELIP <user> <ident@ip> ...

ex. SITE DELIP Drftpd Drftpd@127.0.0.1

This would remove the ident@ip 'Drftpd@127.0.0.1' from the user 'Drftpd'.

SITE DELSLAVE
Syntax: SITE DELSLAVE <slavename>

Deletes the specified slave and removes all the file references from the master. Any files that were on the slave have their references removed in the master's VFS (VirtualFileSystem). This does not delete the files from the slave root's.

SITE DELUSER
Syntax: SITE DELUSER <user>

Delete a user, may be readded with site READD.

SITE FIND
Syntax: SITE FIND -help for details

Example. To search incomplete folders use SITE FIND -incomplete

Example. To search filenames by keyword use -name *word1*

This searches "word1" in all folders whereas "SITE SEARCH word1" would only search folder titles.

SITE GADDUSER
usage: site gadduser <group> <username> <pass> <ident>@<host>

Adds a user to the site, and puts them in the specified group.

eg: site gadduser drGRP drUSER drPASS drIDENT@*.myisp.com

FXP command: site gadduser %d[Group] %d[username] %d[password] *@*

SITE GINFO
Syntax: site ginfo <groupname>

Displays information about and lists users in the specified group.

SITE GIVE
Syntax: SITE give <user> <bytes>

Gives credits to user.

SITE GROUPS
Shows available groups.

SITE IRC connect
Syntax: site irc connect Connect the bot to irc after disconnect

SITE IRC disconnect
Syntax: site irc disconnect Disconnect the bot from irc

SITE IRC reconnect
Syntax: site irc reconnect Reconnect the bot to irc

SITE INVITE irc
Syntax: site invite <username> Invite you into IRC channels

SITE KICK
Syntax: SITE KICK <user>

Kick a user off the site.

ex. SITE KICK Drftpd

This will kill all connections for the user 'Drftpd'


SITE KICK <all/*>
Kick all user off the site.

ex. SITE KICK all

ex2. SITE KICK *

This will kill all connections to the ftp

SITE LIST
This command will list files in a unique DrFTPD type fashion. It includes embedded virtual file system information like the list of slaves that the file resides on. These things are not available through a normal LIST command.

SITE LINK
This command will create symlink to path

Syntax : site link /dir/path/ symlinkname

SITE LOADPLUGIN
Syntax: SITE LOADPLUGIN class.path.to.plugin

This command loads a plugin class file. For example, site loadplugin org.drftpd.plugins.SiteBot would load and execute the SiteBot plugin.

SITE NUKE
Nukes a directory. example : site nuke <directory> <multiply> <reason>

FXP command: site nuke %f %d[multiply] %d[reason] (nuke selected folder)

SITE NUKES
Lists all nuked directories on site.

SITE PASSWD
Syntax: SITE PASSWD <password>

Change your password.

ex. SITE PASSWD newpass

This will change your password to 'newpass'.

SITE PLUGINS
Syntax: SITE PLUGINS

Prints a list of all running plugins in the order of which they were loaded. First plugin executes its code first, then second, third, ...

SITE PRE
Syntax: SITE PRE <directory> <target>

Example: SITE PRE DrFTPD-1.0 java

This would move the directory named DrFTPD-1.0 in the current directory to /java and send a PRE event that is announced by the sitebot (IRCListener) plugin.

The <target> argument is just a pathname with a / added at the beginning.

SITE PURGE
Syntax: SITE PURGE <username>

Remove deleted users, site [Site_Commands#READD](https://web.archive.org/web/20170301173731/http://www.drftpd.org/index.php/Site_Commands#READD) will NOT work on purged users.

example: SITE PURGE frank

This will remove just frank and leave the rest of deleted users on site

SITE READD
Syntax: SITE READD <user>

Readd user.

ex. SITE READD Drftpd

This will remove the user drftpd from deleted group.

SITE RELOAD
Reload Config files. Useful when changing perms.conf (no restart required)

SITE REMERGE
Syntax: SITE REMERGE [SLAVE]

Rebuilds the file database of the specified slave.

SITE RENUSER
Syntax: site renuser <old_username> <new_username>

Rename a user.

SITE RESCAN
Syntax: SITE RESCAN [force]

Verifies all files against .sfv file.

If the force argument is present, all checksums are re-read from the slave(s).

SITE REQUEST
Syntax: SITE REQUEST [releasename]

If you are in a directory that allows requests (see perms.conf), this will create a folder with the given release name.

Example: SITE REQUEST My.Release.Something-MyGroup

This will create the folder REQUEST-by.username-My.Release.Something-MyGroup

SITE REQFILLED
This site command is used to Fill a Request. Directory's name is now FILLED- instead of REQUEST-

SITE RULES
Syntax: site rules Shows Rules

SITE SEEN
Syntax: SITE SEEN <username>

Shows when the specified user was last seen.

SITE SHUTDOWN
Gracefully shuts down all of the slaves and then the master

SITE SLAVE
Syntax: SITE SLAVE <slavename>

This will display information and settings about the slave

SITE SLAVE addmask
Syntax: SITE SLAVE <slavename> addmask ip@ident

SITE SLAVE delmask
Syntax: SITE SLAVE <slavename> delmask ip@ident

SITE SLAVE set
Syntax: SITE SLAVE <slavename> set <key> <value>

This will set any number of dynamic data settings in the slave Note: settings are documented with their respective usages

SITE SLAVES
Will display a list of all configured slaves, and their connection status

SITE TAGLINE
Syntax: site tagline <new tagline>

This will set your tagline, which can be seen in upload/download stats commands

SITE TAKE
Takes the specified amount of credits and adds them to your account

SITE UNBAN
Syntax: SITE UNBAN [username]

Unbans a given user

SITE UNLOADPLUGIN
Syntax: SITE UNPLUGIN class.path.to.plugin

This command unloads a plugin class file. For example, site unloadplugin org.drftpd.plugins.SiteBot would unload the SiteBot plugin.

SITE UNNUKE
UNukes a directory.

SITE USER
Syntax: SITE USER [username]

Displays detailed information for a user account

SITE USERS
Displays a list of all user accounts on the site

SITE WHO
Show's a list of everyone who is currently connected to the site, and their current or previous action like download, upload, idle ...

SITE WIPE
Syntax: SITE WIPE [-r] <file/dir>

Deletes files without removing credits. -r means delete recursively.

FXP command: SITE WIPE -r %f (wipe selected folders)