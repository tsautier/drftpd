Plugins must be written in java. It would be fairly easy to write a plugin that executes external scripts, but traditional FTP scripts utilize the O/S to manipulate the filesystem. Therefore we have decided to write all plugins in java, we get the advantage of having a plugins that are all written in the same language and with they all use the same API's as the rest of the ftpd. It also has the advantage that the plugins are available on all platforms that DrFTPD is available on.

All available plugins listed here are included with the DrFTPD distribution (unless otherwise stated).

## DrFTPD+
A compilation of many popular mods already integrated into DrFTPD is available in a package called [DrFTPD+](https://web.archive.org/web/20170303085159/http://www.drftpd.org/index.php/Plus). You can get more information by visiting the [wiki page devoted to it](https://web.archive.org/web/20170303085159/http://www.drftpd.org/index.php/Plus).

## Available plugins
* [Archive](https://web.archive.org/web/20170303085159/http://www.drftpd.org/index.php/Archive)
* [Trial&Quota](https://web.archive.org/web/20170303085159/http://www.drftpd.org/index.php/Trial%26Quota)
* IRC sitebot using martyr
* GlftpdLog