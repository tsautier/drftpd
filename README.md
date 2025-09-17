<h1 align="center">
  <a href="http://drftpd.org/"><img src="https://github.com/drftpd-ng/drftpd-ng.github.io/blob/2611e3c1010b6c3c9185a542fb6075eb3c697128/docs/resources/logo.jpg" alt="DrFTPD"></a>
</h1>
<p align="center">
  <a href="https://circleci.com/gh/drftpd-ng" alt="Build"><img src="https://circleci.com/gh/drftpd-ng/drftpd/tree/master.svg?style=shield" /></a>
  <a href="https://github.com/drftpd-ng/drftpd/wiki/Documentation" alt="Documentation"><img src="https://img.shields.io/badge/Documentation-RTFM-orange.svg" /></a>
</p>

## About DrFTPD
DrFTPD is a Distributed FTP server written in Java, it's unique because it doesn't handle transfers like normal FTP servers.
DrFTPD is set up with a master and a collection of file transfer slaves that handle the file transfers, you can have as many file transfer slaves as you like.
Some names that could be used to describe this is ftp site merger, ftp cluster, ftp grid or multi site bnc, but the only accurate term is "distributed ftp daemon."

What is unique with DrFTPD is that it works with existing FTP client software, you can use the FTP client application you're used to and make site-to-site (FXP) transfers with normal FTP servers.
The only exception to DrFTPD is with passive (PASV) mode. For this the client needs to support the PRET command. PRET is already supported in several of the most widely used FTP clients.
You can often do without PASV mode unless you are behind a firewall which you don't have access to or you need to FXP with another DrFTPD server or a server which doesn't support PASV.

If you merge 10*1gbit sites, you don't get a 10gbit site but you get a 10x1gbit site. What this means is that the aggregate bandwidth is 10gbit but a single transfer will never go above 1gbit.

DrFTPD's approach to the file system and file transfers is what makes it unique. Each file can, and will, end up on a different transfer slave.

DrFTPD uses transfer slaves for all file storage and transfers, it supports but doesn't require a file transfer slave to be run locally.
The master therefore uses very little bandwidth. FTP control connection, data connections for file listings and instructions to the slaves, are the only operations that consume bandwidth on the master.

The master has a filelist that keeps track of which slaves have which files and information about those files. A file can exist on multiple slaves for redundancy and more bandwidth.

When a slave is started, it gathers a filelist and sends the entire list to the master.
The master merges this list with it's existing file list and makes sure that it's in-sync with it's existing file list by adding and removing files to it's own list.
Because the master doesn't have any files locally, modifications to the virtual filesystem cannot be done easily from outside of the DrFTPD application.

Neither the master or the slaves need root privileges. The virtual filesystem contained on the master of which slaves files reside on is the authoritative source for information about the files.
Items like lastModified, size, user, and group, are all kept on the master.
The slave does however require exclusive write access to the storage area, otherwise it will become unsynced with the master filelist and errors can occur.

The slave is kept thin/dumb and isn't told anything about users. It simply processes the instructions that are given to the master and knows nothing about the ftp protocol.
This is an advantage as it simplifies administration of the slaves.

## How to get started

### Requirements

DrFTPD 4.x installation requires a number of steps before you can utilize the software to its full extend.
To give an overview of the installation process the different steps are listed below in this section.

On the master you will need to:
- Install Java JDK or OpenJDK 17, Apache Maven, Git

On the slaves you will need to:
- Install Java SE or OpenJDK 17.
- Add needed deps that are not present:
  - MediaInfo (CLI): https://mediaarea.net/en/MediaInfo
  - mkvalidator tool: https://github.com/Matroska-Org/foundation-source

## Versions

### Stable version (`4.0.12`)
For stable releases, use the latest official version:

- **Git**:  
  ```bash
  git clone https://github.com/drftpd-ng/drftpd.git
  cd drftpd
  git checkout tags/4.0.12
  ```
- **Archive**:  
  ```bash
  wget https://github.com/drftpd-ng/drftpd/archive/4.0.12.tar.gz
  tar xvzf 4.0.12.tar.gz
  cd drftpd-4.0.12
  ```

### Development version (`4.0.13-SNAPSHOT`)
If you want to test or contribute to the development version, use the main branch or the dev tag:

- **Git** (recommended for development):
  ```bash
  git clone https://github.com/drftpd-ng/drftpd.git
  cd drftpd
  # Main development branch
  git checkout main
  # or dev tag if available
  git checkout tags/4.0.13-SNAPSHOT
  ```
  > ⚠️ This version is not considered stable. It may contain bugs or experimental features.
- **Archive**:  
  ```bash
  wget https://github.com/tsautier/drftpd/archive/refs/heads/main.zip
  unzip main.zip
  cd drftpd-main
  ```
  > ⚠️ Development version, recommended for testing or development only.

---

#### Why use `git clone`?
- **For development or testing**: Always prefer `git clone` to benefit from updates and the full project history.
- **For quick installation or non-technical users**: The archive is sufficient, but not recommended for contribution.

---

## Build

To build the version you downloaded:

```bash
mvn clean
mvn validate
mvn install
```

Check the generated runtime directory.

## Master

- Copy .dist files to .conf only if you change the settings from config and plugins folders (at least `master.conf`)
- Optional: Run `./genkey.sh` for Linux or `genkey.bat` for Windows (if you want to use SSL (Master & Slave))
- Run `./master.sh` for Linux or `master.bat` for Windows
- Connect to `127.0.0.1:2121` with `drftpd:drftpd`
- Execute remote commands: `SITE ADDSLAVE SLAVENAME` and `SITE SLAVE SLAVENAME ADDMASK *@IP.MASK`
- Optional: Create Master Service (`systemd`, `sc.exe` ...)

## Slave

- Copy .dist files to .conf only if you change the settings from config folder (at least `slave.conf`)
- Optional: Copy the `drftpd.key` from the master to the config directory
- Run `./slave.sh` for Linux or `slave.bat` for Windows
- Optional: Create Slave Service (`systemd`, `sc.exe` ...)

## For development (debug only)
### Git

Clone the project from https://github.com/drftpd-ng/drftpd.git

- Run `git clone https://github.com/drftpd-ng/drftpd.git`
- Run `cd drftpd`
- Open `pom.xml` from .dev folder with IntelliJ IDEA Community
- IntelliJ:
Run `mvn package`

#### Master 
Create new Application via Run -> Edit Configurations

- Name: `Master`
- JDK: `java 17 SDK of 'drftpd-dev' module`
- Main class: `org.drftpd.master.Master`
- Example Working Directory: `C:\Users\Administrator\Documents\GitHub\drftpd\runtime\master`
- Example Environment variables: `DRFTPD_CONFIG_PATH=C:\Users\Administrator\Documents\GitHub\drftpd\runtime\master`
- Use `org.drftpd.master.Master`

Start debug Master

#### Slave 
Create new Application via Run -> Edit Configurations

- Name: `Slave`
- JDK: `java 17 SDK of 'drftpd-dev' module`
- Main class: `org.drftpd.slave.Slave`
- Example Working Directory: `C:\Users\Administrator\Documents\GitHub\drftpd\runtime\slave`
- Example Environment variables: `DRFTPD_CONFIG_PATH=C:\Users\Administrator\Documents\GitHub\drftpd\runtime\slave`

Start debug Slave

## Docker (experimental)
Docker images are automatically built from git tags. You can view the available images here: https://github.com/drftpd-ng/drftpd/pkgs/container/drftpd

The Docker images run in rootless mode. The mounted volumes must be writable by UID 1000.

A script is provided to initialize the config directory with the default config files. On first start of a container, set the entrypoint to run the initconfig script (see below). The config must be set as for a regular installation. The container can then be started with the default entrypoint.
One may also mount existing DrFTPD directories.

Example minimal compose file for the master:
```
  drftpd-master:
    image: ghcr.io/drftpd-ng/drftpd:master-latest
    environment:
      - JAVA_TOOL_OPTIONS=-Djdk.tls.acknowledgeCloseNotify=true -Xms3G -Xmx3G -XX:+UseG1GC -Dlog4j.configurationFile=config/log4j2-master.xml
    ports:
      - 1099:1099/tcp
      - 2121:2121/tcp
      - 50000-60000:50000-60000/tcp
    # Note: UID 1000 must have write access
    volumes:
      - /path/to/drftpd/master/config:/home/drftpd/master/config
      - /path/to/drftpd/master/index.bkp:/home/drftpd/master/index.bkp
      - /path/to/drftpd/master/logs:/home/drftpd/master/logs
      - /path/to/drftpd/master/userdata:/home/drftpd/master/userdata
    # Optional: initialize the config directory with the default config files
    #entrypoint: sh -c ./initconfig
```

Example minimal compose file for slaves:
```
  drftpd-slave:
    image: ghcr.io/drftpd-ng/drftpd:slave-latest
    environment:
      - JAVA_TOOL_OPTIONS=-Djdk.tls.acknowledgeCloseNotify=true -Xms1G -Xmx1G -XX:+UseG1GC -Dlog4j.configurationFile=config/log4j2-slave.xml
    ports:
      - 30000-33000:30000-33000/tcp
    # Note: UID 1000 must have write access
    volumes:
      - /path/to/drftpd/slave/config:/home/drftpd/slave/config
      - /path/to/drftpd/slave/logs:/home/drftpd/slave/logs
      - /path/to/drftpd/slave/site:/home/drftpd/slave/site
    # Optional: initialize the config directory with the default config files
    #entrypoint: sh -c ./initconfig
```

## Documentation (incomplete)
You can find the documentation online at: https://github.com/drftpd-ng/drftpd/wiki

## Support & Bug tracker
- ircs://irc.efnet.org:6697/drftpd - IRC support
- ircs://irc.efnet.org:6697/drftpd-devel - IRC support (too ;-))
- https://github.com/drftpd-ng/drftpd/issues - Bug tracker