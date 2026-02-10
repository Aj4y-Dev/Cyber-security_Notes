OAuth 2.0 is the commonly used authorisation framework / **authorization framework** that allows applications to access user data **without sharing user passwords**. Understanding these roles is critical for finding OAuth misconfigurations and vulnerabilities. which allows for CSRF, XSS, data leakage and exploitation of other vulnerabilities.

As a pentester or a secure coder, it is essential to understand these concepts to pentest a website or write code without a vulnerability. To make these concepts more relatable, we will explain them through a daily routine example: using a coffee shop's mobile app to order and pay for coffee.
### 1. Resource Owner

- The **user** who owns the data and can grant permission.
- Controls consent and access.
- **Example:** You, the coffee shop customer, who owns your account and payment data.
### 2. Client

- The **application** requesting access to user data.
- Can be a web app, mobile app, or backend service.
- **Example:** Coffee shop mobile/web app used to order coffee.
### 3. Authorization Server

- Authenticates the resource owner.
- Issues **access tokens** after user consent.
- **Example:** Coffee shop backend handling login and permissions.
### 4. Resource Server

- Hosts **protected resources** (data).
- Validates access tokens before serving data.
- **Example:** Database storing orders, account info, and payments.
### 5. Authorization Grant

- Proof that the user authorized the client.
- Used to obtain an access token.
- Common types:
    - Authorization Code (most secure)
    - Implicit (legacy, risky)
    - Password Credentials (dangerous)
    - Client Credentials (machine-to-machine)
- **Example:** Login approval given when you sign into the app.
### 6. Access Token

- Short-lived credential used to access APIs.
- Has **scope** and expiration.
- Prevents repeated password use.
- **Example:** Token allowing the app to place orders for you.
### 7. Refresh Token

- Used to get a **new access token** without re-login.
- Long-lived → high-value target in attacks.
- **Example:** Keeps you logged in after token expiry.
### 8. Redirect URI

- Pre-registered URL where the authorization server redirects the user.
- Prevents token leakage and phishing.
- **Example:** App page you return to after login.
### 9. Scope

- Defines **what actions/data** the client can access.
- Enforces least privilege.
- **Example:** `read:orders`, `write:payments`
### 10. State Parameter

- Protects against **CSRF attacks**.
- Must be unpredictable and validated.
- **Example:** Random value sent during login and checked on return.
### 11. Authorization Endpoint vs Token Endpoint

- **Authorization Endpoint**
    - User login + consent
- **Token Endpoint**
    - Exchange grant/refresh token → access token

---
## OAuth 2.0 Grant Types

OAuth 2.0 defines **grant types** that specify how a client application obtains an **access token** to access protected resources.
### 1. Authorization Code Grant

- **Most secure and widely used**
- Designed for **server-side applications** (PHP, Java, .NET, etc.)
- Flow:
    - User authenticates on the authorization server
    - Server returns an **authorization code**
    - Client exchanges the code for an **access token** (server-to-server)
- **Access token is never exposed to the browser**
- Supports **refresh tokens**
- **Best choice for web applications**

---
### 2. Implicit Grant

- Designed for **browser-based and mobile apps**
- Access token is returned **directly in the URL fragment**
- No authorization code exchange
- **Faster but less secure**
- ❌ Access token exposed to browser and history
- ❌ No refresh token support
- **Now considered deprecated** in modern OAuth implementations

---
### 3. Resource Owner Password Credentials Grant

- Used only for **highly trusted (first-party) applications**
- User provides **username and password directly to the client**
- Client exchanges credentials for an access token
- Simple and direct flow
- ❌ Less secure (credentials shared with client)
- ❌ Not suitable for third-party apps

---
### 4. Client Credentials Grant

- Used for **server-to-server communication**
- No user involvement
- Client authenticates using **client ID and client secret**
- Authorization server returns an access token
- Ideal for **backend services, APIs, microservices**
- **No user data exposure**

---
