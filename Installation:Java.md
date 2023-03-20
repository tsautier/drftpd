Installing Java
Java 1.5 (aka: 5.0)
RedHat Enterprise Linux 3 (and most other linux versions)
The following document should lead you by the hand and help you install Java on your box. So hopefully you get a working install and learn a little about RH and Java while doing it.

First off you need to download java. It can be found here.

[http://java.sun.com/j2se/1.5.0/download.jsp](https://web.archive.org/web/20170303085741/http://java.sun.com/j2se/1.5.0/download.jsp)

NOTE: For master you need to get the JDK version For Slave you just need the JRE version.

Download and check the download file size to ensure that you have downloaded the full, uncorrupted software bundle.
NOTE: If you are unfamiliar with RH you can use the wget command once you get the download link. That way you can download it directly to your box vs downloading it on your local computer and uploading it to your box. Also make sure you have rights in that directory or it will not download.
You can download to any directory you choose; it does not have to be the directory where you want to install the J2SE Runtime Environment but once again make sure you have rights to that directory or it wont download.

Make sure that execute permissions are set on the self-extracting binary.
Run this command:
ex. $chmod +x jre-1_5_0_<version>-linux-i586.bin

Change directory to the location where you would like the files to be installed.
The next step installs the J2SE Runtime Environment into the current directory.

Run the self-extracting binary.
Execute the downloaded file, prepended by the path to it. For example, if the file is in the current directory, prepend it with "./" (necessary if "." is not in the PATH environment variable):
ex. $./jre-1_5_0_<version>-linux-i586.bin
The binary code license is displayed, and you are prompted to agree to its terms, type yes
The J2SE Runtime Environment files are installed in a directory called jre1.5.0_<version>

Now you need to make sure you set java in your path.
Use whatever text editor your confortable with.
ex. $nano /etc/profile
At the end of that input:
For master: PATH=$PATH:"<Location of your jdk1.5>/bin directory"
For slave: PATH=$PATH:"<Location of your jre1.5>/bin directory"

Save and close
Done :)

Debian
If you want java 1.5

sudo apt-get install sun-java5-jdk
java -verison
If you want java 1.6

sudo apt-get install sun-java6-jdk
java -version
If you want to switch java versions (if you have multiple copies installed)

sudo update-java-alternatives
Java 1.4
Freebsd
Note: DrFTPD works fine with /usr/ports/java/diablo-jdk16/, See [FreeBSD 7 - DrFTPD Plus Install](https://web.archive.org/web/20170303085741/http://www.drftpd.org/index.php/FreeBSD_7_-_DrFTPD_Plus_Install).

Installing Java from a package
To install Java, you must compile it or install a prebuilt package. Currently there are no pre-built packages for the FreeBSD 5.x tree, although you can use the older Java 1.3.1 packages for FreeBSD 4.8 (and later,) tree. These are older packages (the latest Java is 1.4.2 (although 1.5 is just around the corner, widespread use of this recent release is not yet common).

If you're using FreeBSD 4.x, and you don't mind using an older Java, check out the [FreeBSD Foundation's Java pre-built binaries](https://web.archive.org/web/20170303085741/http://www.freebsdfoundation.org/downloads/java-errata.shtml). They're .tbz's and you'll want to download them to /usr/ports/distfiles and then go into the /usr/ports/java/diablo-jdk (or /usr/ports/java/diablo-jre etc) directory and run

make install
from there so that the port builds properly.

If you want to use a newer Java, or if you're running the 5.x releases instead of the 4.x tree, you'll need to compile and install your own Java VM.

But I don't want Java 1.3.1, or I am running FreeBSD 5.x!
Sorry, you have to build it yourself. In order to built it yourself, you have to have a working Java first. So how do you do this? You install linux emulation, use the linux Java in ports, download the Java sources distributed by Sun under their SCSL scheme and build it. See instructions below.

Installing Java 1.4.2 SDK, the quick and dirty way.
NOTE THAT THERE ARE LICENSING ISSUES WITH JAVA TECHNOLOGIES FROM SUN.COM AND FreeBSD -- IT IS YOUR RESPONSIBILITY TO RECONCILE THESE AS THEY PERTAIN TO YOUR USE OF JAVA

You will need:

1. about 2 gigs free disk space. Check that you have this by running df 2. to update your ports tree -- see cvsup and Update_the_ports_tree

3. to download the following files:

bsd-jdk14-patches-6.tar.gz from [Eyesbeyond.com](https://web.archive.org/web/20170303085741/http://www.eyesbeyond.com/freebsddom/java/JDK14SCSLConfirm.html)
j2re-1_4_2_06-linux-i586.bin from [Sun's j2se JRE page](https://web.archive.org/web/20170303085741/http://java.sun.com/j2se/1.4.2/download.html)
j2sdk-1_4_2-bin-scsl.zip from [This page at sun.com](https://web.archive.org/web/20170303085741/http://www.sun.com/software/communitysource/j2se/java2/download.html)
j2sdk-1_4_2-mozilla_headers-unix.zip from [This page at sun.com](https://web.archive.org/web/20170303085741/http://www.sun.com/software/communitysource/j2se/java2/download.html)
j2sdk-1_4_2-src-scsl.zip from [This page at sun.com](https://web.archive.org/web/20170303085741/http://www.sun.com/software/communitysource/j2se/java2/download.html)
j2sdk-1_4_2_06-linux-i586.bin from [Sun's j2se JDK page](https://web.archive.org/web/20170303085741/http://java.sun.com/j2se/1.4.2/download.html)
For all the sun.com links, you'll need to login (and accept their EULA,) in order to download the files. It's free and relatively painless. If you don't agree to Sun's terms, you don't get to download the files and therefore don't get to build your own JRE/JDK/JVM. Deal with it.

4. to enable linux compatibility -- you can do this via [sysinstall](https://web.archive.org/web/20170303085741/http://www.drftpd.org/index.php?title=Sysinstall&action=edit&redlink=1) if you didn't do it on your initial install.

5. add the following line to your /etc/fstab:

linprocfs   /compat/linux/proc   linprocfs   rw   0  0
6. to run linprocfs -- you may need to do this:

samizdata# kldload linprocfs
samizdata# mount /compat/linux/proc
7. place the files you downloaded in the third step in /usr/ports/distfiles

8. go into the jdk14 dir:

samizdata# cd /usr/ports/java/jdk14
9. and install it:

samizdata# make install

Note that you don't want to make install clean because if something fails at some point during the install, the clean will wipe out your work files. Just run make install and when it's done, clean up with a make clean && make distclean after it's installed. Don't forget to use [rehash](https://web.archive.org/web/20170303085741/http://www.drftpd.org/index.php?title=Rehash&action=edit&redlink=1) after the install, and you can test to see that it's installed and working by running a java -version.

See [http://taosecurity.blogspot.com/2004/11/installing-java-on-freebsd-in-january.html](https://web.archive.org/web/20170303085741/http://taosecurity.blogspot.com/2004/11/installing-java-on-freebsd-in-january.html) for a similar method of doing this.

This guide shamelessly cut & pasted from: [[[1]](https://web.archive.org/web/20170303085741/http://bsdwiki.tehinterweb.net/index.php/Java%7Chttp://bsdwiki.tehinterweb.net/index.php/Java)]