# Basic Install Instructions

## DOWNLOADING

Download the latest available version from [http://sf.net/projects/drftpd/](https://web.archive.org/web/20170804022954/http://sf.net/projects/drftpd/) or the latest stable branch in our [Subversion Repository](https://web.archive.org/web/20170804022954/http://www.drftpd.org/index.php?title=Subversion_Repository&action=edit&redlink=1)

**drftpd-dependencies**

You need to download the [dependencies zip file](https://web.archive.org/web/20170804022954/http://sourceforge.net/project/showfiles.php?group_id=56385&package_id=142530) from SourceForge as well. The contents rarely change to it's distributed seperately as it's a much larger download than the source. Download the file to your drftpd directory and unzip.

## OTHER DEPENDENCIES

**Java**

Now that you have the latest drftpd, you will need a way to compile it. Drftpd 2.0 slaves can run under the older Java 1.4, however the master, and the machine on which you will be compiling must be using Java 5.0 (aka: 1.5). To compile drftpd you must use the Java SDK (aka: JDK). See [Installation:Java](https://web.archive.org/web/20170804022954/http://www.drftpd.org/index.php/Installation:Java)

**Apache Ant**

Drftpd 2.0 comes with a build.xml file which can be used by Apache Ant to compile all of the necessary classes into your classes/ folder. Ant is written in java, so it will run on any platform, and can be found here: [http://ant.apache.org/](https://web.archive.org/web/20170804022954/http://ant.apache.org/)

## COMPILING

Once you have properly installed the latest JDK and Ant, you should unpack both the drftpd source and dependacies zip files into the same directory and run ant, from the drftpd directory. Before you can run ant, you may need to setup some environment variables, this is different depending on your platform.

**Windows**

* Right-click on My Computer and select 'Properties'
* Select the 'Advanced' tab
* Click 'Environment Variables'
* Locate the 'PATH' variable under System Variables and press 'Edit'
* Paths are separated by semi-colons, so add ";X:\Path\To\Java\bin;X:\Path\To\Ant\bin" and press 'OK'
* Create a new system variable with the name 'JAVA_HOME' and the value "X:\Path\To\Java" and press 'OK'

Be sure to change all directory paths to the locations they are stored on your pc.

**Linux/BSD/etc**

If ant and java/javac aren't in your path, edit /etc/profile and add lines like the following to the bottom of the file replacing the paths with the actual paths on your system:

```
PATH="/usr/java/jdk1.5.0/bin:$PATH"; export PATH
PATH="/usr/share/ant/bin:$PATH"; export PATH
JAVA_HOME="/usr/java/jdk1.5.0"; export JAVA_HOME
```

**Other**

(insert instructions here)

## CONFIGURING

**CHANGE ALL .conf.dist Files to Conf**

1. If you are sure you don't want a .dist copy as a backup (i.e. this is NOT your first time installing DRFTPD), you can run this command from inside the conf directory (works on Linux/BSD):
2. for file in *.conf.dist ; do mv $file `echo $file | sed 's/\(.*\.\)conf.dist/\1conf/'` ; done
3. Note: It is an acute (grave accent,the key below Esc) before echo, apostrophe after sed, then apostrophe acute after 1conf/
4. This will move all .conf.dist files to .conf files in the working directory. Here is a 3 line .sh script that I keep with my drftpd files.
5. #! /bin/sh
6. for file in *.conf.dist ; do mv $file `echo $file | sed 's/\(.*\.\)conf.dist/\1conf/'` ; done
7. exit 0

**Edit drftpd.conf**

1. Change master.bindport and master.port if wanted
2. Uncomment the plug-ins you want (i.e. SiteBotSSL) to enable them
3. Make sure they are in Numeric Order
4. Change sectionmanager to org.drftpd.sections.conf.SectionManager if you want to use conf/sections.conf to control your sections (Recommended)

**Edit slave.conf**

1. Change Slavename
2. Change master.bindport to correspond to bindport set in drftpd.conf

**Now To conf/**

**perms.conf**

Uncomment and change pasv_addr to your master IP.

**irc.conf**

Change irc.server/port/ssl to desired Change blowfish if wanted Change/Add channels in irc.channel.n (n to be incremented) Change irc.nick/name/user to desired Permissions on IRC commands are no longer located in irc.conf, they are in irccommands.conf

**trial.conf**

> Change n.name to desired quota/trial group
> Change n.quota to the size of trial/quota you want
> Change n.period to weekly/monthly/daily
> Change n.fail to delete/setgrp grpname
> Change n.pass to delete/setgrp grpname
> Change n.perms to !=siteop (not siteop group) !=iND (not iND group) * (but everyone else)
> Make sure all trial/quotas are incremented by n

**sections.conf**

> Change n.path/name to desired
> For Dated Paths Use:

```
n.type=dated`
n.dated=MMdd  (dated code, could be any type of date/week/month code)
n.now=-Today (Symbolic link to be set in main dir for linking to dated dir)
```

End Of Basic conf/ Configuration

## Starting It Up

You must make a 'logs' directory in your main drftpd directory if it is not there. Back to the root drftpd directory run ./genkey.sh or genkey.bat (This creates a generated key file for secure sockets) If all goes well, Time to start the FTPD

master.bat or ./master.sh console (If no errors come up you are A.O.K) ctrl->c to close

If the wrapper stops, there are errors. They will be in drftpd.log in the logs directory. Usually last few lines.

./master.sh start (Starts the Ftp Server)

Now you have to log into the ftp server ftp 127.0.0.1 port(the port you specified in drftpd.conf) username = drftpd password = drftpd

you are now logged in!

Next, you need to add a slave. Since 2.0 does not have auto local slaves, you have to add it yourself. To add a slave type "site addslave slavename(the name you have in slave.conf)", once that is done type "site slave slavename addmask *@mask (if its local 127.0.0.1 will work fine)

Now that the slave is added, you can exit it off the ftp and run ./slave.sh console (Note how I always run console until I know the ftp server runs correctly *HINT *HINT)

If there are no errors the slave will connect without problems and will say "******* Finished Registering With Master" (Or Something to that sort) Once that is done, you can stop the console by ctrl->c and then start the slave using ./slave.sh start

Log back onto the ftp and type "site slaves" you should see you slave online

# Advanced Perm/Slave selection Configuration

## Common Permissions Settings

Now that your ftp server is running, time to tweak it a little bit
Got into conf/ and edit perms.conf

**To Deny The making of dirs in root dir by anyone other than siteop use these**

```
makedir   /?*/*   *
makedir   /*   =siteop
```

This can go for other things like rename, delete, etc. For those that don't know: * = Nothing+Anything ? = At.Least.1.char+anything

**No Stats (People Always Ask About This)**

```
nostatsup   /DIR1/*   *
nostatsdn   /DIR1/*   *
```

This Does Not Record Stats(allup,monthup etc., but not credits) For uploading or downloading in /DIR1/

**New Dir Spam**

Another problem that people always encounter is dirs listing on IRC that they don't want, to fix this dirlog is the key:

```
dirlog   /DIR1/*   !*
dirlog   *   *
```

This Will Not Announce Anything In /DIR1/ to IRC but Will Announce Everything Else

Along With The Previous Command is hideinwho (This is used to keep users uploading in these dirs hidden from !who !speed !bw "site who" etc):

```
hideinwho   /DIR1/*   *
hideinwho   /DIR2/*   =group1
```

This will hide everyone that is in /DIR1/ and hide group1 from being in /DIR2/

**Next Is To Not Allow Credits For Certain Dirs**

```
#creditcheck   <path>   <ratio>   <flag/=group/-user>
#creditloss   <path>   <ratio>   <flag/=group/-user>
```

```
creditcheck   /DIR1/*   0   *
creditloss    /DIR1/*   0   *
```

This works the same as nostats, creditcheck will allow u to download anything in /DIR1/ regardless of credits and upload anything without receiving credits

In This file there is also a way to restrict ftp site commands to certain users/groups

```
shutdown =siteop
take =take =siteop
slaves !=iND
```

'shutdown =siteop' is very counter-intuitive. You may think it will limit access to 'site shutdown' to only siteops, but in fact it will allow only siteop group to login to the site. To allow all groups to login, use 'shutdown *' or 'shutdown =siteop =iND =group1 =group2 ... =groupN'.

'slaves !=iND' will allow everyone BUT group iND to "site slaves"

End Of perms.conf

## Slave Addition and Removal

Slaves aren't added in the same way as in DrFtpd1, all administration is done through commands.

Syntax: `SITE ADDSLAVE <slavename>`

or to remove: `SITE DELSLAVE <slavename>`

Once a slave is added, it will need an IP mask...

Syntax: `SITE SLAVE <slavename> ADDMASK <ident@ip>`

Similarly, to remove...

Syntax: `SITE SLAVE <slavename> DELMASK <ident@ip>`

You can list all slaves by doing SITE SLAVES and see specific information about a slave by doing SITE SLAVE <slavename>

## Slave Selection

Now a file that has become more and more popular DAILY, here is slaveselection-up.conf (This is for setting up which files get sent to which slaves)

First Thing, DO NOT REMOVE ANYTHING THAT IS THERE, UNLESS YOU KNOW WHAT UR DOING

As you can see again these are all number in sequential order of n.

The problem most people are having, and the one I am going to describe is matchdir, if you want certain dirs to go to certain slaves

Whenever adding a new filter, be sure you add it before "n.filter=cycle". That should always be at the end of the file unless you know what you're doing

`n.filter=matchdir`
`n.match=/DIR1*`
`n.assign=SLAVE1+remove, SLAVE2+remove, SLAVE3+500, SLAVE4-200`

The other one that some people edit is the filter minfreespace. The n=3 one. 100MB is pretty low to stop the ftp from sending because the logs can be over 100mb's in many instances and the computer will stop running. In this cause you can change the 100MB to 500MB or even 1GB. But remember this is not per slave, this is for all slaves

End of slaveselection-up.conf

There that's the basic instructions for Advanced Configurations

# Common Problems
**Q: JRE cannot Initialize when trying to run master/slave **
> A: Java path not set in profile. Either add the path to profile, or make a symbolic link to java in /usr/bin

**Q: Connection Refused when trying to start slave **
> A: Could be 2 things, The ip you are trying to connect to is wrong, or the master bind port is wrong

**Q: Cannot Find drftpd.key **
> A: You didn't run genkey.sh

**Q: Can you help me out with 1.1.4 **
> A: It has been out of dev for a while now, so No (Upgrade)

**Q: Will This Work For Windows **
> A: Yes the only difference is the .sh commands to the service commands in windows

**Q: Can you write specific commands for starting Master Slave in windows **
> A: No, Don't have the time or patience for windows

**Q: What Version Of Java Do I Need?** 
> A: Master Needs 1.5 but slaves do not, but to be on the safe side, id put 1.5 on both

**Q: HELP! I can't transfer files, I keep getting Failed errors! **
> A: drftpd.key that you created on your master _NEEDS_ to be on all your slaves too

# Ending Credits

This was made because there are a lot of people asking the same question in #drftpd and im sure people are tired of answering the same questions. This also wouldn't have been possible if it wasn't for the amazing productivity of the development group of DrFtpd, you guys know who you are.

I will keep updating this when I get more questions, and answers.
If you have any other questions about this msg _CyBeR_ on irc.
I really hope this helps all the newbie's out getting this ftp server working  :)
I would also like to thank everyone who has contributed to this help page, it wouldn't be the same without all the contributions