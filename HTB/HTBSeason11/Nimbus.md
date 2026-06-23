network enumeration:

```
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 9.6p1 Ubuntu 3ubuntu13.16 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   256 eb:ab:8f:be:99:02:0b:3e:c4:1c:83:b2:66:2f:17:13 (ECDSA)
|_  256 c1:69:ab:84:f3:88:8b:b3:8a:ae:e2:28:35:54:35:0b (ED25519)
80/tcp open  http    nginx 1.24.0 (Ubuntu)
|_http-title: Did not follow redirect to http://nimbus.htb/
|_http-server-header: nginx/1.24.0 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```


```
http://nimbus.htb/api/v1/health

{"services":{"queue":{"endpoint":"http://aws.nimbus.htb","status":"ok"},"scheduler":{"endpoint":"http://aws.nimbus.htb","status":"ok"},"storage":{"endpoint":"http://aws.nimbus.htb","status":"ok"}},"status":"healthy","version":"1.4.2"}


also also know the nimbus v1.4.2
Hint: Your SSH key needs to be approved by a DevOps lead. Ping marcus on Slack.
```

```
first test the endpoint /jobs/preview where the url is passed i test the ssrf

i test 169.254.169.254 which is blacklist so i 
```



