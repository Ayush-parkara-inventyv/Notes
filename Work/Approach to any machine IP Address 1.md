# Penetration Testing Walkthrough

## Step 1: **Verify Legitimacy and Initial Reconnaissance**
Before starting, ensure you have **explicit permission** to pentest the target. For CTFs (Capture The Flag) or platforms like **Hack The Box**, you're good to go. For real-world targets, verify the scope of your engagement.

### Tools:
- **[[Whois Lookup]]**: Gather domain and IP information.
  ```bash
  whois <TARGET_IP>
```

- **[[Shodan]]**: Identify open ports, services, and potential vulnerabilities.
    
    ```bash
    shodan <TARGET_IP>
    ```
    
- **[[Spiderfoot]]**: Perform OSINT (Open Source Intelligence) gathering.
    
    ```bash
    spiderfoot -d <DOMAIN> -l 3
    ```
    

---

## Step 2: **Network Reconnaissance**

Passive and active techniques to gather information about the target network.

### Tools:

- **[[Nmap]]**: Perform an initial port scan to identify open ports and services.
    
    ```bash
    nmap -sV -A <TARGET_IP>
    ```
    
- **[[Gobuster]]**: Enumerate directories and files on web servers.
    
    ```bash
    gobuster dir -u http://<TARGET> -w /path/to/wordlist
    ```
    
- **[[Google Dorking]]**: Use Google search operators to find sensitive information.
    
    ```plaintext
    site:<DOMAIN> filetype:pdf
    ```
    

---

## Step 3: **Identify Vulnerabilities**

Scan the target for known vulnerabilities in services and configurations.

### Tools:

- **[[Nessus]]**: Perform a vulnerability scan.
    
    ```bash
    nessus -q <TARGET_IP>
    ```
    
- **[[Testssl.sh]]**: Test SSL/TLS configuration and vulnerabilities.
    
    ```bash
    testssl.sh --html <TARGET_IP>:443 > ssltest.html
    ```
    
- **[[ZAP (Zed Attack Proxy)]]**: Identify web application vulnerabilities.
    
    ```bash
    zap-baseline.py -t http://<TARGET>
    ```
    

---

## Step 4: **Web Application Enumeration**

Enumerate endpoints, parameters, and potential attack vectors.

### Tools:

- **[[Gobuster]]**: Enumerate directories and files on web servers.
    
    ```bash
    gobuster dir -u http://<TARGET> -w /path/to/wordlist
    ```
    
- **[[Nikto]]**: Identify vulnerabilities in web servers and applications.
    
    ```bash
    nikto -h http://<TARGET>
    ```
    
- **[[Dirb]]**: Enumerate directories and files (alternative to Gobuster).
    
    ```bash
    dirb http://<TARGET> /path/to/wordlist.txt
    ```
    

---

## Step 5: **Exploitation**

Attempt to exploit identified vulnerabilities.

### Tools:

- **[[Metasploit]]**: Use exploits for known vulnerabilities.
    
    ```bash
    msfconsole
    use exploit/<EXPLOIT_NAME>
    set RHOST <TARGET_IP>
    run
    ```
    
- **[[Hydra]]**: Perform brute-force attacks on login pages or services.
    
    ```bash
    hydra -l <USERNAME> -P <PASSWORD_LIST> <TARGET_IP> http-post-form "/login.php:username=^USER^&password=^PASS^:invalid"
    ```
    
- **[[SQLMAP]]**: Exploit SQL injection vulnerabilities.
    
    ```bash
    sqlmap -u "http://<TARGET>?id=1" --dbs
    ```
    

---

## Step 6: **Wireless and Bluetooth Reconnaissance**

If the target includes wireless or Bluetooth devices, perform additional reconnaissance.

### Tools:

- **[[Aircrack-ng Suite]]**: For wireless network attacks.
    
    ```bash
    airmon-ng start wlan0
    airodump-ng mon0
    ```
    
- **[[sdptool]]**: Discover Bluetooth services on nearby devices.
    
    ```bash
    sdptool browse <BLUETOOTH_ADDRESS>
    ```
    

---

## Step 7: **Credential Harvesting and Social Engineering**

Attempt to gather credentials or use social engineering tactics.

### Tools:

- **[[CUpp (Common User Passwords Profiler)]]**: Generate password profiles for users.
    
    ```bash
    cupp -i
    ```
    
- **[[Zphisher]]**: Create phishing pages to harvest credentials.
    
    ```bash
    git clone https://github.com/htr-tech/zphisher.git
    cd zphisher
    bash zphisher.sh
    ```
    

---

## Step 8: **Post-Exploitation and Privilege Escalation**

After gaining access, escalate privileges and maintain access.

### Tools:

- **[[Metasploit]]**: Use post-exploitation modules to escalate privileges.
    
    ```bash
    use exploit/linux/local/<EXPLOIT_NAME>
    ```
    
- **[[LinPEAS]]**: Identify potential privilege escalation vectors on Linux systems.
    
    ```bash
    ./linpeas.sh
    ```
    

---

## Step 9: **Network Monitoring and Sniffing**

Monitor network traffic to identify additional vulnerabilities or credentials.

### Tools:

- **[[Wireshark]]**: Analyze network traffic captures.
    
    ```bash
    sudo tcpdump -i eth0 -w capture.pcap
    ```
    
- **[[Ettercap]]**: Perform man-in-the-middle (MITM) attacks and sniff traffic.
    
    ```bash
    ettercap -G
    ```
    

---

## Step 10: **Log Analysis and Data Leak Detection**

Analyze logs for sensitive information or data leaks.

### Tools:

- **[[gitleaks]]**: Detect secrets in git repositories.
    
    ```bash
    gitleaks -r <REPO_URL>
    ```
    

---

## Step 11: **Reporting**

Document all findings, vulnerabilities, and steps taken during the pentest. Use a tool like **Obsidian** for easy markdown-based documentation.

_Example Report Structure_:

- **Executive Summary**: High-level overview of the engagement.
- **Vulnerability Findings**: Details of the vulnerabilities found, their impact, and exploitability.
- **Recommendations**: How the vulnerabilities can be mitigated.
- **Proof of Exploitation**: Include screenshots and logs.

---

## Final Thoughts

This guide incorporates a comprehensive approach to pentesting, starting from reconnaissance to exploitation and finally, post-exploitation. It integrates a wide range of tools, covering network scanning, web application testing, wireless attacks, credential harvesting, and more. With each tool integrated into a structured approach, you can tackle a penetration test or CTF challenge effectively.

By refining and adjusting these methods, you’ll continuously improve your skills and stay updated with the latest pentesting techniques.
