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
