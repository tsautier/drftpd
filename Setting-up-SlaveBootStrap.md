URLBootStrap is something i wrote for auto-updating slaves with no effort across both windows and linux platforms

It works by you building a slave.jar on the master which is built on compile.

You then execute URLBootStrap as main-class instead of Slave.

Here is an example snippet from wrapper-slave.conf:

```
wrapper.java.mainclass=org.tanukisoftware.wrapper.WrapperSimpleApp

wrapper.app.parameter.1=URLBootStrap`
wrapper.app.parameter.2=http://mydomain.com/slave.jar
wrapper.app.parameter.3=org.drftpd.slave.Slave
```

WrapperSimpleApp is still used for starting drftpd.

You could use this to create a small slavedist for setting up the slaves, which only includes URLBootStrap and some 3rd party jars, that works on both win32 and linux (and possibly others). This slavedist is called slave.zip and is built on compile.

Here is a list of files included in my slavedist:

```
slave.sh
slave.bat
bin/wrapper.exe
bin/wrapper ''(the ELF executeable)''
slave.conf.dist
bin/UninstallSlave-NT.bat
bin/InstallSlave-NT.bat
classes/org/drftpd/URLBootStrap.class
src/org/drftpd/URLBootStrap.java
conf/wrapper-slave.conf.dist
lib/FileSystem.dll
lib/libFileSystem.so
lib/log4j.jar
lib/libwrapper.so
lib/wrapper.dll
lib/wrapper.jar
lib/slave.jar (to be used without URLBootStrap, but just as a slave distribution)
```