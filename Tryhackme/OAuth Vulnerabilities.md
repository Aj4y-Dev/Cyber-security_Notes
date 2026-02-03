OAuth 2.0 is an **authorization framework** that allows applications to access user data **without sharing user passwords**. Understanding these roles is critical for finding OAuth misconfigurations and vulnerabilities.

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
