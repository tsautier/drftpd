## Installation steps

DrFTPD 3 installation requires a number of steps before you can utilize the software to its full extend.
To give an overview of the installation process the different steps are listed below in this section.
On the master you will need to:

* Install Sun JAVA 1.7
* Install ANT or Eclipse on the master
* Add needed plugins that are not present
* Compile the software using setup wizard
* Configure .conf files

On the slaves you will need to:

* Install Sun JAVA 1.7
* Copy slave.zip to a slave from the master
* Configure slave.conf

Note: All steps with commands below are for *nix. Windows users should be able to complete the steps using the Windows GUI.

## Install java

For o/s specific information see: [Installation:Java](https://web.archive.org/web/20170115090213/http://www.drftpd.org/index.php/Installation:Java)

Generial info follows:

Download and install a java development kit 7 (7DK) on the master.
Download and install a java runtime environment 7 (JRE) or java development kit 7 (JDK) on the slaves.

You can get sun's JDK here: [http://java.sun.com/j2se/downloads.html](https://web.archive.org/web/20170115090213/http://java.sun.com/j2se/downloads.html)

If you want to utilize blowfish in your environment also download Java Cryptography Extension (JCE) Unlimited Strength Jurisdiction Policy Files 7. You will need to manually replace the files local_policy.jar and US_export_policy.jar in your java/jre/lib/security folder.

**Problems**

* Ensure that JAVA_HOME is configured. You can check this using "echo %JAVA_HOME%" on Windows platform or using "echo $JAVA_HOME" on *nix
* *nix If you encounter problems like "master.sh: line 11: exec: java: not found", you need to add the java binary to your PATH environment variable. Edit your /etc/profile or .bashrc (for current user only) and add PATH=$PATH:$JAVA_HOME/bin at the bottom. Make sure that your enviroment variable $JAVA_HOME is set correctly.
* Windows If you encounter problems like " 'JAVA' is not recognized as an internal or external command, operable program or batch file.". You also need to add the java binary to your PATH environment variable. You can do this in Windows in your System Properties under the Advanced Tab, there is a button Environment Variables, edit your PATH variable accordingly.

These are issues with your Operating System/Java Install and not related to DrFTPD.

## Install ant

Compiling DrFTPD is required to use the software.
To allow you to compile java you will need to install ANT or ECLIPSE.
You can find the installation documentation here: [http://ant.apache.org/manual/install.html](https://web.archive.org/web/20170115090213/http://ant.apache.org/manual/install.html)

**Problems**

* Ensure that ANT_HOME is configured. You can check this using "echo %ANT_HOME%" on Windows platform or using "echo $ANT_HOME" on *nix

## Downloading

Download DrFTPD from [https://github.com/drftpd-ng/drftpd3](https://web.archive.org/web/20170115090213/https://github.com/drftpd-ng/drftpd3)

* Change to the main DrFTPD folder, for example ~/drftpd (*nix) or c:\drftpd (windows).

```
git clone hhttps://github.com/drftpd-ng/drftpd3.git
unzip master.zip
rm master.zip
```

## Build

DrFTPD version 3 comes with an installer. Using this installer you are able to choose which components that should be compiled. You start the installer using ./build.sh on *nix or build.bat on Windows.


## Master configuration

Details to come...!!!

Note: DrFTPD version 3 does not require that you create the logs manually like earlier versions. Missing folders will be created. This is the same for both the master and slaves.

## Plugins

Plugins and code modifications of DrFTPD versions prior 3.0.0 will not work. You need to use plugins designed for version 3.0.0.
Unofficial plugins can be found here [http://drftpd.org/forums/viewforum.php?f=26](https://web.archive.org/web/20170115090213/http://drftpd.org/forums/viewforum.php?f=26)

**Installation instructions**

Each plugin should come with its own small installation instruction. Especially if this is not the same installation method as below

1. Upload zip file to src/plugins/
2. Make sure that you remove all files leftovers if the plugin already is present. rm -r <plugin_foldername>
3. Run Unzip to uncompress files into src/plugins. unzip <plugin_name>.zip
4. Run build.sh and choose to activate the plugin, default = no
5. Update .conf files according with new conf settings from .dist files.
6. Start the site or if already started type SITE LOADPLUGIN <plugin name>

## Slave installation

DrFTPD slaves require java runtime environment 7 (JRE) or java development kit 7 (JDK). Please see above for download information.

Copy the slave.zip file from your master to the server that you plan to run the slave on.

* Download slave.zip from the masters main folder
* Unzip slave.zip
* On *nix you will need to change filemodes

```
chmod 744 slave.sh
chmod 744 bin/wrapper
```

* Copy slave.conf.dist to slave.conf
* Copy conf/wrapper-slave.conf.dist to conf/wrapper-slave.conf
* Copy conf/diskselection.conf.dist to conf/diskselection.conf
* Edit slave.conf
> The minimum changes that you must complete in slave.conf is to change slave.name, master.host and master.port. It is also recommended to specify a range of ports to use for file transfers. Edit slave.portfrom and slave.portto. Leave everything else unchanged unless you know what you are doing.

You are now ready to add the slave in the master configuration

```
SITE ADDSLAVE <slavename>
SITE SLAVE <slavename> ADDMASK *@<ipmask_of_slave>
```

You can now start the slave

* *nix

```
./slave.sh
```

Windows, You would very likely want to add the slave as a service within Windows.

```
bin\wrapper -i c:\drftpd\bin\wrapper-slave.conf
net start drftpd-slave
```

Verify that the slave is coming online with `SITE SLAVES`.

## Windows Installation

1. Install the JDK suite (just click through all the defaults). You can get sun's JDK here: [http://java.sun.com/j2se/downloads.html](https://web.archive.org/web/20170115090213/http://java.sun.com/j2se/downloads.html) You do not need the one with netbeans, but just the standard, basic java jdk.
2. Download apache-ant, I take it and unzip it to c:\ant\, such that c:\ant\bin\ant is the ant compiler (this can be tested by going to start->run->cmd and then typing c:\ant\bin\ant, ant should fire up and give an error. ( [http://ant.apache.org/bindownload.cgi](https://web.archive.org/web/20170115090213/http://ant.apache.org/bindownload.cgi) )
3. Download and unzip drftpd, i do so to c:\drftpd\.
4. Add the following lines to wrapper.conf, wrapper-slave.conf, and wrapper-master.conf

```
# Working Dir
wrapper.working.dir=../
```

This can go anywhere in said file.

5. In your System Properties under the Advanced Tab, there is a button entitled "Environment Variables," edit your PATH variable to include the java directory (i.e. c:\program files\java\jre7\bin), also set your JAVA_HOME to this path, but without the bin appeneded to the end (i.e. c:\program files\java\jre\)
6. Open a command prompt (start->run->cmd) and type cd c:\drftpd\ , then type c:\ant\bin\ant and it should compile DrFTPD. If any errors are generated, something went wrong, and DrFTPD most likely will not function.

## FAQ

**java.lang.Error: failed instanciating SAX parser**
You did not setup JAVA and ANT correctly. Check if ANT and JAVA commands execute the right Java version

**org.drftpd.exceptions.SSLUnavailableException: Secure connections to slave required but SSL isn't available**
You did not setup JAVA correctly. Check if the KEYTOOL is the one from your used Java version and if you generated and copyed the drftpd.key

**Exception in thread "main" java.lang.UnsatisfiedLinkError: /..../libTerminal.so**
You are likely trying to build on a x64 OS. Basically you will need to replace wrapper with a 64bit version, and java sdk 64bit .... and libTerminal.so has to be compiled for 64bit aswell. Search on libTerminal.so where you can test a few versions that people have uploaded on the forum (or compile your own).

## Online Help
If you use IRC, connect to [EFnet](https://web.archive.org/web/20170115090213/http://www.efnet.org/?module=servers) and join #drftpd. Alternatively use the forum.