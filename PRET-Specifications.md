Background: Distributed FTP Daemon
***

DrFTPD is a distributed FTP daemon. It does not have a local filesystem but uses nodes/slaves with different IPs for storage and transfers. Files are distributed across the slaves and are present on 1 or more slaves.
DrFTPD uses a different slave/node depending on what command/file is requested. For a LIST DrFTPD specifies it's own IP. For a RETR, DrFTPD checks it's file database to see what slaves have that specific file.
If the client is in active transfer mode (it sent a PORT command), it uses a slave to connect to the ip:port specified, or connects there itself if it was a LIST/NLST request.
If the client is in passive mode (it sent a PASV command), it listens on a random socket on that specific slave, or listens there itself if it was a LIST/NLST.


PASV won't work with distributed transfers
***

However, PASV won't work as DrFTPD hasn't been told what kind of transfer to expect (LIST/NLST/RETR/STOR/STOU/APPE), and it cannot guess reliably, it needs to be told what command is expected at the same time or before.


**Example: RETR in passive transfer mode**
`C: PASV`
_Server thinks: My ip is 10.0.0.1, i have slaves at 10.1.0.1, 10.1.0.2, 10.2.0.1 [unlimited amount of other slaves could be involved]. Which IP should i send? I can't relibably guess but let's try sending my ip anyway_

`S: 227 Entering Passive Mode (10,0,0,1,13,236)`
`C: RETR file.zip`
_Server thinks: Uh oh! I don't have file.zip. It exists only on 10.1.0.2, but i just sent 10.0.0.1 as IP. Bummer!_

`S: 421 Ops! I screwed up! Sure would be easier with Distributed PASV!`

**Example: LIST in passive transfer mode**
`C: PASV`
_Server thinks: My ip is 10.0.0.1, i have slaves at 10.1.0.1, 10.1.0.2, 10.2.0.1 [unlimited amount of other slaves could be involved]. Which IP should i send? I can't relibably guess but let's try sending my ip anyway_

`S: 227 Entering Passive Mode (10,0,0,1,13,236)`
`C: LIST`
_Server thinks: Uh oh! The slave doesn't know what files in the file database. Bummer!_

`S: 421 Ops! I screwed up! Sure would be easier with Distributed PASV!`

**Example: LIST in active transfer mode**
Port works fine however.

`C: PORT 192,168,254,1,12,345`
_Server thinks: Ok, i'll remember that_

`S: 200 PORT command successful.`
`C: LIST`
_Server thinks: No problem! I'll just open a TCP connection to the ip:port you specified with the PORT command_

`S: 150 File status okay; about to open data connection.`
`S: 226 Closing data connection`

**Example: RETR in active transfer mode**
`C: PORT 192,168,254,1,12,345`
_Server thinks: Ok, i'll remember that_

`S: 200 PORT command successful.`
`C: RETR file.zip`
_Server thinks: No problem! I'll just tell the node/slave on 10.1.0.2 that has the file file.zip to send it over a TCP connection to the ip:port you specified with the PORT command_

`S: 150 File status okay; about to open data connection.`
`S: 226 Closing data connection`

Conclusion
***

PASV doesn't work with standard FTP and distributed transfers.

The files could be streamed/proxied through the slaves but this is unwanted as it would create unnessecary network traffic and/or file storage space.

Solution: PRET (PRE Transfer) command
***

This is the easy fix. The client sends a PRET (PRE Transfer) command before PASV to inform DrFTPD on what kind of command/transfer will be used

**Example: RETR with PRET in passive transfer mode**
`C: PRET RETR file.zip`
_Server thinks: Ok! You're about to get file.zip, thanks for the warning!_

`S: 200 Command OK.`
`C: PASV`
_Server thinks: You said you were going to get file.zip, so i'll send you the PASV reply for a slave that has it._

`S: 227 Entering Passive Mode (10,0,0,1,13,236)`
`C: RETR file.zip`
_Server thinks: Right, file.zip which you pre-ordered earlier coming up!_

`S: 150 File status okay; about to open data connection.`
`S: 226 Closing data connection`

PRE Transfer Command for Distributed PASV Transfers
***

PRET is a command intended for easy implementation in FTP clients. Only minimal changes should be required for PRET to be implemented.

PRET is 100% backwards compatible and doesn't break anything.

**PRET Command**
Must be sent before the PASV command.

Commands that need PRET & PASV that are defined in RFC959: RETR STOR STOU* LIST NLST APPE*

`* = not yet implemented in drftpd.`
`PRET LIST [ignored arguments]`
`PRET NLST [ignored arguments]`
`PRET RETR <file> [ignored arguments]`
`PRET APPE <file> [ignored arguments]`
`PRET STOR <file> [ignored arguments]`
`PRET STOU [ignored arguments]`
If the PRET command was invalid the transfer command may reply 503 Bad sequence of commands

**Reply codes**
Servers MAY implement this command and send 202 Command superfluous at this site as a reply but are not required to support this command.

Servers MAY ignore or act upon this command.

`200 Command OK`
Optionally, the server MAY want to inform the user of what action was taken

`200 OK, next transfer will be from <source>.`
Where source is a human-readable string.

`500 Command not understood.`
`502 Command not implemented.`
The expected reply from ftp servers not supporting PRET. No further attempts SHOULD be attempted.

`504 Command not implemented for param.`
If the specified transfer command is not supported.

If any reply other than these are received it is the reply of the upcoming transfer command. The client is not required or encouraged to act on this reply and can continue sending PASV and transfer command to receive the same error from the upcoming transfer command.

`530 Permission denied.`
`550 File not found.`
The reply that the upcoming transfer command will return.

**FEAT response for PRET**
When replying to the FEAT command, an FTP server process that supports the PRET command MUST include a line containing the single word "PRET". This MAY be sent in upper or lower case, or a mixture of both (it is case insensitive) but SHOULD be transmitted in upper case only. That is, the response SHOULD be

`C> Feat`
`S> 211- <any descriptive text>`
`S>  ...`
`S>  PRET`
`S>  ...`
`S> 211 End`
The ellipses indicate place holders where other features may be included, and are not required. The one space indentation of the feature lines is mandatory.

Solution: Distributed transfer commands
***

This solution is turned down in preference for #Solution: PRET (PRE Transfer) command. It might be relevant in the future though when an IPv6 implementations is needed. Or we can support PRET with EPRT, and break the RFC and support it with EPSV.

Distributed transfer commands


Related FTP Exensions
***

**GridFTPD**
GridFTPD defines ERET, ESTO and more for sending of striped files. It doesn't help distributed transfers however.

* http://phase.hpcc.jp/mirrors/globus/gt2/admin/guide-overview.html
**IPv6**
RFC 2428 - FTP Extensions for IPv6 and NATs

* http://www.wu-ftpd.org/rfc/rfc2428.html