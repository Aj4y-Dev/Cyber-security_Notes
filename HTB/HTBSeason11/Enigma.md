network enumeration:

```
PORT      STATE SERVICE  VERSION
22/tcp    open  ssh      OpenSSH 9.6p1 Ubuntu 3ubuntu13.16 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   256 0c:4b:d2:76:ab:10:06:92:05:dc:f7:55:94:7f:18:df (ECDSA)
|_  256 2d:6d:4a:4c:ee:2e:11:b6:c8:90:e6:83:e9:df:38:b0 (ED25519)
80/tcp    open  http     nginx 1.24.0 (Ubuntu)
|_http-title: Did not follow redirect to http://enigma.htb/
|_http-server-header: nginx/1.24.0 (Ubuntu)
110/tcp   open  pop3     Dovecot pop3d
|_pop3-capabilities: UIDL RESP-CODES SASL CAPA TOP AUTH-RESP-CODE STLS PIPELINING
| ssl-cert: Subject: commonName=enigma
| Subject Alternative Name: DNS:enigma
| Not valid before: 2026-02-18T20:33:33
|_Not valid after:  2036-02-16T20:33:33
|_ssl-date: TLS randomness does not represent time
111/tcp   open  rpcbind  2-4 (RPC #100000)
|_rpcinfo: ERROR: Script execution failed (use -d to debug)
143/tcp   open  imap     Dovecot imapd (Ubuntu)
|_ssl-date: TLS randomness does not represent time
| ssl-cert: Subject: commonName=enigma
| Subject Alternative Name: DNS:enigma
| Not valid before: 2026-02-18T20:33:33
|_Not valid after:  2036-02-16T20:33:33
|_imap-capabilities: LOGIN-REFERRALS SASL-IR more LOGINDISABLEDA0001 OK IDLE listed capabilities Pre-login have ENABLE LITERAL+ IMAP4rev1 post-login STARTTLS ID
993/tcp   open  ssl/imap Dovecot imapd (Ubuntu)
|_imap-capabilities: LOGIN-REFERRALS SASL-IR more ENABLE AUTH=PLAINA0001 IDLE listed capabilities have OK LITERAL+ IMAP4rev1 post-login Pre-login ID
| ssl-cert: Subject: commonName=enigma
| Subject Alternative Name: DNS:enigma
| Not valid before: 2026-02-18T20:33:33
|_Not valid after:  2036-02-16T20:33:33
|_ssl-date: TLS randomness does not represent time
995/tcp   open  ssl/pop3 Dovecot pop3d
|_ssl-date: TLS randomness does not represent time
| ssl-cert: Subject: commonName=enigma
| Subject Alternative Name: DNS:enigma
| Not valid before: 2026-02-18T20:33:33
|_Not valid after:  2036-02-16T20:33:33
|_pop3-capabilities: UIDL RESP-CODES SASL(PLAIN) CAPA TOP USER AUTH-RESP-CODE PIPELINING
2049/tcp  open  nfs      3-4 (RPC #100003)
34047/tcp open  status   1 (RPC #100024)
35183/tcp open  mountd   1-3 (RPC #100005)
40331/tcp open  mountd   1-3 (RPC #100005)
41651/tcp open  nlockmgr 1-4 (RPC #100021)
44697/tcp open  mountd   1-3 (RPC #100005)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

in port 2049 NFS mean Network File System, lets you mount a remote directory over the network and access it like a local folder. Designed for internal networks (Linux/Unix environments).

```
ajdev@rootbox:~$ showmount -e 10.129.33.28
Export list for 10.129.33.28:
/srv/nfs/onboarding *
```

`*` wildcard = no authentication, anyone on the network can mount

```
ajdev@rootbox:~/HTB/Enigma$ sudo mkdir -p /mnt/nfs
[sudo] password for ajdev:
ajdev@rootbox:~/HTB/Enigma$ sudo mount -t nfs 10.129.33.28:/srv/nfs/onboarding /mnt/nfs
ajdev@rootbox:~/HTB/Enigma$ ls -la /mnt/nfs
total 12
drwxr-xr-x 2 root root 4096 Feb 20 01:39 .
drwxr-xr-x 3 root root 4096 Jun 28 08:14 ..
-rw-r--r-- 1 root root 1751 Feb 20 01:38 New_Employee_Access.pdf
```

In pdf found:

```
Enigma Corp

IT Department - New Employee System Access
Employee:Kevin Mitchell
Department:Operations
Provisioned by:IT Department
Date:2024-03-01

Webmail Access
URL:http://mail001.enigma.htb
Username:kevin
Password:Enigma2024!
Please change your password upon first login.

For support contact: it@enigma.htb
This document contains confidential internal information intended solely for the recipient.

Unauthorized access, disclosure, or distribution is strictly prohibited.
Generated automatically by Enigma Corp Identity Management System.
```

then by using the credential login.




