## Introduction
* NOTE: This install guide is about 80% done, everything in here has been tested.
* This guide will get DrFTPD+ 2.0 installed and running on FreeBSD 7.x with Diablo jdk16
* This guide will probably work on other versions of Java, FreeBSD, DrFTPD.
* The software will be installed in the home directory of the 'drftpd' user
* It will still need to be configured, but the hard bit is getting it installed in the first place!
* If you have any questions or make any contributions feel free to contact me via email or on irc.
- Enjoy

## Setup Environment - As Root

You had better know your root password :)

`su`

### Create A DrFTPD User Account

**Create The Account**

`pw useradd -n drftpd -s tcsh -w random -m`

**Set Home Dir Perms**

`cd /home`
`chown -R drftpd:drftpd drftpd`
`chmod -R 700 drftpd`

**Set DrFTPD User Shell Environment**

Drop to the user drftpd and edit the tcsh shell environment file

`su - drftpd`
`edit ~/.cshrc`
`exit`

Ensure you have the following two shell environment variables and ant_home added to your path

`setenv  ANT_HOME /usr/local/share/java/apache-ant`
`setenv  JAVA_HOME /usr/local/diablo-jdk1.6.0`
`set path = (~/bin ~/sbin /sbin /bin /usr/sbin /usr/bin /usr/games /usr/local/sbin /usr/local/bin $ANT_HOME/bin)`

## Install Required Ports

**Update The Ports Tree**

`portsnap fetch`
`portsnap extract`
`portsnap update`

## Update All Installed Ports

`cd /usr/ports/ports-mgmt/portmaster/`
`make install`
`rehash`
`portmaster -av`

## Install Required Ports

Have fun with the java port

`cd /usr/ports/archivers/unzip && make install`
`cd /usr/ports/security/md5deep && make install`
`cd /usr/ports/devel/subversion && make install`
`cd /usr/ports/java/diablo-jdk16 && make install`
`cd /usr/ports/devel/apache-ant && make install`

## Create Mtab File

**Generate /etc/mtab file**

`kldload linprocfs`
`mount -t linprocfs linprocfs /compat/linux/proc`
`cat /compat/linux/proc/mtab > /etc/mtab`
`umount /compat/linux/proc`
`kldunload linprocfs`

## Install DrFTPD+ 2.0 As DrFTPD User

Install in the home directory of drftpd as the user

`su - drftpd`

## Install DrFTPD+ 2.0

**Checkout DrFTPD+ 2.0 From SVN**

`svn checkout http://drftpd.svn.sourceforge.net/svnroot/drftpd/plus/2.0/branches/+STABLE+`
`mv +STABLE+ drftpd               `
`md5deep -r -e drftpd/ | grep -v '.svn' | awk '{print }' | sha256`
`3c93a5029197a0f3411e65e9393364c3385e050dd2d6e2d529c28714debec7e6`

**Remove Unneeded Files**

`rm drftpd/CHANGELOG.txt`
`rm drftpd/LICENSE.txt`
`rm drftpd/MAJORCHANGES.txt`
`rm drftpd/UPGRADING.txt`
`rm drftpd/genkey.bat`
`rm drftpd/master.bat`
`rm drftpd/slave.bat`
`rm drftpd/bin/InstallMaster.bat`
`rm drftpd/bin/InstallSlave.bat`
`rm drftpd/bin/UninstallMaster.bat`
`rm drftpd/bin/UninstallSlave.bat`

## Install DrFTPD 2.0 Dependencies

**Download DrFTPD 2.0 Dependencies**

`fetch http://downloads.sourceforge.net/project/drftpd/drftpd-dependencies/2.0/drftpd-2.0-deps.zip`
`unzip -d drftpd drftpd-2.0-deps.zip`
`sha256 drftpd-2.0-deps.zip `
`SHA256 (drftpd-2.0-deps.zip) = fe7cce502e176e0e795c994831e7731657a3744d60076102df909887c0be2427`

**Remove Unneeded Files**

`rm drftpd/lib/FileSystem.dll`
`rm drftpd/lib/libFileSystem.so`
`rm drftpd/lib/libwrapper.so`
`rm drftpd/lib/wrapper.jar`
`rm drftpd/lib/wrapper.dll`
`rm drftpd/bin/wrapper`
`rm drftpd/bin/wrapper.exe`

**Compile DrFTPD+ 2.0**

`cd drftpd`
`ant`
`ant compile-jfreechart`
`ant compile-jsx`
`ant compile-xstream`
`cd ../`

## Install Required Libraries

### Install libFileSystem.so

Fix The Build Script
sed -i '' "s/linux/freebsd/g" drftpd/src/se/mog/io/jni.sh
Compile libFileSystem.so
cd drftpd/src/se/mog/io/
./jni.sh
cd ../../../../../
Install libFileSystem.so
cp drftpd/src/se/mog/io/libFileSystem.so drftpd/lib/
Install Java Wrapper Files
Extract Source Code
tar zxvf drftpd/extsources/wrapper_3.1.2_src.tar.gz -C drftpd/extsources/
Fix Source File
sed -i '' "s/1.2/1.5/g" drftpd/extsources/wrapper_3.1.2_src/build.xml
sed -i '' "s/enum/yourface/g" drftpd/extsources/wrapper_3.1.2_src/src/java/org/tanukisoftware/wrapper/test/AbstractActionApp.java
Build Java Wrapper
cd drftpd/extsources/wrapper_3.1.2_src/
./build.sh
cd ../../../
Install Wrapper Files
cp drftpd/extsources/wrapper_3.1.2_src/bin/wrapper drftpd/bin/
cp drftpd/extsources/wrapper_3.1.2_src/lib/libwrapper.so drftpd/lib
cp drftpd/extsources/wrapper_3.1.2_src/lib/wrapper.jar drftpd/lib
Basic Configuration
Rename Config Files
Rename drftpd/ Config Files
cd drftpd
foreach i (`ls *conf.dist`)
  mv $i `echo $i|sed 's/\..\{4\}$//'`
end
cd ..
Rename drftpd/conf Config Files
cd drftpd/conf
foreach i (`ls *conf.dist`)
  mv $i `echo $i|sed 's/\..\{4\}$//'`
end
cd ../../
Create Logs Folder
mkdir drftpd/logs
Generate SSL Key
cd drftpd
./genkey.sh
cd ../
Ensure DrFTPD+ 2.0 Starts
Start DrFTPD+ 2.0
drftpd/master.sh start
drftpd/slave.sh start
sleep 3
drftpd/master.sh status
drftpd/slave.sh status