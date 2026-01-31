
**OAuth** lets users log in using third-party providers (Google, GitHub, etc.).  
Vulnerabilities happen due to **bad implementation**, not OAuth itself.

### Common OAuth Vulnerabilities:

1. **Open Redirect (redirect_uri manipulation)**  
    → Attacker steals auth code or token by changing `redirect_uri`.
    
2. **Missing / Weak `state` Parameter**  
    → OAuth CSRF → victim gets logged into attacker’s account.
    
3. **Authorization Code Reuse**  
    → Same auth code can be used multiple times → session hijack.
    
4. **Token Leakage in URL**  
    → Access tokens exposed via URL, referer header, logs.
    
5. **Improper Client Validation**  
    → OAuth provider doesn’t properly verify client ID/secret.
    
6. **Email Trust Issues**  
    → App trusts email without checking `email_verified` → account takeover.
    
7. **Scope Manipulation**  
    → Attacker requests higher privileges than intended.
    
8. **Implicit Flow Issues**  
    → Access token in URL → easy to steal (XSS, browser history).
    
## 3. Authorization Code Reuse

### Bug

Auth code can be used **multiple times**

### Attack

- Capture auth code
- Replay it
- Get tokens again

### Impact

- Session hijacking
- Token duplication

### Hint

Look for:


## 5. Improper Client Validation

### Bug

OAuth provider **does not verify client properly**

Example:

- Client ID is public
- No client secret validation


6. Email Trust Issues (OAuth Account Takeover)

### Classic CTF bug 🔥

App trusts:

`email = user@gmail.com`

but ignores:

- `email_verified = false`

### Attack

- OAuth provider allows unverified emailsP
- Create account with victim’s email
- Login → account takeover

## . Scope Manipulation

### Bug

Client asks for **more scopes than needed**

### Attack

Modify:

`scope=profile`

→

`scope=profile email admin`

### Impact

- Access private data
    
- Privilege escalation