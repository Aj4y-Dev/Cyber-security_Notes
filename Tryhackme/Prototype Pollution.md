**Prototype Pollution** is a vulnerability that occurs when an attacker can modify an object’s prototype, allowing malicious properties to be inherited by all objects in a JavaScript application. This can lead to unexpected behavior, security bypasses, and access to sensitive backend functionality.

### Why It Matters in JavaScript

- JavaScript uses **prototype-based inheritance**
- Objects inherit properties from `Object.prototype`
- If an attacker pollutes the prototype, **all objects are affected**
- Runtime modification is easy compared to class-based languages (Java, C++)

Class-based languages use static class definitions, making global runtime manipulation far harder, which is why prototype pollution is mainly a **JavaScript-specific risk**.

---

## How Prototype Pollution Works

- Vulnerability usually exists in **object merge**, **deep copy**, or **JSON parsing** logic
- Dangerous keys:
    - `__proto__`
    - `constructor.prototype`
    - `prototype`
- Attacker injects malicious properties into the prototype chain

Example impact:

- Adding admin privileges
- Overwriting security checks
- Triggering RCE in some frameworks

---
## Potential Risks

- Authentication and authorization bypass
- Application logic manipulation
- Denial of Service (DoS)
- Remote Code Execution (in severe cases)
- Data leakage or backend access