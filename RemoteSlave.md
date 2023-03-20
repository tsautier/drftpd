Basic Run Down on installing a remote slave. (Parts Borrowed from the install guide)

Java

Now that you have the latest drftpd, you will need a way to compile it. Drftpd 2.0 slaves can run under the older Java 1.4, however the master, and the machine on which you will be compiling must be using Java 5.0 (aka: 1.5). To compile drftpd you must use the Java SDK (aka: JDK). JRE is fine for slaves. [JAVA](https://web.archive.org/web/20170303084914/http://www.drftpd.org/index.php/Installation:Java)

**Windows**

   * Right-click on My Computer and select 'Properties'
   * Select the 'Advanced' tab
   * Click 'Environment Variables'
   * Locate the 'PATH' variable under System Variables and press 'Edit'
   * Paths are separated by semi-colons, so add `;X:\Path\To\Java\bin` and press 'OK'
   * Create a new system variable with the name `JAVA_HOME` and the value `X:\Path\To\Java` and press 'OK' 

Be sure to change all directory paths to the locations they are stored on your pc.

**Linux/BSD/etc**

If ant and java/javac aren't in your path, edit `/etc/profile` and add lines like the following to the bottom of the file replacing the paths with the actual paths on your system:

```
PATH="/usr/java/jre1.5.0/bin:$PATH"; export PATH
JAVA_HOME="/usr/java/jre1.5.0"; export JAVA_HOME 
```

Copy `slave.zip` from the master(in the same folder as `drftpd.conf`). copy `conf/wrapper-slave.conf` as well.

Extract to a safe place on slave. Rename `slave.conf.dist` to `slave.conf` and edit to suit.

  1. Change Slavename
  2. Change master.bindport to correspond to bindport set in drftpd.conf 
  3. Change master IP.

Execute slave. `./slave.sh` console

check that it works then

```
./slave.sh start
```

for windows. use the `.bat` file.

```
SITE ADDSLAVE SLAVENAME
SITE SLAVE SLAVENAME ADDMASK *@89.53.64.*
```