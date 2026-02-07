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

---
## Exploitation Techniques

### Client-Side

- DOM manipulation
- XSS chaining
- Modifying frontend logic (e.g., feature flags, role checks)
### Server-Side

- Polluting objects used in:
    - Access control
    - Template rendering
    - Configuration objects
- Can lead to privilege escalation or code execution

---
## Mitigation Techniques

- Block prototype-related keys (`__proto__`, `constructor`, `prototype`)
- Use safe object creation (`Object.create(null)`)
- Avoid unsafe deep merge functions
- Validate and sanitize user-controlled input
- Keep libraries and frameworks updated

---
## JavaScript Basics for Understanding Prototype Pollution

Before learning advanced attacks like **prototype pollution**, it’s important to understand how JavaScript handles **objects, classes, prototypes, and inheritance**, because prototype pollution abuses these exact concepts.

## Objects

- Objects are **containers for related data**
- Stored as **key–value pairs**
- Used to represent real-world entities (e.g., user profiles)

Example concept:

- A user object can store `name`, `age`, `followers`, `DoB`
- Objects help organise and manage application data

Key point:

> Objects are the **core building blocks** of JavaScript applications.

---
## Classes

- Classes are **blueprints** for creating objects
- Allow creation of multiple similar objects
- Improve structure and readability

Key ideas:

- `constructor()` initializes object properties
- `extends` enables inheritance
- `super()` calls the parent constructor

Important security note:

> JavaScript classes are **syntactic sugar** — internally, they still use prototypes.

---
## Prototypes

- Every JavaScript object has a **prototype**
- Prototypes form a **prototype chain**
- If a property isn’t found on an object, JS looks **up the prototype chain**

Key concepts:

- Prototypes act as shared templates
- Methods defined on prototypes are shared across objects
- Prototype-based behavior enables inheritance

Security relevance:

> If an attacker modifies a prototype, **all linked objects are affected**.

---
## Class vs Prototype

### Classes

- Structured and easier to understand
- Blueprint-style object creation
- Common in modern JavaScript
### Prototypes

- More dynamic and flexible
- Objects inherit directly from other objects
- Can be modified at runtime

Key takeaway:

> Classes are built on top of prototypes — prototypes are the real engine.

---
## Inheritance

Inheritance allows objects to **reuse properties and behaviors** from other objects.

Two models in JavaScript:

### Prototype-based Inheritance

- Objects inherit from other objects
- Implemented using `Object.create()`
- Properties are resolved via prototype chain
### Class-based Inheritance

- Uses `class` and `extends`
- Cleaner syntax
- Internally still prototype-based

Example concept:

- `ContentCreatorProfile` inherits common properties from `UserProfile`
- Adds its own properties like `posts` or `content`

---
## Inheritance

Inheritance allows objects to **reuse properties and behaviors** from other objects.

Two models in JavaScript:

### Prototype-based Inheritance

- Objects inherit from other objects
- Implemented using `Object.create()`
- Properties are resolved via prototype chain

### Class-based Inheritance

- Uses `class` and `extends`
- Cleaner syntax
- Internally still prototype-based

Example concept:

- `ContentCreatorProfile` inherits common properties from `UserProfile`
- Adds its own properties like `posts` or `content`