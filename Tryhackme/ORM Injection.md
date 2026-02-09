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

---
## ORM & Database Interconnectivity

### Purpose of ORM

ORM acts as a **bridge** between the **object-oriented programming model** and the **relational database model**.

**Key Benefits:**

1. **Reduce boilerplate code:** Automatically generates SQL queries from object operations
2. **Increase productivity:** Focus on business logic, not database queries
3. **Ensure consistency:** ORM frameworks handle database operations uniformly
4. **Enhance maintainability:** Changes in the database schema reflect in the object model without extensive code modifications

---
### Common ORM Frameworks

|Framework|Language|Notes|
|---|---|---|
|**Doctrine**|PHP|Popular in Symfony, provides query builder, schema management, and object-oriented query language|
|**Hibernate**|Java|Maps Java classes to database tables, uses HQL, supports caching & lazy loading|
|**SQLAlchemy**|Python|Flexible ORM + SQL toolkit, allows raw SQL when needed|
|**Entity Framework**|C#/.NET|Works with domain-specific objects, reduces manual data-access code|
|**Active Record**|Ruby on Rails|Follows Active Record pattern; each table = class, each row = instance, rich query & manipulation methods|
