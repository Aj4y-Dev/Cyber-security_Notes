Session management is the process of managing these sessions and ensuring that they remain secure.

## Session Management Lifecycle

### **Session Creation**

- A session can be created **before login** (to track users) or **after login** (authenticated session).
- After successful login, the server gives the user a **session ID/token**.
- This session ID represents the user’s identity and permissions.
- If session IDs are predictable or poorly generated → **session fixation/hijacking risk**.
### **Session Tracking**

- The session ID is sent with **every request** (usually via cookies).
- Since HTTP is stateless, the server uses the session ID to:
    - Identify the user
    - Check permissions
- Weak handling here can allow **session hijacking or impersonation**.
### **Session Expiry**

- Sessions must have a **time limit**.
- If the session expires, the old session ID should be rejected.
- User must **log in again** to get a new session.
- Missing or long expiry → risk of **stolen session reuse**.
### **Session Termination**

- Happens when a user **logs out manually**.
- Session should be invalidated immediately, even if it hasn’t expired.
- Poor termination allows **persistent unauthorized access**.

To understand the common vulnerabilities in session management, we first need to examine authentication and authorisation. While they sound the same and are often confused, each plays a critical and unique role in session management. To better explain the differences, let's examine the IAAA model:

## IAAA Model

### **Identification**

- User **claims an identity**.
- Usually done by submitting a **username or email**.
- No proof yet—just saying _“I am this user.”_
### **Authentication**

- User **proves their identity**.
- Done using **passwords, OTPs, tokens, etc.**
- If valid → **session is created**.
- Weak auth leads to **credential stuffing, brute force, bypasses**.
### **Authorisation**

- Checks **what the user is allowed to do**.
- Based on **roles/permissions** linked to the session.
- Example: user can _view_ data but not _edit_ it.
- Broken authorisation = **IDOR, privilege escalation**.
### **Accountability**

- **Logs user actions** tied to their session.
- Records _who did what and when_.
- Critical for **incident response & forensics**.
- Missing logs = attacker activity goes unnoticed.

## **IAAA + Session Management (Big Picture)**

- **Authentication → Session Creation**
    - Valid login generates a session ID.
- **Authorisation → Session Tracking**
    - Every request checks permissions using the session.
- **Accountability → Session Logging**
    - All actions are logged with the session ID.

### **Cookie-Based Sessions**

- Server sends a session ID using `Set-Cookie`.
- Browser **automatically stores and sends** the cookie with every request.

```
#Set-Cookie header:

Set-Cookie: session=12345;
```

- Security can be improved using cookie flags:
    - **Secure** → only sent over HTTPS
    - **HttpOnly** → not accessible via JavaScript (protects from XSS)
    - **Expires** → defines cookie lifetime
    - **SameSite** → helps prevent CSRF
- **Main risks**:
    - Vulnerable to **CSRF**
    - If HttpOnly is missing → **XSS can steal cookies**

### **Token-Based Sessions**

- Server returns a **token** (often JWT) after login.
- Token is stored in **LocalStorage**.
- JavaScript must manually attach it to requests using:
`
```
Authorization: Bearer <token>
```

- No automatic browser protections → **developer must secure it properly**.
- **Main risks**:
    - Vulnerable to **XSS** (tokens can be stolen from LocalStorage)
    - Poor validation → token tampering

| Cookie-Based            | Token-Based               |
| ----------------------- | ------------------------- |
| Auto-sent by browser    | Manually added via JS     |
| Built-in security flags | No enforced protections   |
| Vulnerable to CSRF      | CSRF mostly avoided       |
| Domain-restricted       | Good for distributed apps |
| Traditional web apps    | APIs, SPA, mobile apps    |

---
## Session Management Security

### **1. Session Creation Vulnerabilities**

**Weak Session Values**

- Session IDs are **guessable or predictable**.
    
- Example: Base64(username) as session ID.
    
- Leads to **session hijacking**.
    

**Controllable Session Values**

- Common with **JWTs**.
    
- If signature isn’t verified or uses weak secrets → attacker can **forge tokens**.
    
- Results in **account takeover**.
    

**Session Fixation**

- Session is created **before login** and **not rotated after login**.
    
- Attacker steals the pre-login session ID and waits for user to authenticate.
    
- Result: attacker gains the authenticated session.
    

**Insecure Session Transmission**

- Session data exposed during redirects (e.g., SSO flows).
    
- Open or attacker-controlled redirects can **leak session info**.
    
- Leads to **session hijacking**.
    

---

### **2. Session Tracking Vulnerabilities**

**Authorisation Bypass**

- App fails to properly check permissions tied to a session.

Types:

- **Vertical bypass** → user gains admin-level access
- **Horizontal bypass** → user accesses _other users’ data_ (IDOR)

Fix requires:

- Verifying **user identity from session**
- Matching user to requested data

**Insufficient Logging**

- Actions are not logged per session.
- Makes incident investigation impossible.
- Both **allowed and denied actions** must be logged.

---
### **3. Session Expiry Issues**

- Sessions last **too long**.
- Stolen sessions remain usable.
- High-risk apps (banking) need **short lifetimes**.
- Long sessions should check **location/IP changes** and invalidate if suspicious.

---
### **4. Session Termination Issues**

- Logout does not invalidate session **server-side**.
- Attacker keeps access even after user logs out.
- For tokens:
    - Use **blocklists**
    - Allow users to **terminate all active sessions**
- After **password reset**, all sessions should be killed.