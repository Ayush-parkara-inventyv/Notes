## Overview
Gobuster is a fast and flexible tool used for directory and DNS brute-forcing. It is primarily used for discovering hidden directories, subdomains, or virtual hosts within a web server. Gobuster works by making multiple requests to a target and analyzing the server's responses to identify potential points of entry or misconfigurations.

## Key Terminology
- **Brute-forcing**: The process of trying a large number of possible inputs (like directory names or subdomains) to discover valid results.
- **Directories**: Folders within a web server that may contain files or other resources.
- **Subdomains**: Alternative domain names that point to the same web server, often used to segment different areas of a website or service.
- **Virtual Hosts**: Configuration on a web server that allows multiple domains to point to the same IP address, but respond with different content.

---

## Basic Gobuster Commands
### Directory Bruteforce Scan
```bash
gobuster dir -u http://example.com -w /path/to/wordlist.txt
````

Performs a brute force scan to discover hidden directories or files on `example.com`, using the specified wordlist.

### DNS Subdomain Bruteforce Scan

```bash
gobuster dns -d example.com -w /path/to/wordlist.txt
```

Brute-forces subdomains of `example.com`, attempting to resolve them by querying the DNS server.

### Virtual Host Scan

```bash
gobuster vhost -u http://example.com -w /path/to/wordlist.txt
```

Attempts to discover virtual hosts by guessing potential hostnames.

---

## Advanced Gobuster Techniques

### Using Custom Headers

```bash
gobuster dir -u http://example.com -w /path/to/wordlist.txt -H "User-Agent: custom-agent"
```

Adds a custom header (like a User-Agent) to all requests during the scan.

### Tuning Scan Speed with Concurrency

```bash
gobuster dir -u http://example.com -w /path/to/wordlist.txt -t 50
```

Adjusts the number of concurrent threads used in the scan to speed up the process (`-t` specifies threads).

### Filter Out Specific Status Codes

```bash
gobuster dir -u http://example.com -w /path/to/wordlist.txt -s "200,301"
```

Filters the scan results to only show directories that return specific HTTP status codes (e.g., 200 for success or 301 for redirection).

---

## Practical Use Cases

- **Directory Enumeration**: Discovering hidden files or directories that may expose sensitive information or services.
- **Subdomain Discovery**: Identifying subdomains associated with a domain to expand the attack surface during penetration tests.
- **Finding Misconfigured Virtual Hosts**: Detecting virtual hosts that may reveal multiple applications or services on the same server.

---

## Mitigation Strategies

- **Obfuscate Directories**: Ensure that important directories are not accessible or discoverable by using unique or non-standard names.
- **Disable Unnecessary Services**: Avoid leaving unused services or subdomains exposed.
- **Rate Limiting**: Use rate limiting and security mechanisms like CAPTCHA to prevent brute-force attacks from succeeding.
- **Web Application Firewall (WAF)**: Deploy a WAF to detect and block malicious traffic attempting directory or subdomain brute-forcing.

---

## References

- [Gobuster GitHub Repository](https://github.com/OJ/gobuster)