## Howto

Setup ANT and JAVA as in described in [Official Install Guide 3](https://web.archive.org/web/20111109023543/http://www.drftpd.org/index.php/Official_Install_Guide_3)

```
svn checkout http://drftpd.svn.sourceforge.net/svnroot/drftpd/trunk
cd trunk
wget http://downloads.sourceforge.net/project/drftpd/drftpd-dependencies/3.0/drftpd-3.0beta-deps.zip?use_mirror=dfn
unzip drftpd-3.0beta-deps.zip
rm drftpd-3.0beta-deps.zip
./genkey.sh
./build.sh
```

Now edit all the `.conf` files

## Plugins

Plugins and code modifications of DrFTPD versions prior 3.0.0 will not work. You need to use plugins designed for version 3.0.0.
Unofficial plugins can be found here [http://drftpd.org/forums/viewforum.php?f=26](https://web.archive.org/web/20111109023543/http://drftpd.org/forums/viewforum.php?f=26)
Please see [Installing Plugins](https://web.archive.org/web/20111109023543/http://www.drftpd.org/index.php/Official_Install_Guide_3#Plugins) for installation instructions.

## FAQ

**java.lang.Error: failed instanciating SAX parser**

You did not setup JAVA and ANT correctly. Check if ANT and JAVA commands execute the right Java version

**org.drftpd.exceptions.SSLUnavailableException: Secure connections to slave required but SSL isn't available**

You did not setup JAVA correctly. Check if the KEYTOOL is the one from your used Java version and if you generated and copyed the drftpd.key

**Exception in thread "main" java.lang.UnsatisfiedLinkError: /..../libTerminal.so**

You are likely trying to build on a x64 OS. Basically you will need to replace wrapper with a 64bit version, and java sdk 64bit .... and libTerminal.so has to be compiled for 64bit aswell. Search on libTerminal.so where you can test a few versions that people have uploaded on the forum (or compile your own).