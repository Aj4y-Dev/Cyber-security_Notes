network enumeration:

```
PORT      STATE SERVICE  VERSION
22/tcp    open  ssh      OpenSSH 8.2p1 Ubuntu 4ubuntu0.11 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   3072 32:d0:c7:ed:b3:d0:ce:55:44:ad:4d:37:94:57:5e:10 (RSA)
|   256 90:77:3b:67:6d:78:1b:d4:8e:de:4d:89:5f:31:5d:ed (ECDSA)
|_  256 22:03:59:9d:2c:58:69:59:bd:18:b3:d9:4f:f5:2b:24 (ED25519)
25/tcp    open  smtp     Postfix smtpd
|_ssl-date: TLS randomness does not represent time
| ssl-cert: Subject: commonName=ip-10-10-31-82.eu-west-1.compute.internal
| Subject Alternative Name: DNS:ip-10-10-31-82.eu-west-1.compute.internal
| Not valid before: 2021-11-10T16:53:34
|_Not valid after:  2031-11-08T16:53:34
|_smtp-commands: mail.filepath.lab, PIPELINING, SIZE 10240000, VRFY, ETRN, STARTTLS, ENHANCEDSTATUSCODES, 8BITMIME, DSN, SMTPUTF8, CHUNKING
110/tcp   open  pop3     Dovecot pop3d
| ssl-cert: Subject: commonName=ip-10-10-31-82.eu-west-1.compute.internal
| Subject Alternative Name: DNS:ip-10-10-31-82.eu-west-1.compute.internal
| Not valid before: 2021-11-10T16:53:34
|_Not valid after:  2031-11-08T16:53:34
|_pop3-capabilities: SASL TOP UIDL STLS PIPELINING AUTH-RESP-CODE CAPA RESP-CODES
|_ssl-date: TLS randomness does not represent time
143/tcp   open  imap     Dovecot imapd (Ubuntu)
|_ssl-date: TLS randomness does not represent time
| ssl-cert: Subject: commonName=ip-10-10-31-82.eu-west-1.compute.internal
| Subject Alternative Name: DNS:ip-10-10-31-82.eu-west-1.compute.internal
| Not valid before: 2021-11-10T16:53:34
|_Not valid after:  2031-11-08T16:53:34
|_imap-capabilities: LITERAL+ OK have IDLE more listed Pre-login capabilities LOGINDISABLEDA0001 IMAP4rev1 post-login SASL-IR ID LOGIN-REFERRALS ENABLE STARTTLS
993/tcp   open  ssl/imap Dovecot imapd (Ubuntu)
|_imap-capabilities: post-login OK have IDLE more listed Pre-login capabilities AUTH=PLAIN IMAP4rev1 AUTH=LOGINA0001 SASL-IR ID LOGIN-REFERRALS ENABLE LITERAL+
|_ssl-date: TLS randomness does not represent time
| ssl-cert: Subject: commonName=ip-10-10-31-82.eu-west-1.compute.internal
| Subject Alternative Name: DNS:ip-10-10-31-82.eu-west-1.compute.internal
| Not valid before: 2021-11-10T16:53:34
|_Not valid after:  2031-11-08T16:53:34
995/tcp   open  ssl/pop3 Dovecot pop3d
|_pop3-capabilities: SASL(PLAIN LOGIN) TOP UIDL USER PIPELINING AUTH-RESP-CODE CAPA RESP-CODES
|_ssl-date: TLS randomness does not represent time
| ssl-cert: Subject: commonName=ip-10-10-31-82.eu-west-1.compute.internal
| Subject Alternative Name: DNS:ip-10-10-31-82.eu-west-1.compute.internal
| Not valid before: 2021-11-10T16:53:34
|_Not valid after:  2031-11-08T16:53:34
4000/tcp  open  http     Node.js (Express middleware)
|_http-title: Sign In
50000/tcp open  http     Apache httpd 2.4.41 ((Ubuntu))
| http-cookie-flags:
|   /:
|     PHPSESSID:
|_      httponly flag not set
|_http-server-header: Apache/2.4.41 (Ubuntu)
|_http-title: System Monitoring Portal
Service Info: Host:  mail.filepath.lab; OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

