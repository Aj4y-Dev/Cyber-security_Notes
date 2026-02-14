program[https://bugcrowd.com/engagements/mcdonalds-vdp-pro]

Resource[https://www.mcdonalds.com/]

```
POST /graphql/dropoffOptions?operation=dropoffOptions HTTP/2

{"operationName":"dropoffOptions","variables":{"cartId":""},"query":"query dropoffOptions($cartId: ID) {\n  dropoffOptions(cartId: aaaaaa) {\n    id\n    displayString\n    isDefault\n    isEnabled\n    placeholderText\n    disabledMessage\n    __typename\n  }\n}\n"}

"message":"ID cannot represent a non-string and non-integer value: aaaaaa",
```

```
POST /graphql/deviceAttestation?operation=deviceAttestation HTTP/2

if null 
{"message":"400 Bad Request","status":400,"errorId":"15d68f2a283a40759646700c33284ec2"}

and when "" response {"data":{"deviceAttestation":true},"extensions":{"requestInfo":{"version":1,"requestId":"e44c6d56-e0ff-4071-be04-40d7d083f7ea","correlationId":"e44c6d56-e0ff-4071-be04-40d7d083f7ea","ddb":true}}}  

and when abcdefg response {"data":{"deviceAttestation":true},"extensions":{"requestInfo":{"version":1,"requestId":"26f970ed-1456-43da-8dd4-36c136d91b72","correlationId":"26f970ed-1456-43da-8dd4-36c136d91b72","ddb":true}}}

if i change dd_device_id=dx_6fc3bf5d97364b56ab625a00f1664a0c;  
then response {"data":{"deviceAttestation":false},"extensions":{"requestInfo":{"version":1,"requestId":"c0e9dc2f-86ec-4d4a-806d-b8e19fec2d61","correlationId":"c0e9dc2f-86ec-4d4a-806d-b8e19fec2d61","ddb":true}}}

if i change dd_device_id=dx_!!!!!!!!!!!!;
then {"data":{"deviceAttestation":true},"extensions":{"requestInfo":{"version":1,"requestId":"d2d1e554-ef23-48d0-9ddb-6af4f97b1f51","correlationId":"d2d1e554-ef23-48d0-9ddb-6af4f97b1f51","ddb":true}}}

```

```
post can be done POST /unified-gateway/online-ordering/v1/best-store-delivery?business_id=5579&lat=41.50675883051008&lng=-71.59631319344044 HTTP/2

Host: mcdonalds.order.online

Cookie: ddweb_session_id=7aeed070-de6d-4388-94f1-966278922f7a:1:019c57d0-06d3-7e1a-967c-a1bd98a152c4; dd_device_session_id=26e75353-0d0f-4acb-8754-b84acf413d21; dd_delivery_correlation_id=3eb1f3d6-a674-4538-b718-d41315944088; dd_market_id=-1; dd_session_id=sx_d513a2e04f9e4e6db7c26359673a3c29; _cfuvid=xwkHWkuOGziS8QPfm__0xnjXJA.I0vlNUihcsjJaLxI-1770999777367-0.0.1.1-604800000; dd_market_id=-1; rskxRunCookie=0; rCookie=y8simdir9k9rlc3u2kd3mbmll3jl0f; lastRskxRun=1770999781410; ajs_anonymous_id=466166a4-966a-4158-92fd-6f68c6452055; amplitude_idundefinedorder.online=eyJvcHRPdXQiOmZhbHNlLCJzZXNzaW9uSWQiOm51bGwsImxhc3RFdmVudFRpbWUiOm51bGwsImV2ZW50SWQiOjAsImlkZW50aWZ5SWQiOjAsInNlcXVlbmNlTnVtYmVyIjowfQ==; __cf_bm=FDQT7ha6yY2w2McaZ5_Cd6xfgssjo.cI0riiEnCcgHM-1771001652-1.0.1.1-aVNzEmChGRiD6eipoccGAvITLTg8vcZ_l63HwUWtlgc5KrZ049Pcy9I2YxvP537ZJtWGAKOKQ22JaQUP1sWEpePBfAvvGu21Xeox3KkfTow; cf_clearance=TE2NwNjwAW0EMlhAYbq3jGcqCVNGb5BWt5Xh40Melb0-1771001654-1.2.1.1-xXPQ4_3SaXLGeSloZJSYWsiBOqbGXtH91dQTljPSXMlAUoM7sM1EanpA0_ynzoAB6uqPp.8exo_5KDvnb4EboFj1788WBQM07y8TywlI_ihc_QxChutJiucJLzFbkUzIoggffVLQfwgmXm1cew9mcxt2ZEzpSK0sWvIDZFuTNft5RZ.qwaZgJeG6WobkpselreEf5YYVFclILWPP7hMQr1EZoPVIPIUdbHZRQxwetsk; dd_device_id=dx_6fc3bf5d97364b56ab625a00f1664a0c; authState=d77ee771-a7d2-4129-90d0-16629d5375d4; amplitude_id_bf1b161b213fd0b483bb77e6e31ce20corder.online=eyJkZXZpY2VJZCI6IjA1MzZlOTg4LWI0ZDEtNGZhMS1hZWZiLWJmYWFhOTJhMzAxMFIiLCJ1c2VySWQiOm51bGwsIm9wdE91dCI6ZmFsc2UsInNlc3Npb25JZCI6MTc3MTAwMTY1Mjc4NywibGFzdEV2ZW50VGltZSI6MTc3MTAwMTY2NzkzNCwiZXZlbnRJZCI6MTgsImlkZW50aWZ5SWQiOjEsInNlcXVlbmNlTnVtYmVyIjoxOX0=

Sec-Ch-Ua-Full-Version-List:
Sec-Ch-Ua-Platform: "Linux"
Sec-Ch-Ua: "Chromium";v="145", "Not:A-Brand";v="99"
Sec-Ch-Ua-Bitness: ""
Sec-Ch-Ua-Model: ""
Sec-Ch-Ua-Mobile: ?0

Baggage: sentry-environment=production,sentry-release=storefront-web%402.3463.0,sentry-public_key=3b0f1428e20e8dbb74f77c6c332a21de,sentry-trace_id=9bedde646add4faabb851f7ace8cce1c,sentry-sample_rate=0.2,sentry-sampled=false
Sentry-Trace: 9bedde646add4faabb851f7ace8cce1c-8a610d811c44da3d-0
Traceparent: 00-289ccbf0af3066a46d9cb048b319bb1c-8364268b5f697914-00
Sec-Ch-Ua-Arch: ""
Sec-Ch-Ua-Full-Version: ""
X-Unified-Gateway-Generated-Source: v1
Dd-Ids: {"dd_device_id":"dx_6fc3bf5d97364b56ab625a00f1664a0c","dd_session_id":"sx_d513a2e04f9e4e6db7c26359673a3c29"}
Accept-Language: en
X-Experience-Id: storefront
User-Agent: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/145.0.0.0 Safari/537.36

Sec-Ch-Ua-Platform-Version: ""

Accept: */*
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: cors
Sec-Fetch-Dest: empty
Referer: https://mcdonalds.order.online/business/-5579
Accept-Encoding: gzip, deflate, br
Priority: u=1, i
Content-Length: 142

{"redirect_url":"https://order.online/store/mcdonalds-24507843?utm_source=dd-partner-link","best_store_id":24507843,"deliver_to_address":true} response HTTP/2 404 Not Found

Date: Fri, 13 Feb 2026 16:58:46 GMT
Content-Type: application/json
Cf-Ray: 9cd5df53e97f272a-KTM
Cf-Cache-Status: DYNAMIC
Strict-Transport-Security: max-age=31536000; includeSubDomains; preload
Vary: origin, Accept-Encoding
X-Content-Type-Options: nosniff
X-Envoy-Upstream-Service-Time: 2
X-Request-Id: f3f0d094-65e1-4d95-8b4e-c6f4e4763917
Server: cloudflare
Alt-Svc: h3=":443"; ma=86400

{"code":"resource_not_found","correlation_id":"869b3f79-6d2e-46d5-a82e-f8750a9bc771","localized_title":null,"localized_message":null,"internal_message":"Endpoint descriptor not found","detail":null}
```

let's test subdomain in it:

```
https://jobs.mcdonalds.com/ works redirect to -> career41.sapsf.com/career?company=mcdonaldsc&navBarLevel=MY_PROFILE

https://dev.mcdonalds.com/us/en-us.html

https://news.mcdonalds.com/us/en-us.html
response:
# Service Unavailable - DNS failure
The server is temporarily unable to service your request. Please try again later.
Reference #11.b187d817.1771002995.e88d8d3a
https://errors.edgesuite.net/11.b187d817.1771002995.e88d8d3a

https://stage.mcdonalds.com/us/en-us.html

`some sussh when i go 
https://digital.mcdonalds.com/ , https://preview.mcdonalds.com/, https://dna.mcdonalds.com/
response want username and password`

https://careers.mcdonalds.com/ 

https://qa.mcdonalds.com/us/en-us.html

qa, feeedbacks redirect to some other login section

https://delivery.mcdonalds.com/

https://order.mcdonalds.com/
# Access Denied
You don't have permission to access "http://order.mcdonalds.com/" on this server.
Reference #18.ab87d817.1771003354.51b5622c
https://errors.edgesuite.net/18.ab87d817.1771003354.51b5622c

https://corporate.mcdonalds.com/corpmcd/home.html

https://smetrics.mcdonalds.com/ -> blackout

https://newsletters.mcdonalds.com/ response It work's

inverstor subdomain goes to https://corporate.mcdonalds.com/corpmcd/investors.html

stories also goes to
https://corporate.mcdonalds.com/corpmcd/our-stories.html
```

```
https://careers.mcdonalds.com/ 

/corporate-careers
/university
```

```
https://newsletters.mcdonalds.com/


```