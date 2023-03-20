Many people have issues with connecting to drftpd because they are behind some kind of NAT device.

Your choices are

1. Getting a [Client with PRET support](https://github.com/drftpd-ng/drftpd3/wiki/Clients-with-PRET-support)
2. Getting a UPnP client and router (such as SmartFTP)
3. Configuring your router and client to use a port forwarding with a port range or DMZ.

## DMZ - Demilitarized Zone

Below is a url link that shows the first steps in resolving this issue if you are using FlashFXP http://w1.195.telia.com/~u19510214/guide/

The link above assumes that you are behind a router and already setup as the DMZ (demiliterized zone).

**===SETTiNG YOUR DMZ===**

Most home routers allow you to access your router via the web : example: http://192.168.?.? whereas the question marks represent your routers ip address. Each Brand of router has different default settings.

Example: http://192.168.0.1 would be the login for the admin for a D-Link router and http://192.168.1.1 is for LinkSys. Your router manual will have this documentation. If you dont have your manual go to the manufaturers web page and download it. Once the login pops up for you to access your router you need to type in the default username and password if the router requires you to do so. This is also in the routers manual.

D-link default is username: admin password: (there is none).

Once you get into your router you need to check all the tabs and find a DMZ section or anything about DMZ. Inside that section of your router admin you need to enable it and have it assign to the computer's ip address that you are using to login to the ftp. to find out what ip addres your router assigned your computer you can do this.

MS windows - start > run > cmd (for win98-2000-nt) This will bring up a command prompt window. XP > start > all programs > accessories > command prompt This also brings up the same thing. at the command prompt you see c:\>_ and the cursor is blinking. Type ipconfig and this will display your ip address. something like 192.168.?.? This is the ip address your assign your dmz to. Once you assign the DMZ to your pc you are set. As long as you have your flash fxp firewall settings bound to your wan ip addess and you specify any port range.

## Port range

I suggest you specify port ranges of 1000-1099 or something allowing at least 100 ports open. Since ftp only uses TCP, only TCP port ranges need to be forwarded. For the purpose of drftpd, you can drop all incoming UDP packets.