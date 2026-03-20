
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

- “Am I trying to become admin?” → **Vertical**
- “Am I trying to access someone else’s data?” → **Horizontal**

