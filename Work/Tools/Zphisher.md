## Overview
Zphisher is an advanced phishing tool designed to automate the process of creating phishing pages for various websites and platforms. The tool is primarily used for penetration testing, social engineering, and ethical hacking exercises. Zphisher allows users to create phishing pages for a wide range of popular websites and social media platforms like Facebook, Instagram, Twitter, and more. It is designed to be easy to use and can generate a working phishing page with minimal effort.

The tool automates the process of creating fake login pages and captures credentials entered by users. Once a victim enters their credentials, Zphisher collects the data and stores it for the attacker. It can be used for educational purposes and to demonstrate the risks of phishing attacks.

---

## Key Terminology
- **Phishing**: A type of cyber attack where the attacker impersonates a legitimate service to steal sensitive information like usernames, passwords, or financial details.
- **Credential Harvesting**: The process of collecting login credentials from unsuspecting victims via phishing pages.
- **Social Engineering**: A manipulation technique that exploits human psychology to gather confidential information.
- **Clone Attack**: Creating a copy of a website to trick users into entering their sensitive data.
- **Payload**: The part of the attack where the victim's data is captured (e.g., when they submit their credentials on a phishing page).

---

## Installing Zphisher

Zphisher is a simple tool that can be easily installed on Linux-based systems. Follow these steps to install Zphisher.

### 1. Prerequisites
Ensure you have the following tools installed on your system before proceeding with the installation:
- **Git**: To clone the Zphisher repository.
- **PHP**: For the server-side execution of the phishing scripts.
- **Curl**: For making network requests.
- **Ngrok**: For tunneling traffic to your local server (optional but recommended).

#### Install Dependencies
```bash
sudo apt update
sudo apt install git php curl unzip
````

### 2. Clone Zphisher Repository

Clone the Zphisher repository from GitHub using the following command:

```bash
git clone https://github.com/htr-tech/zphisher.git
```

### 3. Navigate to the Zphisher Directory

Once the repository is cloned, navigate into the Zphisher directory:

```bash
cd zphisher
```

### 4. Run Zphisher

Zphisher does not require installation beyond cloning the repository. You can directly run the script using:

```bash
bash zphisher.sh
```

{picture: Insert image showing terminal output after cloning Zphisher}

---

## Using Zphisher

Zphisher is designed to be user-friendly, offering an interactive menu to select various phishing templates. Here's how to use it:

### 1. Start Zphisher

After navigating to the Zphisher directory, start the tool by running:

```bash
bash zphisher.sh
```

This will present you with a menu of options for different phishing templates.

{picture: Insert image showing Zphisher menu options}

### 2. Choose a Phishing Template

Zphisher offers a wide range of phishing templates for popular websites. You can choose the website you want to create a phishing page for from the list of available options. Some popular templates include:

- Facebook
- Instagram
- Google
- Twitter
- Yahoo
- Snapchat

Simply type the number corresponding to the website you want to create a phishing page for.

{picture: Insert image showing Zphisher phishing template selection}

### 3. Configure the Server

After selecting the phishing template, Zphisher will ask if you want to use **Ngrok** to tunnel traffic to your local server. Using Ngrok allows you to get a public URL for the phishing page. You can either choose to use Ngrok or continue without it.

If you opt for Ngrok, you'll need to install it if you haven't already:

```bash
sudo apt install ngrok
```

Then, you'll be prompted to sign up for an Ngrok account and authenticate the tool.

{picture: Insert image showing Ngrok setup in Zphisher}

### 4. Launch the Phishing Page

Once the server is configured, Zphisher will generate the phishing page and start hosting it locally. The tool will display a URL (either local or via Ngrok) that you can share with potential victims. When a victim clicks the link and enters their credentials on the phishing page, Zphisher will capture the data and display it in the terminal.

{picture: Insert image showing phishing page URL and terminal output capturing credentials}

---

## Zphisher Features

### 1. Social Media Phishing Templates

Zphisher comes pre-loaded with various social media login pages, which are commonly targeted for phishing attacks. Some examples include:

- Facebook
- Instagram
- Twitter
- Snapchat
- LinkedIn

These templates are designed to closely resemble the real login pages of the respective platforms.

### 2. Email Services Phishing Templates

In addition to social media, Zphisher also includes templates for popular email services, such as:

- Gmail
- Yahoo
- Outlook
- ProtonMail

### 3. Custom URL Generator

Zphisher allows users to generate a custom phishing URL. This can be done by either creating a custom phishing page or modifying an existing template.

### 4. Real-Time Data Capture

Once a victim submits their login credentials on the phishing page, Zphisher instantly displays the collected data (e.g., username, password) in real time within the terminal.

### 5. Multi-Page Support

Zphisher supports multi-page phishing, meaning it can create and manage phishing pages for multiple websites simultaneously, and capture credentials from each.

{picture: Insert image showing multiple phishing pages in use}

---

## Advanced Usage

### 1. Customizing Phishing Pages

Zphisher is designed to be flexible and allows you to customize phishing pages according to your needs. You can modify the HTML and PHP files in the phishing template directories to change the appearance and behavior of the phishing page.

For example, to modify the **Facebook phishing page**, navigate to the directory containing the Facebook template:

```bash
cd templates/facebook
```

Here, you can edit the HTML or PHP files to suit your specific requirements.

### 2. Using Zphisher in an Automated Attack

Zphisher can be used in an automated manner with scripting. You can specify parameters such as the phishing URL and template to be used, which allows for a more streamlined approach to phishing attacks.

For example, use the following command to automate the creation of a phishing page:

```bash
bash zphisher.sh -t facebook -u victim@example.com
```

This command will automatically create a phishing page for Facebook and send the victim’s information to the specified email address.

{picture: Insert image showing Zphisher command-line automation}

---

## Ethical Considerations and Legal Implications

### 1. Ethical Use of Zphisher

Zphisher is intended for ethical use in controlled environments, such as penetration testing, security research, and educational purposes. It should not be used to conduct illegal activities, such as targeting unsuspecting individuals or organizations without permission.

### 2. Legal Implications

Phishing attacks are illegal and highly punishable by law. Unauthorized access to systems or accounts through phishing can result in serious legal consequences, including imprisonment. Always ensure that you have explicit permission before conducting any phishing campaigns.

---

## Mitigation Strategies

### 1. Awareness Training

Organizations and individuals should conduct regular awareness training to educate people about phishing attacks. Training should cover the risks associated with phishing, how to recognize suspicious emails and links, and how to avoid falling victim to attacks.

### 2. Multi-Factor Authentication (MFA)

Enabling multi-factor authentication (MFA) significantly reduces the risk of phishing attacks, as it requires an additional layer of authentication (e.g., an authentication app or SMS code) to access accounts.

### 3. Use of Password Managers

Password managers help users create and store strong, unique passwords for each website, reducing the likelihood of a successful phishing attack. They also help prevent users from reusing passwords across multiple sites.

### 4. Phishing Detection Tools

Organizations can use anti-phishing tools and email filters to detect and block phishing attempts. These tools often rely on known patterns of phishing emails and websites to identify malicious content.

---

## References

- [Zphisher GitHub Repository](https://github.com/htr-tech/zphisher)
- [Phishing Awareness and Training Resources](https://www.phishing.org/phishing-awareness-training)
