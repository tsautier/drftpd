> Applies to DrFTPD 4.0.x.

# DrFTPD 4.x Wiki

DrFTPD is a distributed FTP daemon written in Java. One **master** handles FTP clients, users, permissions and the virtual file system. Any number of **slaves** store the files and do all the data transfers. A client talks to the master with an ordinary FTP client, and the master tells the slaves what to send or receive. Because the master does not carry file data, you can merge several sites into one: ten 1 Gbit slaves give you 10 Gbit of aggregate bandwidth, although a single transfer never goes above the speed of one slave.

This wiki documents DrFTPD **4.0.x** only. Pages about DrFTPD 2.0 and 3 are kept for history, see [Legacy Pages](Legacy-Pages). They do not apply to 4.x.

## Start here

1. [Introduction](Introduction): how master and slaves fit together
2. [Requirements](Requirements): JDK 17, Maven, git, mediainfo
3. Install on [Linux](Installation-Linux) or [Windows](Installation-Windows), or use [Docker](Docker)
4. [First Login](First-Login): default account and password change
5. [Adding Slaves](Adding-Slaves): connect your first slave
6. [SSL/TLS](SSL-TLS) and [Firewalls and Passive Mode](Firewalls-and-Passive-Mode)

## Reference

- [Master Configuration](Master-Configuration) and [Slave Configuration](Slave-Configuration)
- [Site Commands](Site-Commands), [IRC Commands](IRC-Commands), [Permissions](Permissions)
- [Users and Groups](Users-and-Groups), [Sections](Sections), [Log Levels](Log-Levels)
- [Plugins](Plugins) overview. Zipscript, sitebot and the other plugins are listed in the sidebar.

## Operations

- [Upgrading](Upgrading), [Building from Source](Building-from-Source), [FAQ](FAQ)

## Getting help

See [Getting Help](Getting-Help): IRC on EFnet `#drftpd`, and the GitHub issue tracker.
