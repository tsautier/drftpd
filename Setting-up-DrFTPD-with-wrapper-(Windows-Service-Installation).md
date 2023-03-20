This is a guide to using DrFTPD with wrapper.

Wrapper allows you to run DrFTPD as an NT Service under windows or as a nicely configured daemon process with init scripts under linux.

You can get documentation on wrapper is on their [website](https://web.archive.org/web/20130907054842/http://wrapper.tanukisoftware.org/doc/english/index.html) All you need for drftpd is also included in the dependencies package.

**wrapper.conf**
What you need to configure specifically for master and slave is included in wrapper-{master,slave}.conf.dist. Again, all of this is done for you, just lookover it to see if it is what you want.

**common**
`wrapper.java.mainclass=org.tanukisoftware.wrapper.WrapperSimpleApp
`
`wrapper.java.additional.1=-Djava.rmi.server.randomIDs=true
`
`wrapper.java.additional.2=-Djava.rmi.dgc.leaseValue=300000
`
`wrapper.java.additional.3=-Djava.rmi.server.disableHttp=true`