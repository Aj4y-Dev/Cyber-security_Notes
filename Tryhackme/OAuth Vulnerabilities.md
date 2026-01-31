
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