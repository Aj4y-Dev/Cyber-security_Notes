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

```
let user = {
  name: "Ben S",
  age: 25,
  followers: 200
};

console.log(user.name);        // Ben S
console.log(user.followers);  // 200
```

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

```
class UserProfile {
  greet() {
    return "Hello user";
  }
}

let u1 = new UserProfile();

console.log(u1.greet()); // Hello user

Now check where greet() actually lives:

console.log(u1.hasOwnProperty("greet")); 
// false → not inside u1

console.log(UserProfile.prototype.hasOwnProperty("greet")); 
// true → stored in prototype

# Methods defined in classes live in the prototype, not the object.
```

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

```
let obj = {};

console.log(obj.toString()); 

# toString() is NOT inside obj.

console.log(obj.hasOwnProperty("toString")); // false
console.log(Object.prototype.hasOwnProperty("toString")); // true

Prototype chain:
obj → Object.prototype → null
```

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

```
let baseUser = {
  role: "user",
  canLogin: true
};

let adminUser = Object.create(baseUser);
adminUser.role = "admin";

console.log(adminUser.role);      // admin
console.log(adminUser.canLogin);  // true (inherited)
```
### Class-based Inheritance

- Uses `class` and `extends`
- Cleaner syntax
- Internally still prototype-based

```
class User {
  isUser() {
    return true;
  }
}

class Admin extends User {
  isAdmin() {
    return true;
  }
}

let a = new Admin();

console.log(a.isAdmin()); // true
console.log(a.isUser());  // true

Under the hood:
a → Admin.prototype → User.prototype → Object.prototype
```

Example concept:

- `ContentCreatorProfile` inherits common properties from `UserProfile`
- Adds its own properties like `posts` or `content`

---
## Prototype Chain (How Property Lookup Works)

1. JavaScript checks the object itself
2. If not found, checks its prototype
3. Continues up the prototype chain
4. Stops at `Object.prototype`

```
let innocentObject = {};

console.log(innocentObject.isAdmin); // undefined

// POLLUTION
Object.prototype.isAdmin = true;

let user1 = {};
let user2 = {};

console.log(user1.isAdmin); // true 😈
console.log(user2.isAdmin); // true 😈
```

This hierarchy allows:

- Code reuse
- Shared behavior
- Flexible object design

```
Final Mental Model:

Object → Prototype → Prototype → Object.prototype
             ↑
       attacker modifies here

# this does NOT mean there are always _two prototypes_.  
It was showing “prototype chain levels”, not actual objects named Prototype.
```

#imp JavaScript inheritance is powerful but dangerous when user input can modify objects or prototypes. Understanding objects, classes, prototypes, and inheritance is **mandatory** before exploiting or mitigating prototype pollution.

---
### What is Prototype Pollution?

Prototype pollution is a vulnerability where an attacker **modifies a shared prototype**, causing malicious properties or methods to appear in **all objects that inherit from it**.

In JavaScript, objects inherit behavior through prototypes. If an attacker gains the ability to modify a prototype, they can **globally affect application behavior**.

```
let personPrototype = {
  introduce: function() {
    return `Hi, I'm ${this.name}.`;
  }
};

function Person(name) {
  let person = Object.create(personPrototype);
  person.name = name;
  return person;
}

Prototype chain:
ben → personPrototype → Object.prototype → null

# Normal Object Creation
let ben = Person('Ben');
console.log(ben.introduce());

result:
Hi, I'm Ben.

# The Attack (Prototype Pollution)
ben.__proto__.introduce = function() {
  console.log("You've been hacked, I'm h4ck3r");
};

What this line REALLY does:
ben.__proto__ === personPrototype // true

So this means:
personPrototype.introduce = maliciousFunction;

You are not modifying ben, you are modifying the shared prototype.

console.log(ben.introduce());
You've been hacked, I'm h4ck3r

And now:

let alice = Person("Alice");
alice.introduce(); 

Output:
You've been hacked, I'm h4ck3r
```

## Why This Is Dangerous

Prototype pollution alone:

- Modifies application behavior
- Breaks trust assumptions

Prototype pollution chained with:

- **XSS** → arbitrary JS execution
- **CSRF** → forced actions
- **Auth logic** → privilege escalation

---
### 1.Standard Approach

In JavaScript, every object inherits from **`Object.prototype`**.

Two important properties attackers target:

- **`__proto__`** → References the object's prototype
- **`constructor`** → Points to the function that created the object
    - `constructor.prototype` can be used to reach the global prototype

Attackers exploit these properties to modify the prototype chain, which affects **all objects** in the application.

This attack is called **Prototype Pollution**.

### 2.Golden Rule of Prototype Pollution

Prototype pollution happens when:

```
Object[x][y] = value

If an attacker controls x and sets:

x = "__proto__"

then:

Object.__proto__[y] = value
Now every object in the application gets property y = value.
```

```
# Advanced Case

If attacker controls:

Object[x][y][z] = value

And sets:

x = "constructor"
y = "prototype"

then:

Object.constructor.prototype[z] = value

This also pollutes the global prototype.
```

### 3.Dangerous Functions to Look For

Focus on functions that:

- Set properties using user-controlled paths
- Deep merge objects
- Copy or assign properties dynamically

Common vulnerable pattern:

```
object[a][b][c] = value
```

Or when using:

```
_.set(object, userInputPath, userInputValue)
```

From **Lodash**

### 4.Example Scenario

Initial Object

```
let friends = [
  {
    id: 1,
    name: "testuser",
    age: 25,
    country: "UK",
    reviews: [],
    albums: [{}],
    password: "xxx"
  }
];
```

Application uses:

```
_.set(friend, input.path, input.value);
```

Normal Input

```
{
  "path": "reviews[0].content",
  "value": "<script>alert('anycontent')</script>"
}

Result:

- Review is added    
- No sanitization → XSS risk
  
Malicious Input:

{
  "path": "isAdmin",
  "value": true
}

Result:

{
  ...
  isAdmin: true
}

Now the attacker escalates privileges.
```


---
### MItigation Measures

﻿Mitigating the risks associated with prototype pollution is crucial for both pentesters and secure code developers, as the vulnerability enables attackers to manipulate an object's prototype, potentially leading to unexpected behaviour and security issues. Here are some mitigation measures for prototype pollution:  

Pentesters

- **Input Fuzzing and Manipulation**: Interact with user inputs extensively, especially those used to interact with prototype-based structures, and fuzz them with a variety of payloads. Look for scenarios where untrusted data can lead to prototype pollution.
- **Context Analysis and Payload Injection**: Analyse the application's codebase to understand how user inputs are used within prototype-based structures. Inject payloads into these contexts to test for prototype pollution vulnerabilities.
- **CSP Bypass and Payload Injection**: Evaluate the effectiveness of security headers such as CSP in mitigating prototype pollution. Attempt to bypass CSP restrictions and inject payloads to manipulate prototypes.
- **Dependency Analysis and Exploitation**: Conduct a thorough analysis of third-party libraries and dependencies used by the application. Identify outdated or vulnerable libraries that may introduce prototype pollution vulnerabilities. Exploit these vulnerabilities to manipulate prototypes and gain unauthorised access or perform other malicious actions.
- **Static Code Analysis**: Use static code analysis tools to identify potential prototype pollution vulnerabilities during the development phase. These tools can provide insights into insecure coding patterns and potential security risks.

Secure Code Developers

- **Avoid Using __proto__**: Refrain from using the `__proto__` property as it is mosltly susceptible to prototype pollution. Instead, use `Object.getPrototypeOf()` to access the prototype of an object in a safer manner.
- **Immutable Objects**: Design objects to be immutable when possible. This prevents unintended modifications to the prototype, reducing the impact of prototype pollution vulnerabilities.
- **Encapsulation**: Encapsulate objects and their functionalities, exposing only necessary interfaces. This can help prevent unauthorised access to object prototypes.
- **Use Safe Defaults**: When creating objects, establish safe default values and avoid relying on user inputs to set prototype properties. Initialise objects securely to minimise the risk of pollution.
- **Input Sanitisation**: Sanitise and validate user inputs thoroughly. Be cautious when using user-controlled data to modify object prototypes. Apply strict input validation practices to mitigate injection risks.
- **Dependency Management**: Regularly update and monitor dependencies. Choose well-maintained libraries and frameworks, and stay informed about any security updates or patches related to prototype pollution.
- Security Headers: Implement security headers such as Content Security Policy (CSP) to control the sources from which resources can be loaded. This can help mitigate the risk of loading malicious scripts that manipulate prototypes.

By combining rigorous testing practices, secure coding principles, and ongoing security awareness, both pentesters and secure code developers can contribute to the effective mitigation of Prototype Pollution vulnerabilities in applications. Regularly updating knowledge on emerging threats and vulnerabilities is essential to avoid potential risks.