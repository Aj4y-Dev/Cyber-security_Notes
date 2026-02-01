
**OAuth** lets users log in using third-party providers (Google, GitHub, etc.).  
Vulnerabilities happen due to **bad implementation**, not OAuth itself.

### Common OAuth Vulnerabilities:

1. **Open Redirect (redirect_uri manipulation)**  
    → Attacker steals auth code or token by changing `redirect_uri`.
    
2. **Missing / Weak `state` Parameter**  
    → OAuth CSRF → victim gets logged into attacker’s account.
    
