# Backend Security Attacks & Mitigations

This document is written for **backend developers** participating in hackathons. It focuses on **real-world web attacks**, **how attackers exploit them**, and **how to fix them properly on the backend**. Examples assume Node.js/Express, but concepts apply to any backend (Django, Spring, Laravel, Go, etc.).

---
## 1. SQL Injection (SQLi)

### What is it?

SQL Injection happens when user input is directly concatenated into SQL queries, allowing attackers to manipulate the database query.

### Vulnerable Example

```js
const query = `SELECT * FROM users WHERE email = '${email}' AND password = '${password}'`;
```

### Attack Examples

- `' OR 1=1 --`
- `' UNION SELECT username, password FROM users --`
### Impact

- Database dump
- Authentication bypass
- Data manipulation or deletion
### Backend Fix

✅ **Always use prepared statements / parameterized queries**

```js
const query = "SELECT * FROM users WHERE email = ? AND password = ?";
```

### Extra Protection

- Use ORM (Sequelize, Prisma, TypeORM)
- Least-privilege DB user
- Input validation

---

## 2. NoSQL Injection

### What is it?

Occurs in MongoDB when user input is used directly in query objects.

### Vulnerable Example

```js
User.findOne({ email: req.body.email, password: req.body.password });
```

### Attack Payload

```json
{ "email": { "$ne": null }, "password": { "$ne": null } }
```

### Fix

```js
if (typeof email !== 'string') return res.status(400);
```

- Use schema validation (Joi, Zod)
- Disable query selectors in user input    

---
## 3. Cross-Site Scripting (XSS)

### What is it?

XSS occurs when user input is rendered as executable JavaScript in the browser.

### Types

- Stored XSS
- Reflected XSS
- DOM XSS    
### Backend Fix

- Escape output (HTML encoding)
- Never trust frontend sanitization
- Use Content Security Policy (CSP)

```http
Content-Security-Policy: default-src 'self'
```

---
## 4. Server-Side Template Injection (SSTI)

### What is it?

Occurs when user input is rendered inside server templates (EJS, Jinja2, Twig).

### Vulnerable Example

```js
res.render('profile', { name: req.query.name });
```

### Attack Example (EJS)

```ejs
<%= this.constructor.constructor('return process')() %>
```

### Fix

- Never render raw user input
- Disable dangerous template features
- Use strict template modes    

---
## 5. Command Injection

### What is it?

Executing system commands with user input.

### Vulnerable Example

```js
exec(`ping ${req.query.host}`);
```

### Attack

```bash
127.0.0.1; cat /etc/passwd
```

### Fix

- Avoid shell execution
- Use allowlists
- Use execFile instead of exec    

---
## 6. File Upload Vulnerabilities

### Attacks

- Web shell upload
- Path traversal
- MIME spoofing
### Fix

- Check MIME type + extension
- Rename files
- Store outside web root
- Disable execution permissions

---
## 7. Path Traversal / LFI

### Vulnerable Example

```js
fs.readFile(`/files/${req.query.file}`)
```
### Attack

```bash
../../../../etc/passwd
```
### Fix

```js
path.normalize()
```

- Allowlist filenames
- Never trust paths from users    

---
## 8. Authentication Vulnerabilities

### Common Issues

- Weak password hashing
- JWT misconfiguration
- Session fixation
### Fix

- Use bcrypt / argon2
- Short JWT expiry
- HttpOnly + Secure cookie    

---
## 9. Authorization (IDOR)

### What is it?

Accessing resources by changing IDs.
### Vulnerable Example

```http
GET /api/user/123
```
### Fix

- Always check ownership
- Never trust user-supplied IDs    

---
## 10. CSRF

### Fix

- CSRF tokens
- SameSite cookies
- Use POST for state-changing actions    

---
## 11. CORS Misconfiguration

### Dangerous

```http
Access-Control-Allow-Origin: *
```

### Fix
- Restrict origins
- Never allow credentials with '*'    

---
## 12. Rate Limiting & Bruteforce

### Fix

- Rate limit login endpoints
- CAPTCHA after failures
- Account lockout

---
## 13. Logging & Monitoring

### Why?

- Detect attacks early
- Incident response
### Best Practices

- Log auth failures
- Monitor 500 errors
- Never log secrets

---
## 14. Secure Backend Checklist

✅ Input validation  
✅ Output encoding  
✅ Authentication & Authorization checks  
✅ Secure headers  
✅ Proper error handling  
✅ Dependency updates

---
## Final Hackathon Advice

- **Security = backend responsibility**
- Assume frontend is bypassed
- If unsure, restrict
- Simple secure design > complex insecure logic

If you want, I can:

- Convert this into **PDF / PPT**
- Add **real CTF-style payloads**
- Tailor it to **Node.js / Django / Laravel**
- Create a **hackathon security checklist**