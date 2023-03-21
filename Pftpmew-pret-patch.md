```
diff -ur pftpfxp-mew.orig/src/server.cc pftpfxp-mew/src/server.cc
--- pftpfxp-mew.orig/src/server.cc	2003-09-05 15:26:57.000000000 +0200
+++ pftpfxp-mew/src/server.cc	2003-09-26 22:23:10.000000000 +0200
@@ -3041,6 +3041,14 @@
 			return (FALSE);
 		}
 	} else {
+		sprintf(this->temp_string, "PRET RETR %s\r\n", file);
+		if (!this->tcp.SendData(this->temp_string)) {
+			this->error = E_CONTROL_RESET;
+			this->PostBusy(NULL);
+			return (FALSE);
+		}
+		this->tcp.WaitForMessage();
+
 		if (!this->tcp.SendData("PASV\r\n")) {
 			this->error = E_CONTROL_RESET;
 			this->PostBusy(NULL);
@@ -3434,6 +3442,15 @@
 	}
 
 	if (fxpmethod == 0) {
+		sprintf(this->temp_string, "PRET RETR %s\r\n", file);
+		if (!this->tcp.SendData(this->temp_string)) {
+			this->error = E_CONTROL_RESET;
+			this->PostBusy(NULL);
+			dest->PostBusy(NULL);
+			return (FALSE);
+		}
+		this->tcp.WaitForMessage();
+
 		// send PASV, extract PORT info and post file, then wait (until DEST sent PORT and then STOR)
 		if (!this->tcp.SendData("PASV\r\n")) {
 			this->error = E_CONTROL_RESET;
@@ -3646,6 +3663,15 @@
 
 		debuglog("normal fxp rejected trying alternative method");
 
+		sprintf(this->temp_string, "PRET STOR %s\r\n", file);
+		if (!dest->tcp.SendData(this->temp_string)) {
+			this->error = E_CONTROL_RESET;
+			this->PostBusy(NULL);
+			dest->PostBusy(NULL);
+			return (FALSE);
+		}
+		dest->tcp.WaitForMessage();
+ 
 		// send PASV, extract PORT info and post file, then wait (until DEST sent PORT and then RETR)
 		if (!dest->tcp.SendData("PASV\r\n")) {
 			this->error = E_CONTROL_RESET;
@@ -3967,6 +3993,14 @@
 			return (FALSE);
 		}
 	} else {
+		sprintf(this->temp_string, "PRET STOR %s\r\n", file);
+		if (!this->tcp.SendData(this->temp_string)) {
+			this->error = E_CONTROL_RESET;
+			this->PostBusy(NULL);
+			return (FALSE);
+		}
+		this->tcp.WaitForMessage();
+
 		if (!this->tcp.SendData("PASV\r\n")) {
 			this->error = E_CONTROL_RESET;
 			this->PostBusy(NULL);
@@ -4218,6 +4252,13 @@
 				return (FALSE);
 			}
 		} else {                /* PASV MODE */
+			if(!this->tcp.SendData("PRET LIST\r\n")) {
+				this->error = E_CONTROL_RESET;
+				this->PostBusy(NULL);
+				return (FALSE);
+			}
+			this->tcp.WaitForMessage();
+
 			if (!this->tcp.SendData("PASV\r\n")) {
 				this->error = E_CONTROL_RESET;
 				this->PostBusy(NULL)
```