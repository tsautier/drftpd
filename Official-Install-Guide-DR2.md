## Preparation
DR2 is no longer maintained nor supported by the developers. Please use DR3 if you want support.

Sign up to the [drftpd-discuss mailing list](https://web.archive.org/web/20140317122933/http://lists.sourceforge.net/lists/listinfo/drftpd-discuss)

If you use IRC, connect to [EFnet](https://web.archive.org/web/20140317122933/http://www.efnet.org/?module=servers) and join #drftpd

## Install java
For o/s specific information see: [Installation:Java](https://web.archive.org/web/20140317122933/http://www.drftpd.org/index.php/Installation:Java)

Generial info follows: Download and install a java development kit (JDK) on the master and all slaves. Version >=1.5(5.0) of the JDK will be needed for running a master. Version >=1.4 of the JDK will be needed for running a slave.

You can get sun's JDK here: [http://java.sun.com/j2se/downloads.html](https://web.archive.org/web/20140317122933/http://java.sun.com/j2se/downloads.html)

**Path Problems**
* *nix If you encounter problems like "master.sh: line 11: exec: java: not found", you need to add the java binary to your PATH environment variable. Edit your /etc/profile and add PATH=$PATH:/path/to/java/bin at the bottom.
* Windows If you encounter problems like "'JAVA' is not recognized as an internal or external command, operable program or batch file." You also need to add the java binary to your PATH environment variable. You can do this in WindowsXP in your System Properties under the Advanced Tab, there is a button Environment Variables, edit your PATH variable accordingly.

These are issues with your Operating System/Java Install

## Download
Download DrFTPD from [http://sourceforge.net/project/showfiles.php?group_id=56385](https://web.archive.org/web/20140317122933/http://sourceforge.net/project/showfiles.php?group_id=56385)

Unpack it to for example ~/drftpd (*nix) or c:\drftpd (win32).

You will also need the corresponding binary file pack (which is under the section drftpd dependencies)

## Compile
There is an ant build.xml file that is ready for compilation. All you need to do is install apache-ant and then run ant from the drftpd/ directory. Apache-ant is available at [http://ant.apache.org/](https://web.archive.org/web/20140317122933/http://ant.apache.org/)

## Master configuration
All configuration files are kept in the subdirectory conf

* Rename drftpd.conf.dist to drftpd.conf and edit.
Note: If you put \ in your paths, you need to escape it like this: \\. Alternatively, you can use / which doesn't need escaping and works on all platforms (both win32 and linux).

Rename/Edit all files in conf/ with the exception of wrapper-slave.conf.dist
* .dist files should become .conf files
* It is recommended you copy the .dist file to the .conf file and then edit the .conf file, this way you can revert to the default settings at any time
* .dist files should be kept at default for most installations
* Plugins: There are multiple plugins included with DrFTPD, each plugin has its own settings, instructions on the plugins are in their respective configuration files(if they have them). VERY IMPORTANT: the numbers of the plugins have to be sequential.

Example:
`plugins.2=org.drftpd.plugins.SiteBotSSL`
`plugins.3=net.sf.drftpd.event.listeners.Trial`

This will NOT work. There has to be a plugins.1=..., otherwise no plugin will be loaded. The order can be random, but numbers have to be sequential.

`plugins.2=org.drftpd.plugins.SiteBotSSL`
`plugins.1=net.sf.drftpd.event.listeners.Trial`

This works.

When finished, run master.sh/master.bat

64-bit: You need to get the correct wrapper at [http://wrapper.tanukisoftware.org/doc/english/download.jsp](https://web.archive.org/web/20140317122933/http://wrapper.tanukisoftware.org/doc/english/download.jsp). Wrapper included is for 32-bit only.
The working-dir is <installpath>/bin/ so everything is expected to be in that path!?

## Slave installation
"localslaves" how they used to be defined in 1.1.4 no longer exist. You can still use a local slave, but you must run it as a seperate process. This enables your slave to run separate from the master, which means if the slave has a problem, the master can still continue to run.

Copy the slave.zip file from your master to the server that you plan to run the slave on. Note that slave.zip must be compiled (with ant) on the same basic platform as the server that it is to be run on...

A slave only needs you to configure conf/wrapper-slave.conf and slave.conf

When finished, run slave.sh/slave.bat

[Example Slave Config](https://web.archive.org/web/20140317122933/http://www.drftpd.org/index.php/Example_Slave_Config)
[Setting up SlaveBootStrap](https://web.archive.org/web/20140317122933/http://www.drftpd.org/index.php/Setting_up_SlaveBootStrap)
[Setting up DrFTPD with wrapper](https://web.archive.org/web/20140317122933/http://www.drftpd.org/index.php/Wrapper) (Windows Service Installation)

## Finished
Now you're finished. you can now login at [ftp://drftpd:drftpd@localhost:2121](https://web.archive.org/web/20140317122933/ftp://localhost:2121/) (or the port you specified as master.port in drftpd.conf).

You can find more documentation [here](https://web.archive.org/web/20140317122933/http://www.drftpd.org/index.php/Documentation).

Sign up to the [drftpd-discuss mailing list](https://web.archive.org/web/20140317122933/http://lists.sourceforge.net/lists/listinfo/drftpd-discuss) if you didn't already. [drftpd-commits mailing list](https://web.archive.org/web/20140317122933/http://lists.sourceforge.net/lists/listinfo/drftpd-commits) may be helpful also for you java guru's who like to watch development.

## Windows Installation
Wrapper Compiled on Windows -> [http://drftpd.ultrapresence.net/wrapper-winxp.zip](https://web.archive.org/web/20140317122933/http://drftpd.ultrapresence.net/wrapper-winxp.zip)

1) Install the JDK suite (just click through all the defaults). You can get sun's JDK here: [http://java.sun.com/j2se/downloads.html](https://web.archive.org/web/20140317122933/http://java.sun.com/j2se/downloads.html) You do not need the one with netbeans, but just the standard, basic java jdk.
2) Download apache-ant, I take it and unzip it to c:\ant\, such that c:\ant\bin\ant is the ant compiler (this can be tested by going to start->run->cmd and then typing c:\ant\bin\ant, ant should fire up and give an error. ( [http://ant.apache.org/bindownload.cgi](https://web.archive.org/web/20140317122933/http://ant.apache.org/bindownload.cgi) )
3) Download and unzip drftpd, i do so to c:\drftpd\.
4) Add the following lines to wrapper.conf, wrapper-slave.conf, and wrapper-master.conf

`# Working Dir`
`wrapper.working.dir=../`
`This can go anywhere in said file.`

5) In your System Properties under the Advanced Tab, there is a button entitled "Environment Variables," edit your PATH variable to include the java directory (i.e. c:\program files\java-jdk1.5\bin), also set your JAVA_HOME to this path, but without the bin appeneded to the end (i.e. c:\program files\java-jdk1.5\)
6) Open a command prompt (start->run->cmd) and type cd c:\drftpd\ , then type c:\ant\bin\ant and it should compile drftpd. If any errors are generated, something went wrong, and drftpd most likely will not function.

## BSD Notes
If you are using *BSD, please also read [BSD Notes](https://web.archive.org/web/20140317122933/http://www.drftpd.org/index.php/BSD_Notes)
* Note: The notes are semi-useful, Also checkout [FreeBSD 7 - DrFTPD Plus Install](https://web.archive.org/web/20140317122933/http://www.drftpd.org/index.php/FreeBSD_7_-_DrFTPD_Plus_Install)