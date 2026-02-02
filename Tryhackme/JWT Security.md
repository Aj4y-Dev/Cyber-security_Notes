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

