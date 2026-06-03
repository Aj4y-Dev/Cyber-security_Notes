
network enumeration:

```
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 8.9p1 Ubuntu 3ubuntu0.15 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   256 35:78:2e:79:0d:87:13:05:2f:53:8e:e7:3c:55:b6:4c (ECDSA)
|_  256 dd:56:8e:bc:da:b8:38:3e:9a:cd:0b:74:ee:53:85:f8 (ED25519)
80/tcp   open  http    nginx 1.18.0 (Ubuntu)
|_http-server-header: nginx/1.18.0 (Ubuntu)
|_http-title: Did not follow redirect to http://devhub.htb/
6274/tcp open  unknown
| fingerprint-strings:
|   DNSStatusRequestTCP, DNSVersionBindReqTCP, Help, RPCCheck, SSLSessionReq:
|     HTTP/1.1 400 Bad Request
|     Connection: close
|   GetRequest:
|     HTTP/1.1 200 OK
|     access-control-allow-credentials: true
|     content-length: 466
|     content-type: text/html; charset=utf-8
|     vary: Origin
|     Date: Wed, 03 Jun 2026 10:29:28 GMT
|     Connection: close
|     <!doctype html>
|     <html lang="en">
|     <head>
|     <meta charset="UTF-8" />
|     <link rel="icon" type="image/svg+xml" href="/mcp_jam.svg" />
|     <meta name="viewport" content="width=device-width, initial-scale=1.0" />
|     <title>MCPJam Inspector</title>
|     <script type="module" crossorigin src="/assets/index-DRYhT9Xb.js"></script>
|     <link rel="stylesheet" crossorigin href="/assets/index-XvFRNbCs.css">
|     </head>
|     <body>
|     <div id="root"></div>
|     </body>
|     </html>
|   HTTPOptions, RTSPRequest:
|     HTTP/1.1 204 No Content
|     access-control-allow-credentials: true
|     access-control-allow-methods: GET,HEAD,PUT,POST,DELETE,PATCH
|     vary: Origin
|     content-type: text/plain; charset=UTF-8
|     Date: Wed, 03 Jun 2026 10:29:29 GMT
|_    Connection: close
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port6274-TCP:V=7.94SVN%I=7%D=6/3%Time=6A200208%P=x86_64-pc-linux-gnu%r(
SF:GetRequest,290,"HTTP/1\.1\x20200\x20OK\r\naccess-control-allow-credenti
SF:als:\x20true\r\ncontent-length:\x20466\r\ncontent-type:\x20text/html;\x
SF:20charset=utf-8\r\nvary:\x20Origin\r\nDate:\x20Wed,\x2003\x20Jun\x20202
SF:6\x2010:29:28\x20GMT\r\nConnection:\x20close\r\n\r\n<!doctype\x20html>\
SF:n<html\x20lang=\"en\">\n\x20\x20<head>\n\x20\x20\x20\x20<meta\x20charse
SF:t=\"UTF-8\"\x20/>\n\x20\x20\x20\x20<link\x20rel=\"icon\"\x20type=\"imag
SF:e/svg\+xml\"\x20href=\"/mcp_jam\.svg\"\x20/>\n\x20\x20\x20\x20<meta\x20
SF:name=\"viewport\"\x20content=\"width=device-width,\x20initial-scale=1\.
SF:0\"\x20/>\n\x20\x20\x20\x20<title>MCPJam\x20Inspector</title>\n\x20\x20
SF:\x20\x20<script\x20type=\"module\"\x20crossorigin\x20src=\"/assets/inde
SF:x-DRYhT9Xb\.js\"></script>\n\x20\x20\x20\x20<link\x20rel=\"stylesheet\"
SF:\x20crossorigin\x20href=\"/assets/index-XvFRNbCs\.css\">\n\x20\x20</hea
SF:d>\n\x20\x20<body>\n\x20\x20\x20\x20<div\x20id=\"root\"></div>\n\x20\x2
SF:0</body>\n</html>\n")%r(HTTPOptions,F0,"HTTP/1\.1\x20204\x20No\x20Conte
SF:nt\r\naccess-control-allow-credentials:\x20true\r\naccess-control-allow
SF:-methods:\x20GET,HEAD,PUT,POST,DELETE,PATCH\r\nvary:\x20Origin\r\nconte
SF:nt-type:\x20text/plain;\x20charset=UTF-8\r\nDate:\x20Wed,\x2003\x20Jun\
SF:x202026\x2010:29:29\x20GMT\r\nConnection:\x20close\r\n\r\n")%r(RTSPRequ
SF:est,F0,"HTTP/1\.1\x20204\x20No\x20Content\r\naccess-control-allow-crede
SF:ntials:\x20true\r\naccess-control-allow-methods:\x20GET,HEAD,PUT,POST,D
SF:ELETE,PATCH\r\nvary:\x20Origin\r\ncontent-type:\x20text/plain;\x20chars
SF:et=UTF-8\r\nDate:\x20Wed,\x2003\x20Jun\x202026\x2010:29:29\x20GMT\r\nCo
SF:nnection:\x20close\r\n\r\n")%r(RPCCheck,2F,"HTTP/1\.1\x20400\x20Bad\x20
SF:Request\r\nConnection:\x20close\r\n\r\n")%r(DNSVersionBindReqTCP,2F,"HT
SF:TP/1\.1\x20400\x20Bad\x20Request\r\nConnection:\x20close\r\n\r\n")%r(DN
SF:SStatusRequestTCP,2F,"HTTP/1\.1\x20400\x20Bad\x20Request\r\nConnection:
SF:\x20close\r\n\r\n")%r(Help,2F,"HTTP/1\.1\x20400\x20Bad\x20Request\r\nCo
SF:nnection:\x20close\r\n\r\n")%r(SSLSessionReq,2F,"HTTP/1\.1\x20400\x20Ba
SF:d\x20Request\r\nConnection:\x20close\r\n\r\n");
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

devhub.htb:6274/#settings found the version of the system

MCPJam Version: v1.4.2 found CVE-2026-23744

```
https://github.com/MCPJam/inspector/security/advisories/GHSA-232v-j27c-5pp6

i found that their is vlunerbility that the /api/mcp/connect is vlunerable to remote code execution (RCE) attack can be triggered by sending a simple HTTP request to the target host running MCPJam inspector


```


