```
diff -ur pftpfxp.orig/src/server.cc pftpfxp/src/server.cc
--- pftpfxp.orig/src/server.cc	2003-09-16 14:16:59.000000000 +0200
+++ pftpfxp/src/server.cc	2003-09-16 15:37:12.000000000 +0200
@@ -2632,6 +2632,14 @@
             return (FALSE);
         }
     } else {
+	sprintf(this->temp_string, "PRET RETR %s\r\n", file);
+	if (!this->tcp.SendData(this->temp_string)) {
+	    this->error = E_CONTROL_RESET;
+	    this->PostBusy(NULL);
+	    return (FALSE);
+	}
+	this->tcp.WaitForMessage();
+
         if (!this->tcp.SendData("PASV\r\n")) {
             this->error = E_CONTROL_RESET;
             this->PostBusy(NULL);
@@ -3021,6 +3029,15 @@
 
     if (fxpmethod == 0) {
 // send PASV, extract PORT info and post file, then wait (until DEST sent PORT and then STOR)
+	sprintf(this->temp_string, "PRET RETR %s\r\n", file);
+	if (!this->tcp.SendData(this->temp_string)) {
+	    this->error = E_CONTROL_RESET;
+	    this->PostBusy(NULL);
+	    dest->PostBusy(NULL);
+	    return (FALSE);
+	}
+	this->tcp.WaitForMessage();
+
         if (!this->tcp.SendData("PASV\r\n")) {
             this->error = E_CONTROL_RESET;
             this->PostBusy(NULL);
@@ -3232,6 +3249,15 @@
         debuglog("normal fxp rejected trying alternative method");
 
 // send PASV, extract PORT info and post file, then wait (until DEST sent PORT and then RETR)
+	sprintf(this->temp_string, "PRET STOR %s\r\n", file);
+	if (!dest->tcp.SendData(this->temp_string)) {
+            this->error = E_CONTROL_RESET;
+            this->PostBusy(NULL);
+            dest->PostBusy(NULL);
+            return (FALSE);
+	}
+	dest->tcp.WaitForMessage();
+
         if (!dest->tcp.SendData("PASV\r\n")) {
             this->error = E_CONTROL_RESET;
             this->PostBusy(NULL);
@@ -3538,6 +3564,14 @@
             return (FALSE);
         }
     } else {
+	sprintf(this->temp_string, "PRET STOR %s\r\n", file);
+	if (!this->tcp.SendData(this->temp_string)) {
+	    this->error = E_CONTROL_RESET;
+	    this->PostBusy(NULL);
+	    return (FALSE);
+	}
+	this->tcp.WaitForMessage();
+
         if (!this->tcp.SendData("PASV\r\n")) {
             this->error = E_CONTROL_RESET;
             this->PostBusy(NULL);
@@ -3788,6 +3822,13 @@
             }
 /* PASV MODE */
         } else {
+	    if(!this->tcp.SendData("PRET LIST\r\n")) {
+		this->error = E_CONTROL_RESET;
+		this->PostBusy(NULL);
+		return (FALSE);
+	    }
+	    this->tcp.WaitForMessage();
+
             if (!this->tcp.SendData("PASV\r\n")) {
                 this->error = E_CONTROL_RESET;
                 this->PostBusy(NULL);
```