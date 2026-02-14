**CORS (Cross-Origin Resource Sharing):** Allows web apps to securely request resources from different domains, controlling which sites can access data.

**SOP (Same-Origin Policy):** A security rule that blocks web pages from accessing resources on a different origin (protocol, domain, or port) to prevent malicious data access.

---
### **Same-Origin Policy (SOP)** 

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

---
# Access-Control-Allow-Origin (ACAO)

### What is ACAO?

**Access-Control-Allow-Origin (ACAO)** is an HTTP **response header** used in **CORS (Cross-Origin Resource Sharing)**.  
It tells the browser **which origins are allowed** to access resources from the server.

Without this header, browsers enforce the **Same-Origin Policy (SOP)** and block cross-origin access.
Why ACAO is Needed?
By default:

- A website can only access resources from **the same origin**
- Cross-origin requests are **blocked by the browser**    

CORS relaxes this restriction **only when the server explicitly allows it**, using headers like ACAO.

## How ACAO Works (Basic Flow)

- Browser sends a request with an **Origin** header  
    Example:

```
Origin: https://evil.com
```

- Server checks:
    - Is this origin allowed?
- If allowed:
    - Server responds with:

```
Access-Control-Allow-Origin: https://evil.com
```

- Browser allows JavaScript access to the response
- If **not allowed**:
    - ACAO header is **missing**
    - Browser blocks access (even if the server responded)

CORS is enforced by the **browser**, not the server.

#### ACAO Configurations

1. **Single Origin**:
    - Configuration: `Access-Control-Allow-Origin: https://example.com`
    - Implication: Only requests originating from `https://example.com` are allowed. This is a secure configuration, as it restricts access to a known, trusted origin.
2. **Multiple Origins**:
    - Configuration: Dynamically set based on a list of allowed origins.
    - Implication: Allows requests from a specific set of origins. While this is more flexible than a single origin, it requires careful management to ensure that only trusted origins are included.
3. **Wildcard Origin**:
    - Configuration: `Access-Control-Allow-Origin: *`
    - Implication: Permits requests from any origin. This is the least secure configuration and should be used cautiously. It's appropriate for publicly accessible resources that don't contain sensitive information.
4. **With Credentials**:
    - Configuration: `Access-Control-Allow-Origin` set to a specific origin (wildcards not allowed), along with `Access-Control-Allow-Credentials: true`
    - Implication: Allows sending of credentials, such as cookies and HTTP authentication data, to be included in cross-origin requests. However, it's important to note that browsers will send cookies and authentication data without the Access-Control-Allow-Credentials header for simple requests like some GET and POST requests. For preflight requests that use methods other than GET/POST or custom headers, the Access-Control-Allow-Credentials header must be **true** for the browser to send credentials.

## ACAO Decision Flow (Server Side)

1. Check if request has an `Origin` header
2. If no origin:
    - Set `Access-Control-Allow-Origin: *`
3. If origin exists:
    - Check against allowed list
4. If allowed:
    - Set `Access-Control-Allow-Origin` to that origin
5. If not allowed:
    - Do not set ACAO → browser blocks access

This is the **core logic behind CORS enforcement**.

---
### Common CORS Misconfigurations & Secure Handling

### **1. Null Origin Misconfiguration**

- Occurs when a server accepts requests from the `"null"` origin (e.g., `file://` or `data:` URLs).
- Exploit: Attackers can host malicious HTML files that send requests, bypassing SOP.
- Mitigation: Explicitly reject or carefully validate `null` origins.
### **2. Bad Regex in Origin Checking**

- Happens when origin validation uses poorly written regex, e.g., `/example.com$/` allows `badexample.com`.
- Exploit: Attacker can register domains that match the flawed regex (`example.com.attacker.com`) and bypass restrictions.
- Mitigation: Test regex patterns rigorously and ensure they only allow intended origins.
### **3. Trusting Arbitrary Supplied Origin**

- Server echoes back the `Origin` header in `Access-Control-Allow-Origin`.
- Exploit: Any attacker-controlled origin can bypass SOP by sending a custom request.
- Mitigation: Use an allowlist of trusted origins and validate incoming requests against it.

---
### **Secure Handling of CORS Requests**

1. **Reject `null` origin requests** unless explicitly needed.
2. **Check origin against an allowlist** of trusted domains.
3. **Set `Access-Control-Allow-Origin` only for allowlisted origins**.
4. **Reject all other origins** to prevent unauthorized access.

**Note:** Using `Access-Control-Allow-Origin: *` can be safe for public, non-sensitive resources that don’t rely on cookies or authentication.