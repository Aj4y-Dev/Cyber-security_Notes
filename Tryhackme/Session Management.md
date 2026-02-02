Session management is the process of managing these sessions and ensuring that they remain secure.

### Session Management Lifecycle

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

