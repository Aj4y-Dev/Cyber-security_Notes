- **ORM (Object-Relational Mapping):**  
    A programming technique that allows developers to interact with databases using **objects instead of raw SQL queries**.
- **Why ORMs are used:**
    - Simplify database operations
    - Improve code readability
    - Reduce the risk of **SQL Injection**

---
## Why ORM Injection Still Exists

- ORMs are designed to **prevent classic SQL injection**, but:
    - Misuse of ORM methods
    - Unsafe query construction
    - Dynamic filters or raw queries  
        can still lead to injection vulnerabilities.
- **ORM Injection** occurs when an attacker manipulates ORM-based queries to:
    - Execute **arbitrary database queries**
    - Bypass authentication or authorization
    - Extract or modify sensitive data

---
### ORM (Object-Relational Mapping)

### Definition

- **ORM** is a programming technique that **converts data between incompatible systems** using object-oriented programming (OOP).
- It allows developers to interact with a database using the **programming language’s native syntax**, reducing the need for raw SQL.
- Benefits:
    - Simplifies complex data interactions
    - Promotes **code reusability**
    - Makes database access more intuitive