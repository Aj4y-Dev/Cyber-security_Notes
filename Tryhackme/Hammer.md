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

Now lets fuzz the directory using the different wordlists i found some things:

```
ajdev@rootbox:~/THM/Hammer$ ffuf -w /usr/share/seclists/Discovery/Web-Content/raft-medium- -u "http://10.48.154.110:1337/FUZZ" -fs 280

index.php        [Status: 200, Size: 1326, Words: 351, Lines: 37, Duration: 40ms]
config.php             [Status: 200, Size: 0, Words: 1, Lines: 1, Duration: 41ms]
logout.php             [Status: 302, Size: 0, Words: 1, Lines: 1, Duration: 40ms]
dashboard.php          [Status: 302, Size: 0, Words: 1, Lines: 1, Duration: 42ms]
reset_password.php     [Status: 200, Size: 1664, Words: 484, Lines: 48, Duration: 39ms]
javascript         [Status: 301, Size: 326, Words: 20, Lines: 10, Duration: 38ms]
phpmyadmin         [Status: 301, Size: 326, Words: 20, Lines: 10, Duration: 37ms]
vendor             [Status: 301, Size: 322, Words: 20, Lines: 10, Duration: 36ms]
```

in vendor their is a lot of file but none of them seem related.

in /phpmyadmin their is also a login page:

![[Pasted image 20260211165142.png]]

i found this in