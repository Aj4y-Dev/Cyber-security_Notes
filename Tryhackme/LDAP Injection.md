### LDAP (Lightweight Directory Access Protocol)

- **Definition:** LDAP is a protocol used to access and maintain distributed directory information services over an IP network.
- **Purpose:** It allows organizations to centrally manage users, groups, and other directory information.
- **Common Uses:**
    - Authentication (verifying user identities)
    - Authorization (managing access to resources)
    - Integration with web applications and internal systems

---

### LDAP Directory Entries & Structure

- **Directory Entries:**
    - LDAP entries are **objects** that follow a specific **schema**, defining allowed attributes and rules.
    - This **object-oriented approach** ensures consistency and governs representation of users, groups, and other resources.
- **Common LDAP Services:**
    - **Microsoft Active Directory:** Manages Windows domain networks using LDAP to handle domain resources.
    - **OpenLDAP:** Open-source LDAP implementation for managing user information and authentication across platforms.
- **LDIF (LDAP Data Interchange Format):**
    - Standard **plain text format** for representing LDAP entries and updates.
    - Supports operations like **add, modify, and delete entries**.
    - Used for importing/exporting directory contents.
- **LDAP Hierarchical Structure:**
    - Similar to a **tree structure** in a file system.
    - **Top-Level Domain (TLD):** Example: `dc=ldap,dc=thm`
    - **Subdomains / Organizational Units (OUs):** Example: `ou=people`, `ou=groups`
- **Key Terms:**
    - **Distinguished Names (DNs):** Unique identifiers for entries, specifying the path from the top of the LDAP tree. Example: `cn=John Doe,ou=people,dc=example,dc=com`
    - **Relative Distinguished Names (RDNs):** Individual levels within the hierarchy. Example: `cn=John Doe` (`cn` = Common Name)
    - **Attributes:** Properties of entries. Example: `mail=john@example.com`

---
### LDAP Search Queries

- **Purpose:**  
    LDAP search queries are used to **locate and retrieve information** stored in an LDAP directory.
- **Why important:**  
    Knowing how to build LDAP queries is essential for **authentication systems**, **directory enumeration**, and **LDAP injection** scenarios in CTFs.

---
### Components of an LDAP Search Query

An LDAP search query consists of the following parts:

1. **Base DN (Distinguished Name)**
    - Starting point of the search in the directory tree
    - Example: `dc=ldap,dc=thm`
2. **Scope**  
    Defines how deep the search goes from the Base DN:
    - `base` → search only the Base DN
    - `one` → search only immediate children
    - `sub` → search Base DN and **all descendants** (most common)
3. **Filter**
    - Conditions entries must match to be returned
    - Written using LDAP filter syntax
4. **Attributes**
    - Specifies which attributes to return (e.g., `cn`, `mail`)
    - If omitted, all attributes are returned

```
(base DN) (scope) (filter) (attributes)
```

---
## LDAP Filters & Syntax

- Filters define **search conditions**
- Defined in **RFC 4515**
- Written inside parentheses
### Common Operators

|Operator|Meaning|
|---|---|
|`=`|Equality|
|`=*`|Presence|
|`>=`|Greater than|
|`<=`|Less than|
|`*`|Wildcard (any number of characters)|

---
## Filter Examples

Simple Filter:

```
(cn=John Doe)
```

Matches entries where `cn` is exactly **John Doe**.

Wildcard Filter:

```
(cn=J*)
```

Matches entries where `cn` **starts with J**.

Logical Operators

LDAP supports:

- **AND** → `&`
    
- **OR** → `|`
    
- **NOT** → `!`
    

### Complex Filter Example