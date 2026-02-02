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

