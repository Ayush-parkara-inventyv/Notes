## Overview
Nessus is a comprehensive vulnerability scanner that helps identify potential security vulnerabilities in systems, networks, and applications. It is used by security professionals to perform network and system scans, detecting known vulnerabilities such as missing patches, weak configurations, and outdated software. Nessus supports both authenticated and unauthenticated scans, providing deep insights into system weaknesses.

## Key Terminology
- **Vulnerability Scanning**: The process of detecting security flaws or weaknesses in a system, network, or application.
- **Plugin**: A module in Nessus that performs a specific type of scan, such as checking for missing patches or weak passwords.
- **CVEs (Common Vulnerabilities and Exposures)**: Identifiers for publicly known cybersecurity vulnerabilities.
- **Authenticated Scans**: Scans performed with credentials, allowing Nessus to check for deeper vulnerabilities in the system that require higher levels of access.
- **Unauthenticated Scans**: Scans performed without credentials, simulating an attacker's perspective.

---

## Basic Nessus Commands
### Running a Basic Scan
```bash
nessus -T html -o /path/to/output.html -p /path/to/target.txt
````

Scans the targets listed in `target.txt` and outputs the results in HTML format.

### Starting Nessus with a Specific Plugin

```bash
nessus -p 9123 --plugin=plugin_id
```

Starts a scan on port 9123 and runs a specific plugin to test for a vulnerability.

### Running an Authenticated Scan

```bash
nessus -p 22 --username=user --password=password --target 192.168.1.1
```

Performs an authenticated scan against `192.168.1.1` with the provided username and password.

---

## Advanced Nessus Techniques

### Scanning a Range of IPs

```bash
nessus -T html -o /path/to/output.html -p 192.168.1.1-192.168.1.255
```

Scans a range of IP addresses (from `192.168.1.1` to `192.168.1.255`) and generates an HTML report.

### Scanning Specific Ports

```bash
nessus -T html -p 80,443,8080 --target example.com
```

Scans specific ports (80, 443, and 8080) on `example.com` for vulnerabilities.

### Customizing Scan Settings

```bash
nessus --plugin-set=advanced --scan-name="Custom Scan"
```

Customizes the scan settings using an advanced plugin set, naming the scan as "Custom Scan."

---

## Practical Use Cases

- **Network Security Audits**: Identifying security weaknesses and vulnerabilities in an organization's network infrastructure.
- **Penetration Testing**: Scanning systems and networks to uncover exploitable vulnerabilities.
- **Compliance Testing**: Ensuring that systems are compliant with security standards like PCI DSS, HIPAA, or GDPR.
- **Patch Management**: Identifying missing patches and outdated software that could expose systems to attacks.

---

## Mitigation Strategies

- **Regular Scanning**: Perform routine vulnerability scans to identify and address security flaws in a timely manner.
- **Patch Management**: Regularly update and patch systems to reduce the risk of known vulnerabilities being exploited.
- **Least Privilege**: Use least privilege access when configuring systems, ensuring only authorized users can access critical areas.
- **Security Awareness**: Educate staff and developers about secure coding practices and proper patch management to prevent vulnerabilities from being introduced in the first place.

---

## References

- [Nessus Official Website](https://www.tenable.com/products/nessus)
