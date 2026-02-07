**CORS (Cross-Origin Resource Sharing):** Allows web apps to securely request resources from different domains, controlling which sites can access data.

**SOP (Same-Origin Policy):** A security rule that blocks web pages from accessing resources on a different origin (protocol, domain, or port) to prevent malicious data access.

---
### **Same-Origin Policy (SOP) 

**Definition:**  
SOP is a web security policy that controls how scripts and resources on one web page can interact with another. Access is only allowed if both pages share the same **origin**, which is determined by **protocol**, **hostname**, and **port**.

**Purpose:**

- Prevents malicious scripts on one page from accessing sensitive data on another.
- Ensures secure separation between resources from different origins.

**Examples:**

1. **Same domain, same port:**
    - `https://test.com:80` can access `https://test.com:80/about`.
    - Cannot access `https://test.com:8080` (different port).
2. **Different protocols:**
    - `http://test.com` cannot access `https://test.com` (protocol mismatch).

**Common Misconceptions:**

- **Applies to scripts only?** → No, SOP applies to all resources: scripts, images, stylesheets, frames, etc.
- **Blocks all cross-origin interactions?** → Not completely. Techniques like **CORS** or **postMessage** allow controlled cross-origin communication.
- **Same domain = same origin?** → Not necessarily. Protocol and port must also match.

**Decision Process (Browser Check):**

1. Protocol match → Yes/No
2. Hostname match → Yes/No
3. Port match → Yes/No

- **All match → Allowed**
- **Any mismatch → Blocked**

---
### **Cross-Origin Resource Sharing (CORS) 

**Definition:**  
CORS is a security mechanism that allows servers to specify which cross-origin requests are permitted. While **SOP** restricts access by default, CORS enables controlled exceptions via HTTP headers.

**Purpose:**

- Allows web pages to request resources from different domains safely.
- Browser enforces the policy by interpreting headers sent from the server.

**Key HTTP Headers in CORS:**

1. **Access-Control-Allow-Origin:** Specifies allowed domains (cannot be `*` if credentials are used).
2. **Access-Control-Allow-Methods:** Lists permitted HTTP methods (GET, POST, etc.).
3. **Access-Control-Allow-Headers:** Specifies which request headers are allowed.
4. **Access-Control-Max-Age:** Caches preflight results for a set duration.
5. **Access-Control-Allow-Credentials:** Allows cookies or credentials; must specify an explicit domain, not `*`.

**Common Use Cases:**

- APIs accessed from a different domain.
- Loading resources from CDNs (libraries, fonts).
- Web fonts shared across domains.
- Third-party widgets/plugins (social buttons, chatbots).
- Multi-domain authentication (SSO, OAuth tokens).

**Types of Requests:**

1. **Simple Requests:**
    - Methods: GET, HEAD, POST
    - Content-Type: `application/x-www-form-urlencoded`, `multipart/form-data`, `text/plain`
    - Sent directly; browser enforces CORS based on response headers.
2. **Preflight Requests:**
    - Triggered when requests use other methods, custom headers, or non-simple content types.
    - Browser sends an **OPTIONS** request first to check server permissions.
    - Server responds with CORS headers to approve the actual request.

**CORS Request Process:**

1. Browser sends request with **Origin** header.
2. Server checks if origin is allowed.
3. Server responds with **Access-Control-Allow-Origin** and other CORS headers.
4. Browser allows or blocks access based on headers.

