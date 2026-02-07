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