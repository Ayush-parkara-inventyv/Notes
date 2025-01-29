# Gitleaks

## Overview
Gitleaks is a tool used to detect and prevent sensitive information such as API keys, passwords, and other secrets from being exposed in Git repositories. It scans commit history, branches, and pull requests to identify potentially leaked secrets in code. Gitleaks helps developers and organizations maintain secure coding practices by preventing accidental exposure of sensitive data.

## Key Terminology
- **Git Repositories**: A version-controlled storage system for code, often used for collaborative development.
- **Sensitive Information**: Data such as API keys, passwords, tokens, and private credentials that should not be publicly exposed.
- **Secret Scanning**: The process of searching for and identifying sensitive information that may have been accidentally committed to version control.
- **Regular Expressions (Regex)**: Pattern-matching sequences used by Gitleaks to identify potential secrets based on predefined patterns.

---

## Basic Gitleaks Commands
### Scanning a Repository
```bash
gitleaks detect --source=/path/to/repo
````

Scans the specified repository for any leaked secrets, providing a report of potential findings.

### Scanning a Specific Branch

```bash
gitleaks detect --source=/path/to/repo --branch=main
```

Scans the `main` branch of the repository for secrets.

### Scanning with a Custom Configuration

```bash
gitleaks detect --source=/path/to/repo --config=/path/to/config.json
```

Uses a custom configuration file for scanning, which allows for more tailored detection rules.

---

## Advanced Gitleaks Techniques

### Scan a Specific Commit

```bash
gitleaks detect --source=/path/to/repo --commit=abcdef123456
```

Scans a specific commit (`abcdef123456`) in the repository to check for secrets.

### Scan Only the Latest Commits

```bash
gitleaks detect --source=/path/to/repo --since=2021-01-01
```

Scans commits in the repository since a specified date (`2021-01-01`) for secrets.

### Excluding Specific Files

```bash
gitleaks detect --source=/path/to/repo --exclude=path/to/exclude/*
```

Excludes files or directories from the scan to focus on specific parts of the repository.

---

## Practical Use Cases

- **Preventing Secret Leaks**: Regularly scan Git repositories to identify and prevent accidental exposure of sensitive information in code.
- **Continuous Integration (CI)**: Integrate Gitleaks into a CI pipeline to automatically detect secrets in new commits and pull requests.
- **Audit Past Commits**: Scan historical commits to identify if any secrets were accidentally exposed in the past.
- **Compliance and Security Audits**: Ensure that repositories adhere to security policies by proactively detecting leaked secrets.

---

## Mitigation Strategies

- **Use Environment Variables**: Store sensitive data in environment variables instead of hardcoding it in the source code.
- **Enable Secret Scanning in CI**: Integrate secret scanning tools like Gitleaks into the CI pipeline to ensure all commits are scanned for secrets before they are merged.
- **Rotate Exposed Secrets**: Immediately rotate any exposed secrets (API keys, tokens) if a leak is detected.
- **Educate Developers**: Train developers on secure coding practices and the importance of not committing sensitive information to repositories.

---

## References

- [Gitleaks GitHub Repository](https://github.com/zricethezav/gitleaks)