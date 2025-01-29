## Overview

Nmap (Network Mapper) is a powerful open-source tool used for network discovery, security auditing, and vulnerability assessment. It allows users to scan networks to identify active hosts, open ports, running services, and operating systems.

## Key Terminology

- **Port Scanning**: Identifying open ports on a target system.
- **Service Detection**: Determining what services and versions are running on a host.
- **OS Fingerprinting**: Identifying the operating system of a target device.
- **Network Discovery**: Finding devices and hosts on a network.

---

## Basic Nmap Commands

### Scanning a Single Host

```bash
nmap example.com
```

Performs a basic scan on `example.com` to detect open ports.

### Scanning Multiple Hosts

```bash
nmap 192.168.1.1 192.168.1.2 192.168.1.3
```

Scans multiple specified IP addresses.

### Scanning an Entire Subnet

```bash
nmap 192.168.1.0/24
```

Scans all devices within the `192.168.1.0/24` subnet.

### Scanning a Specific Port

```bash
nmap -p 80 example.com
```

Checks if port 80 (HTTP) is open on `example.com`.

### Scanning Multiple Ports

```bash
nmap -p 22,80,443 example.com
```

Scans ports 22 (SSH), 80 (HTTP), and 443 (HTTPS) on `example.com`.

---

## Advanced Nmap Scanning Techniques

### Aggressive Scan (Service & OS Detection)

```bash
nmap -A example.com
```

Performs a comprehensive scan, including OS detection, version detection, script scanning, and traceroute.

### Detecting Operating System

```bash
nmap -O example.com
```

Attempts to determine the operating system of `example.com`.

### Performing a Stealth Scan

```bash
nmap -sS example.com
```

Conducts a SYN scan to avoid detection by firewalls and intrusion detection systems (IDS).

### Scanning with Version Detection

```bash
nmap -sV example.com
```

Identifies services running on open ports and their versions.

### Scanning for Vulnerabilities with NSE Scripts

```bash
nmap --script=vuln example.com
```

Uses Nmap Scripting Engine (NSE) to check for known vulnerabilities on `example.com`.

---

## Practical Use Cases

- **Network Security Audits**: Identifying open ports and misconfigured services.
- **Penetration Testing**: Assessing security vulnerabilities in networks.
- **IT Asset Management**: Mapping devices and services in a corporate network.
- **Firewall Testing**: Checking if firewall rules are correctly blocking unwanted traffic.

---

## Mitigation Strategies

- **Limit Exposure of Open Ports**: Only expose necessary services to the public network.
- **Enable Firewalls**: Block unauthorized scanning attempts.
- **Use Intrusion Detection Systems (IDS)**: Monitor for unauthorized scans and mitigate potential threats.
- **Regular Network Audits**: Ensure that unnecessary services and ports are closed to reduce the attack surface.

---

## References

- [Official Nmap Documentation](https://nmap.org/book/)
- [Nmap Cheat Sheet](https://highon.coffee/blog/nmap-cheat-sheet/)

---

> **Pro Tip**: Use `nmap -Pn` to bypass ICMP echo request restrictions when scanning hosts that block ping requests!