first of all network enumeration:

```
# found the only two port is open:

PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.11 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   3072 d2:89:f2:f5:d0:2b:02:49:70:de:f5:80:db:b9:13:b7 (RSA)
|   256 77:0d:1a:bd:b0:37:45:70:39:02:0b:ce:f8:80:4d:8c (ECDSA)
|_  256 3c:a0:98:9d:a6:32:5f:93:be:9e:ae:9a:67:73:14:97 (ED25519)
1337/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))
| http-cookie-flags:
|   /:
|     PHPSESSID:
|_      httponly flag not set
|_http-title: Login
|_http-server-header: Apache/2.4.41 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

in the port 1337 their is a login page:

![[Pasted image 20260211163442.png]]

