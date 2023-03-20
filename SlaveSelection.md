SlaveSelection is a solution to picking the "best" slave for each situation.

As of now, there are two SlaveSelectionManagers. One resides in `org.drftpd.slaveselection.def`, this class works like drftpd did in versions prior to SlaveSelection (pre 1.0.x) versions.

It uses the slave with the most free bandwidth in its respective direction.

The other one resides in `org.drftpd.slaveselection.filter`, this class works by using filters (which also reside in this directory).

Each filter works by using special factors that are given to it to attach a score to a slave.

The scores are tallied up through each of the filters and the slave with the best score is used. The list of filters are as follows:

* [Cycle](https://github.com/drftpd-ng/drftpd/wiki/Cycle)
* [Bandwidth](https://github.com/drftpd-ng/drftpd/wiki/Bandwidth)
* [Reversebandwidth](https://github.com/drftpd-ng/drftpd/wiki/Reversebandwidth)
* [Maxbandwidth](https://github.com/drftpd-ng/drftpd/wiki/Maxbandwidth)
* [Maxtransfers](https://github.com/drftpd-ng/drftpd/wiki/Maxtransfers)
* [Minfreespace](https://github.com/drftpd-ng/drftpd/wiki/Minfreespace)
* [Mintimeonline](https://github.com/drftpd-ng/drftpd/wiki/Mintimeonline)
* [Slavetop](https://github.com/drftpd-ng/drftpd/wiki/Slavetop)
* [Matchdir](https://github.com/drftpd-ng/drftpd/wiki/Matchdir)

For typical transfers either `slaveselection-up.conf` or `slaveselection-down.conf` are used, however if you are using the Mirror or Archive plugin, then you are also using the `slaveselection-jobup.conf` and `slaveselection-jobdown.conf`.