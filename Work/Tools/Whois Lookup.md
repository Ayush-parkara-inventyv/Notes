## Overview

WHOIS lookup is a protocol used to query databases that store the registration details of domain names. It provides information about domain ownership, registrar details, expiration dates, and contact information.

## Key Terminology

- **WHOIS Database**: A publicly accessible database containing domain registration details.
- **Registrar**: The company responsible for managing domain name registrations.
- **Nameserver**: A server that translates domain names into IP addresses.

---

## Basic WHOIS Commands

### Checking Domain Registration Information

```bash
whois example.com
```

Displays registration details for `example.com`.

### Finding Registrar Information

```bash
whois -h whois.verisign-grs.com example.com
```

Queries a specific WHOIS server for more detailed information.

### Checking IP Address Ownership

```bash
whois 8.8.8.8
```

Displays the owner of the IP address `8.8.8.8`.

---

## Advanced WHOIS Lookup Techniques

### Finding Expired Domains

```bash
whois example.com | grep "Expiration Date"
```

Finds the expiration date of a domain.

### Identifying Domain History

Use third-party tools like:

- [Whois History Lookup](https://whois.domaintools.com/)
- [Wayback Machine](https://web.archive.org/)

### Checking Domain Availability

```bash
whois example.com | grep "No match"
```

Determines if a domain is available for registration.

### Discovering Abuse Contacts

```bash
whois example.com | grep -i "abuse"
```

Finds email addresses for reporting abuse related to a domain.

---

## Practical Use Cases

- **Cybersecurity Investigations**: Identifying malicious domains and their owners.
- **Domain Management**: Checking domain expiration dates and renewal details.
- **Competitive Research**: Finding out the owners of competitor domains.

---

## Mitigation Strategies

- **Enable WHOIS Privacy Protection**: Hides personal details from public WHOIS lookups.
- **Use GDPR-Compliant Registrars**: Some registrars redact personal information due to privacy regulations.
- **Monitor Domain Expiration**: Ensure domains are renewed on time to prevent hijacking.

---

## References

- [ICANN WHOIS Lookup](https://lookup.icann.org/)
- [DomainTools WHOIS](https://whois.domaintools.com/)

---

> **Pro Tip**: Use `whois -h whois.arin.net <IP>` to query ARIN for North American IP ownership details!