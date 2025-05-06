# Nuclei Tool
================
## Introduction
---------------

Nuclei is a fast and customizable vulnerability scanner that can be used to scan for various types of vulnerabilities, including SQL injection, cross-site scripting (XSS), and command injection. It is designed to be highly flexible and can be used to scan a wide range of web applications and APIs.

## Features
-----------

* **Template-based scanning**: Nuclei uses a template-based approach to scanning, which allows users to define custom scanning templates for different types of vulnerabilities.
* **Highly customizable**: Nuclei can be customized to scan for a wide range of vulnerabilities, including SQL injection, XSS, and command injection.
* **Support for multiple protocols**: Nuclei supports multiple protocols, including HTTP, HTTPS, and TCP.
* **Fast and efficient**: Nuclei is designed to be fast and efficient, making it suitable for large-scale vulnerability scanning.

## Installation
---------------

### Installing Nuclei

To install Nuclei, you can use the following command:
```bash
go get -u github.com/projectdiscovery/nuclei/v2/cmd/nuclei
```
Alternatively, you can download the pre-compiled binaries from the Nuclei GitHub repository.

### Installing Dependencies

To use Nuclei, you will need to have the following dependencies installed:

* **Go**: Nuclei is written in Go, so you will need to have Go installed on your system.
* **Git**: You will need to have Git installed to clone the Nuclei repository.

## Using Nuclei
--------------

### Basic Usage

To use Nuclei, you can use the following command:
```bash
nuclei -u http://example.com
```
This will scan the specified URL for vulnerabilities using the default scanning template.

### Customizing Scanning Templates

To customize the scanning template, you can use the `-t` option:
```bash
nuclei -u http://example.com -t my-template.yaml
```
This will use the custom scanning template defined in `my-template.yaml` to scan the specified URL.

### Supported Options

The following options are supported by Nuclei:

* `-u`: Specifies the URL to scan.
* `-t`: Specifies the scanning template to use.
* `-o`: Specifies the output file.
* `-v`: Enables verbose mode.
* `-h`: Displays help information.

## Scanning Templates
---------------------

### Introduction to Scanning Templates

Scanning templates are used to define the specific vulnerabilities to scan for. Nuclei supports a wide range of scanning templates, including SQL injection, XSS, and command injection.

### Creating a Custom Scanning Template

To create a custom scanning template, you can use the following format:
```yml
id: my-template
info:
  name: My Template
  author: My Name
  description: This is my custom template
requests:
  - method: GET
    path: /vuln
    body: |
      id=1
    headers:
      Content-Type: application/x-www-form-urlencoded
    matchers:
      - type: status
        status: 200
```
This template defines a single request to the `/vuln` path with a `GET` method. The `matchers` section specifies that the response status code should be 200.

### Supported Template Options

The following options are supported by Nuclei scanning templates:

* `id`: Specifies the unique ID of the template.
* `info`: Specifies metadata about the template.
* `requests`: Specifies the requests to make.
* `matchers`: Specifies the matchers to use.

## Matchers
------------

### Introduction to Matchers

Matchers are used to specify the conditions under which a vulnerability should be reported. Nuclei supports a wide range of matchers, including status code matchers, header matchers, and body matchers.

### Supported Matchers

The following matchers are supported by Nuclei:

* **Status matcher**: Matches the HTTP status code of the response.
* **Header matcher**: Matches a specific header in the response.
* **Body matcher**: Matches a specific string in the response body.

### Creating a Custom Matcher

To create a custom matcher, you can use the following format:
```yml
matchers:
  - type: status
    status: 200
  - type: header
    header: Set-Cookie
    regex: ^JSESSIONID=
```
This matcher specifies that the response status code should be 200 and that the `Set-Cookie` header should contain the string `JSESSIONID`.

## Output
----------

### Introduction to Output

Nuclei supports a wide range of output formats, including JSON, CSV, and Markdown.

### Supported Output Options

The following output options are supported by Nuclei:

* **JSON**: Outputs the results in JSON format.
* **CSV**: Outputs the results in CSV format.
* **Markdown**: Outputs the results in Markdown format.

### Customizing Output

To customize the output, you can use the `-o` option:
```bash
nuclei -u http://example.com -o output.json
```
This will output the results in JSON format to the `output.json` file.

## Integrations
--------------

### Introduction to Integrations

Nuclei supports a wide range of integrations, including GitHub, JIRA, and Slack.

### Supported Integrations

The following integrations are supported by Nuclei:

* **GitHub**: Integrates with GitHub to create issues and pull requests.
* **JIRA**: Integrates with JIRA to create issues and tickets.
* **Slack**: Integrates with Slack to send notifications.

### Creating a Custom Integration

To create a custom integration, you can use the following format:
```yml
integrations:
  - type: github
    token: my-token
    repo: my-repo
```
This integration specifies that Nuclei should create issues in the `my-repo` repository using the `my-token` token.

## Conclusion
----------

Nuclei is a powerful and customizable vulnerability scanner that can be used to scan for a wide range of vulnerabilities. Its template-based approach to scanning makes it highly flexible and adaptable to different scanning scenarios. With its support for multiple protocols, high performance, and customizable output, Nuclei is an ideal tool for vulnerability scanning and penetration testing.