Note: This guide will not work 100%, Also check out [FreeBSD 7 - DrFTPD Plus Install](https://web.archive.org/web/20170303085449/http://www.drftpd.org/index.php/FreeBSD_7_-_DrFTPD_Plus_Install)

Firstly, in this document, we are going to install java, and the other necessary things that you will need.

NOTE: This entire document will prove to be a waste of time if you have linux_compat turned on in /etc/rc.conf; please uninstall the linux_base (found with pkg_info, removed with pkg_delete) and remove the linux_enable="YES" line in /etc/rc.conf and reboot


Before continuing. We will start by updating your ports collection.

If you do not have cvsup (cvsup-without-gui) installed, you will need it:

cd /usr/ports/net/cvsup-without-gui ; make install

You are going to need to make a file to let cvsup know which ports collections you wish to update. Following is an example supfile that I use for updating my ports collection, and in my case it sits in /etc, as "supfile-ports":

joe /etc/supfile-ports

Add the following to the file:

*default host=cvsup12.FreeBSD.org
*default base=/usr
*default prefix=/usr
*default release=cvs tag=.
*default delete use-rel-suffix
#*default compress
ports-base
ports-archivers
ports-benchmarks
ports-comms
ports-converters
ports-databases
ports-devel
ports-editors
ports-emulators
ports-ftp
ports-irc
ports-java
ports-lang
ports-net
ports-security
ports-shells
ports-sysutils
ports-textproc
ports-www
You may find that you don't need or want all those collections updated, if so, just comment out the ones you don't wish to update. Once you're done editting that file, save it, and exit. We will now run cvsup to update your ports collection:

cvsup -g -L 2 /etc/supfile-ports

This will take some time, depending on the speed of your machine, and your internet connection. Once it has completed, continue with the steps below.

Now we're going to grab the necessary files java and ant require to compile. These files are not fetched during the install, due to the nature of the "License Agreement".

cd /usr/ports/java/diablo-jdk15 ; make install distclean

You will be prompted as to what files need to be downloaded and where they need to be manually downloaded from.


Put said files in /usr/ports/distfiles/


Once that is done, continue with this tutorial.


Now that java is installed, try doing "java", and make sure that it actually outputs something. One error that it may output is:Java HotSpot(TM) Client VM warning: Can't detect initial thread stack location - find_vma failed. You can ignore this error, and pretty well any other error it outputs. If the command "java" works, you are ready to go onto the next step: Installing Ant, the compiling utility for various things, one of them being drftpd.

cd /usr/ports/devel/apache-ant ; make ; make install

This part could also take a little while to complete. If you didn't get laid on the first break, take this time now, to do so; or maybe shower, or go shopping.

You shouldn't have any problems installing Ant that way; if you do, go to www.apache.org, grab the tarball, and install it manually. This document does not cover how to do that, as on multiple boxes I had 0 issues installing Ant through the ports collection. Once Ant is installed, you're going to have to configure your environment so that it knows where to grab the appropriate libraries when you're compiling drftpd. If you're using sh do the following:

export ANT_HOME=/usr/local/share/java/apache-ant
export JAVA_HOME=/usr/local/diablo-jdk1.5.0
export PATH=${PATH}:${ANT_HOME}/bin
If you're using csh do the following:

setenv ANT_HOME /usr/local/share/java/apache-ant
setenv JAVA_HOME /usr/local/diablo-jdk1.5.0
set path=( $path $ANT_HOME/bin )
Now, we will add the user we want to use for drftpd, and start configuring drftpd.

adduser

Follow the prompts, it's not rocket science. Once you've successfully added your user, continue on reading.

Since we are going to try and configure everything under the user that was added, we need to su that user, like the following:

su usernameyouadded

Now, we need to grab the drftpd tarball and the dependancies zipfile:

cd ~/ ; fetch [http://internap.dl.sourceforge.net/sourceforge/drftpd/drftpd-2.0.3.tar.gz](https://web.archive.org/web/20170303085449/http://internap.dl.sourceforge.net/sourceforge/drftpd/drftpd-2.0.3.tar.gz) ; fetch [http://easynews.dl.sourceforge.net/sourceforge/drftpd/drftpd-2.0-dep.zip](https://web.archive.org/web/20170303085449/http://easynews.dl.sourceforge.net/sourceforge/drftpd/drftpd-2.0-dep.zip)

If you have unzip already installed, pass this next step. If you do not have unzip installed, you will need to install it; as root do:

cd /usr/ports/archivers/unzip/ ; make install ; rehash

tar -zxvf drftpd-2.0.3.tar.gz ; mv drftpd-2.0.3 drftpd ; cp drftpd-2.0-dep.zip drftpd/ ; cd drftpd ; unzip drftpd-2.0-dep.zip

Now onto compiling drftpd. In your drftpd root directory do:

ant

You should see a whole whack of output, with hopefully no errors. If you do get errors, try doing "ant" again, as root. I personally had no problems compiling drftpd, after following the afforementioned instructions. Once that compiling is finished, you will need to add/readd linux_enable="YES" to your /etc/rc.conf before executing drftpd. You may have to reboot for the changes to take effect.

FreeBSD Specific Notes

It is necessary to create an /etc/mtab file that contains the relative lines from /etc/fstab for your installation. Just copy the lines for the drives which you're going to use.

Good luck and enjoy!