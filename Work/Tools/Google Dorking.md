## Overview

Google Dorking, also known as Google hacking, is a technique used to uncover hidden information on the internet using advanced search queries. It leverages Google's search operators to find exposed databases, login pages, sensitive files, and misconfigured websites.

## Key Terminology

- **Google Dork**: A specialized search query designed to extract specific information.
- **Search Operator**: A command that refines Google searches (e.g., `site:`, `filetype:`, `intitle:`).
- **Indexing**: The process by which search engines collect and store website data.

---

## Basic Google Dorks

### Searching Within a Specific Site

```bash
site:example.com
```

Finds all indexed pages of `example.com`.

### Searching for Specific File Types

```bash
filetype:pdf "confidential"
```

Finds PDFs containing the word "confidential".

### Finding Exposed Directories

```bash
intitle:"index of" "parent directory"
```

Locates open directories that may contain sensitive files.

---

## Advanced Google Dorking Techniques

### Finding Login Pages

```bash
inurl:admin login
```

Finds administrative login portals.

### Exposed Database Files

```bash
ext:sql | ext:db | ext:sqlite "password"
```

Searches for exposed database files containing passwords.

### Discovering Camera Feeds

```bash
inurl:/view/view.shtml
```

Finds publicly accessible webcams.

### Locating Configuration Files

```bash
inurl:wp-config.php
```

Finds WordPress configuration files that may contain database credentials.

### Finding Open FTP Servers

```bash
intitle:"index of" inurl:ftp
```

Searches for FTP servers with publicly accessible directories.

---

## Practical Use Cases

- **Cybersecurity Audits**: Identifying misconfigured or exposed assets.
- **Ethical Hacking**: Conducting penetration testing to secure systems.
- **Bug Bounty Hunting**: Discovering security flaws for responsible disclosure.

---

## Mitigation Strategies

- **Robots.txt Restrictions**: Prevent sensitive directories from being indexed.
- **Proper Access Controls**: Restrict access to sensitive files and databases.
- **Regular Security Audits**: Ensure that no confidential data is publicly accessible.

---

## References

- [Google Search Operators Guide](https://support.google.com/websearch/answer/2466433)
- [Exploit-DB Google Hacking Database](https://www.exploit-db.com/google-hacking-database)

---

> **Pro Tip**: Use `-site:example.com` to exclude a domain from search results!