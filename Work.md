resource[https://uat.resv.buddhatech.info/]


```
nmap 13.228.112.54 -p- -Pn -sV 
Starting Nmap 7.95 ( https://nmap.org ) at 2026-02-15 11:54 +0545 Stats: 0:00:52 elapsed; 0 hosts completed (1 up), 1 undergoing Connect Scan Connect Scan Timing: About 37.93% done; ETC: 11:56 (0:01:25 remaining) Stats: 0:01:43 elapsed; 0 hosts completed (1 up), 1 undergoing Connect Scan Connect Scan Timing: About 73.30% done; ETC: 11:56 (0:00:38 remaining) Nmap scan report for ec2-13-228-112-54.ap-southeast-1.compute.amazonaws.com (13.228.112.54) Host is up (0.072s latency). Not shown: 65522 filtered tcp ports (no-response) 

PORT STATE SERVICE VERSION 
22/tcp open ssh OpenSSH 8.9p1 Ubuntu 3ubuntu0.13 (Ubuntu Linux; protocol 2.0) 
80/tcp open http nginx (reverse proxy) 
81/tcp open http nginx (reverse proxy) 82/tcp open http nginx (reverse proxy) 
443/tcp open ssl/http nginx (reverse proxy) 
3306/tcp open mysql MariaDB 10.3.23 or earlier (unauthorized) 
5678/tcp open http nginx (reverse proxy) 
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

```
SQLSTATE[HY000] [1045] Access denied for user 'txuraz'@'localhost' (using password: YES) (Connection: mysql, SQL: select * from `countries` order by `country_name` asc)

app/Http/Controllers/Location/CountryController.php :53

    public function countriesList()
    {
        $countryNamesFromDb = Country::all()->map(fn($country) => Str::replace(" ", "_", Str::lower($country->country_name)) . ".json")->flip()->toArray();
//        $a = scandir("/home/rinjha/WebstormProjects/agentmgt/public/countries");
        $a = scandir(public_path('countries'));
        $files = array_flip($a);
        $remaining = $countryNamesFromDb;
        $remainingFiles = $files;
        foreach ($countryNamesFromDb as $name => $key) {
            if (array_key_exists($name, $files)) {
                unset($remaining[$name]);
                unset($remainingFiles[$name]);
            }
        }
```

```
user: txuraz
host: localhost (127.0.1.230.142)

 $a = scandir("/home/rinjha/WebstormProjects/agentmgt/public/countries");
```

```
POST /api/reset-password  HTTP/1.1
Host: 13.228.112.54
Accept-Language: en-US,en;q=0.9
Content-Type: application/json
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/145.0.0.0 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Ch-Ua: "Chromium";v="145", "Not:A-Brand";v="99"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "Linux"
Sec-Fetch-Site: none
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Accept-Encoding: gzip, deflate, br
Priority: u=0, i
Connection: keep-alive
Content-Length: 51

{
	"user":"txuraz",
	"password":"password123"	
}

//this dont work for now only post req allowed
```

```
ajdev@rootbox:~$ nmap -p 3306 --script=mysql-info 13.228.112.54
Starting Nmap 7.94SVN ( https://nmap.org ) at 2026-02-15 13:34 +0545
Nmap scan report for 13.228.112.54
Host is up (0.073s latency).

PORT     STATE    SERVICE
3306/tcp filtered mysql

Nmap done: 1 IP address (1 host up) scanned in 13.85 seconds
```


```
GET /countries/nepal.json HTTP/1.1
Host: 13.228.112.54
Accept-Language: en-US,en;q=0.9
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/145.0.0.0 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Ch-Ua: "Chromium";v="145", "Not:A-Brand";v="99"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "Linux"
Sec-Fetch-Site: none
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Accept-Encoding: gzip, deflate, br
Priority: u=0, i
Connection: keep-alive

# Response:

HTTP/1.1 200 OK
Date: Sun, 15 Feb 2026 08:02:20 GMT
Content-Type: application/json
Connection: keep-alive
Access-Control-Allow-Origin: *
Last-Modified: Thu, 15 May 2025 11:23:59 GMT
ETag: W/"84819-1747308239302"
Cache-Control: no-cache
Server: BuddhaTech
Access-Control-Allow-Origin: *
Access-Control-Allow-Methods: GET, POST, OPTIONS
Access-Control-Allow-Headers: Origin, X-Requested-With, Content-Type, Accept, Authorization
Access-Control-Allow-Credentials: true
Content-Length: 84819

{
    "type": "FeatureCollection",
    "features": [
        {
            "type": "Feature",
            "geometry": {
                "type": "Polygon",
                "coordinates": [
                    [
                        [
                            81.61880493164062,
                            30.44512194010915
                        ],
                        [
                            81.61331176757812,
                            30.413150465068878
                        ]
                        in here so much things
                        ,
                        [
                            81.61880493164062,
                            30.44512194010915
                        ]
                    ]
                ]
            },
            "properties": {}
        }
    ]
}
```

![[Pasted image 20260215143103.png]]

![[Pasted image 20260215143305.png]]

Scope[uat.resv.buddhatech.info]

```
GET /static/test HTTP/1.1
Host: uat.resv.buddhatech.info -> This is test.

GET /api/health HTTP/1.1
Host: uat.resv.buddhatech.info -> {"redis":true,"database":true}

GET /api/v1/email HTTP/1.1
Host: uat.resv.buddhatech.info 
|
HTTP/1.1 401 Unauthorized
{"payload":null,"code":401,"success":false,"message":"Client error","kind":null,"do_logout":null}

GET /api/v1/crew HTTP/1.1
Host: uat.resv.buddhatech.info
|
HTTP/1.1 401 Unauthorized
{"payload":null,"code":401,"success":false,"message":"Client error","kind":null,"do_logout":null}

GET /api/v1/apis HTTP/1.1
Host: uat.resv.buddhatech.info
|
HTTP/1.1 401 Unauthorized
{"payload":null,"code":401,"success":false,"message":"Client error","kind":null,"do_logout":null}

GET /api/v1/sso/logout HTTP/1.1
Host: uat.resv.buddhatech.info
|
HTTP/1.1 200 OK
set-cookie: sso_session=None; HttpOnly; Max-Age=7200; Path=/api/v1/sso; SameSite=lax; Secure
{"payload":null,"message":"Logged out.","success":true,"code":200}

GET /api/v1/sso/me HTTP/1.1
Host: uat.resv.buddhatech.info
|
HTTP/1.1 401 Unauthorized
{"payload":null,"code":401,"success":false,"message":"Client error","kind":null,"do_logout":null}


```



endPoints:{"https://uat.resv.buddhatech.info/api/v1/departure/departure-history/{ticket_no}":"GET"}

{"https://uat.resv.buddhatech.info/api/v1/offload-reasons/":"GET",

"https://uat.resv.buddhatech.info/api/v1/departure/seat-parameter/flight/{flight_id}/aircraft/{aircraft_id}":"GET",

"https://uat.resv.buddhatech.info/api/v1/departure/dcs-passengers":"GET"},"DCS 

Save Info":{"https://uat.resv.buddhatech.info/api/v1/departure/check-in":"POST"}

{"https://uat.resv.buddhatech.info/api/v1/departure/dcs-open":"PATCH"}

:{"https://uat.resv.buddhatech.info/api/v1/departure/cargo-load-offload":"POST"},

{"https://uat.resv.buddhatech.info/api/v1/departure/baggage-load-offload":"POST"}

{"https://uat.resv.buddhatech.info/api/v1/departure/departure-history/{ticket_no}":"GET"}

{"https://uat.resv.buddhatech.info/api/v1/crew":"GET","https://uat.resv.buddhatech.info/api/v1/departure/assign-crew":"POST"}

"https://uat.resv.buddhatech.info/api/v1/departure/assign-crew":"POST"

"https://uat.resv.buddhatech.info/api/v1/departure/assign-aircraft":"POST","https://uat.resv.buddhatech.info/api/v1/flight/sector-pair/{sector_pair_id}/flight-date/{flight_date}":"GET"

"https://uat.resv.buddhatech.info/api/v1/flight-booking/internal/hold":"POST","https://uat.resv.buddhatech.info/api/v1/seat-availability/":"GET"

https://uat.resv.buddhatech.info/api/v1/sales-agent-guaranteed-amount/{agent_name_id}":"GET"

https://uat.resv.buddhatech.info/api/v1/fare/taxes/":"GET"

