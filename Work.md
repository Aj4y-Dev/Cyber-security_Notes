resource[https://uat.resv.buddhatech.info/]


```
nmap 13.228.112.54 -p- -Pn -sV 
Starting Nmap 7.95 ( https://nmap.org ) at 2026-02-15 11:54 +0545 Stats: 0:00:52 elapsed; 0 hosts completed (1 up), 1 undergoing Connect Scan Connect Scan Timing: About 37.93% done; ETC: 11:56 (0:01:25 remaining) Stats: 0:01:43 elapsed; 0 hosts completed (1 up), 1 undergoing Connect Scan Connect Scan Timing: About 73.30% done; ETC: 11:56 (0:00:38 remaining) Nmap scan report for ec2-13-228-112-54.ap-southeast-1.compute.amazonaws.com (13.228.112.54) Host is up (0.072s latency). Not shown: 65522 filtered tcp ports (no-response) 

PORT STATE SERVICE VERSION 
22/tcp open ssh OpenSSH 8.9p1 Ubuntu 3ubuntu0.13 (Ubuntu Linux; protocol 2.0) 
80/tcp open http nginx (reverse proxy) 
81/tcp open http nginx (reverse proxy) 82/tcp open http nginx (reverse proxy) 
443/tcp open ssl/http nginx (reverse proxy) 
3306/tcp open mysql MariaDB 10.3.23 or earlier (unauthorized) 5678/tcp open http nginx (reverse proxy) 
6969/tcp open http nginx (reverse proxy) 8811/tcp open http nginx (reverse proxy) 
9099/tcp open http nginx (reverse proxy) 
9955/tcp closed alljoyn-stm 9956/tcp closed alljoyn 63306/tcp closed unknown Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel https://www.buddhatech.info/ behind cloudflare but oh well http://13.228.112.54:8811/ origin ip
```

```
made in PHP 8.3.27 — Laravel 11.22.0
```

```
ajdev@rootbox:~$ ffuf -w /usr/share/seclists/Discovery/Web-Content/raft-large-directories-lowercase.txt -u "https://13.228.112.54/api/FUZZ" -fs 615

        /'___\  /'___\           /'___\
       /\ \__/ /\ \__/  __  __  /\ \__/
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/
         \ \_\   \ \_\  \ \____/  \ \_\
          \/_/    \/_/   \/___/    \/_/

       v2.1.0-dev
________________________________________________

 :: Method           : GET
 :: URL              : https://13.228.112.54/api/FUZZ
 :: Wordlist         : FUZZ: /usr/share/seclists/Discovery/Web-Content/raft-large-directories-lowercase.txt
 :: Follow redirects : false
 :: Calibration      : false
 :: Timeout          : 10
 :: Threads          : 40
 :: Matcher          : Response status: 200-299,301,302,307,401,403,405,500
 :: Filter           : Response size: 615
________________________________________________

register  [Status: 405, Size: 238408, Words: 43511, Lines: 2434, Duration: 784ms]
logout    [Status: 405, Size: 238404, Words: 43511, Lines: 2434, Duration: 812ms]
login     [Status: 405, Size: 238402, Words: 43511, Lines: 2434, Duration: 791ms]
users             [Status: 302, Size: 354, Words: 60, Lines: 12, Duration: 326ms]
up              [Status: 200, Size: 2143, Words: 747, Lines: 51, Duration: 301ms]
agents            [Status: 302, Size: 354, Words: 60, Lines: 12, Duration: 216ms]
forgot-password   [Status: 405, Size: 238422, Words: 43511, Lines: 2434, Duration: 424ms]
me                [Status: 302, Size: 354, Words: 60, Lines: 12, Duration: 191ms]
countries         [Status: 302, Size: 354, Words: 60, Lines: 12, Duration: 228ms]
areas             [Status: 302, Size: 354, Words: 60, Lines: 12, Duration: 212ms]
sss      [Status: 500, Size: 305367, Words: 71027, Lines: 3946, Duration: 1249ms]
permissions       [Status: 302, Size: 354, Words: 60, Lines: 12, Duration: 206ms]
reset-password    [Status: 405, Size: 238420, Words: 43511, Lines: 2434, Duration: 697ms]
roles            [Status: 302, Size: 354, Words: 60, Lines: 12, Duration: 197ms]
```


![[Pasted image 20260215130325.png]]

```
/api/up -> get check working or not
```

![[Pasted image 20260215131815.png]]

