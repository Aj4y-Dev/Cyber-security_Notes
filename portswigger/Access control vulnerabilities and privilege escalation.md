
Broken access controls are common and often present a critical security vulnerability. Design and management of access controls is a complex and dynamic problem that applies business, organizational, and legal constraints to a technical implementation. Access control design decisions have to be made by humans so the potential for errors is high.

ACCESS CONTROL TYPES:

1. Vertical Access Control
- Restricts access based on user roles
- Example: Admin vs normal user
- Attack: Accessing higher privilege functionality
- Example bug: User accessing /admin panel

2. Horizontal Access Control
- Restricts access between users
- Same role, different data
- Attack: Accessing another user's resources
- Example bug: Changing user ID (IDOR)

3. Context-Dependent Access Control
- Restricts actions based on application state
- Ensures correct sequence of operations

Key Difference:
- Vertical → privilege escalation (role-based)
- Horizontal → data access (user-based)
- Context -> Can I:

- Skip steps?
- Repeat actions?
- Do things out of order?

- “Am I trying to become admin?” → **Vertical**
- “Am I trying to access someone else’s data?” → **Horizontal**
- "It is about "when/how action is performed"" -> context

## Vertical privilege escalation LABS

#### Lab: Unprotected admin functionality

in robots.txt :

```
User-agent: *
Disallow: /administrator-panel

Meaning:
- All bots should avoid this path
- But humans can still access it
```

then i can access to /administrator-panel and delete carlos.

#### Lab: Unprotected admin functionality with unpredictable URL

i found script in the html code :

```
var isAdmin = false; 
if (isAdmin) { 
var topLinksTag = document.getElementsByClassName("top-links")[0]; 
var adminPanelTag = document.createElement('a'); adminPanelTag.setAttribute('href', '/admin-d43y8o'); 
adminPanelTag.innerText = 'Admin panel'; 
topLinksTag.append(adminPanelTag); 
var pTag = document.createElement('p'); 
pTag.innerText = '|'; 
topLinksTag.appendChild(pTag); 
}
```







