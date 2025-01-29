## Overview
Nikto is an open-source web server scanner used to detect vulnerabilities and misconfigurations in web servers. It performs comprehensive tests on a target server, checking for common security issues, outdated software, and possible security holes. Nikto helps security professionals and penetration testers quickly identify and mitigate web server-related vulnerabilities.

## Key Terminology
- **Web Server**: A server that hosts websites or web applications and responds to requests made by clients (browsers).
- **Vulnerability Scanning**: The process of identifying security flaws in a system, network, or application.
- **HTTP Methods**: The types of operations that can be performed on a web server, such as GET, POST, PUT, DELETE, etc.
- **SSL/TLS**: Protocols used for securing communication between web servers and clients.

---

## Basic Nikto Commands
### Scanning a Web Server
```bash
nikto -h http://example.com
````

Scans the web server at `http://example.com` for common vulnerabilities and configuration issues.

### Scanning with Specific Port

```bash
nikto -h http://example.com -p 8080
```

Scans the web server on port `8080` for vulnerabilities.

### Scanning a Range of IPs

```bash
nikto -h 192.168.1.1-192.168.1.255
```

Performs a vulnerability scan across a range of IP addresses (from `192.168.1.1` to `192.168.1.255`).

---

## Advanced Nikto Techniques

### Saving Output to a File

```bash
nikto -h http://example.com -o /path/to/output.txt
```

Saves the scan results to the specified output file (`output.txt`).

### Using a Proxy

```bash
nikto -h http://example.com -proxy http://localhost:8080
```

Routes the scan through a proxy server (useful for evading detection or for testing behind a proxy).

### Enabling SSL/TLS Scanning

```bash
nikto -h https://example.com
```

Scans a web server using HTTPS (SSL/TLS) instead of HTTP to check for SSL-related vulnerabilities.

### Scanning for Specific Vulnerabilities

```bash
nikto -h http://example.com -Tuning 3
```

Uses the `-Tuning` flag to adjust the scan’s focus on specific types of vulnerabilities. (`-Tuning 3` targets tests for web server and file vulnerabilities).

---

## Practical Use Cases

- **Web Application Security Audits**: Quickly identifying security vulnerabilities in web applications and servers.
- **Penetration Testing**: Discovering weak points in a web server during a penetration test to exploit or mitigate vulnerabilities.
- **Compliance Audits**: Ensuring that web servers are compliant with standards such as PCI DSS or HIPAA by scanning for misconfigurations.
- **Bug Bounty Programs**: Scanning websites and applications for known vulnerabilities as part of a bug bounty program.

---

## Mitigation Strategies

- **Regular Vulnerability Scanning**: Regularly scan web servers for vulnerabilities using tools like Nikto to ensure they remain secure.
- **Patch Management**: Keep web server software and services up to date to avoid known vulnerabilities being exploited.
- **Disabling Unnecessary HTTP Methods**: Disable unsafe HTTP methods (like PUT and DELETE) that may be used to manipulate server resources.
- **Hardening Web Server Configurations**: Ensure that web server configurations are secure by following best practices for web server hardening.
- **SSL/TLS Configuration**: Ensure that SSL/TLS is properly configured, including using strong ciphers and certificates.

---

## References

- [Nikto GitHub Repository](https://github.com/sullo/nikto)
- [Nikto Documentation](https://cirt.net/Nikto2)