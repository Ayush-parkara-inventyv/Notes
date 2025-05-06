https://medium.com/@hashsleuth.info/how-to-exploit-dvwa-blind-sql-injection-sqli-with-sqlmap-and-burp-suite-e4b3f08a0dfc

Blind SQL injection occurs when a software application is susceptible to SQL injection attacks, but it does not reveal the outcomes of the executed SQL queries or any specific details about potential database errors in the application’s HTTP responses. In simpler terms, even though an attacker can inject malicious SQL code into the application, they won’t receive direct feedback or visible evidence of the results. This lack of feedback makes blind SQL injection more challenging for attackers; however, it still allows them to potentially exploit vulnerabilities and gain unauthorized access to the application’s database.

In this tutorial, we will explore how to leverage two powerful tools, SQLMap and Burp Suite, to exploit a blind SQL injection vulnerability in DVWA. However, it is essential to note that this tutorial aims to raise awareness and educate about potential security risks, emphasizing ethical hacking to enhance web application security.

# Setting Up DVWA and Burp Suite

If you’re looking for detailed step-by-step instructions on how to install DVWA in your local environment, I recommend checking out my previous article on the topic titled ‘How to Install and Run DVWA in Kali Linux.’ There, you’ll find a comprehensive guide that will walk you through the entire setup process, including the installation of necessary software, configuring the database, and launching DVWA on your local machine.

Burp Suite, developed by Portswigger and its founder Dafydd Stuttard, is a comprehensive set of tools used for penetration testing web applications. The aim of Burp Suite is to provide an all-in-one solution for web app security, and its capabilities can be expanded by installing add-ons known as BApps. This tool has gained immense popularity among professional web app security researchers and bug bounty hunters for its effectiveness and efficiency.

# Set-up proxy configuration

To configure a proxy with Burp Suite using the FoxyProxy extension in Firefox, follow these steps:

1. Download and install the FoxyProxy extension from the Firefox add-ons store.
2. Once installed, click on the FoxyProxy icon in the Firefox browser’s toolbar.
3. From the menu, select “Options.”
4. In the FoxyProxy options, click on “Add New Proxy.”
5. In the “Proxy Details” window, provide the following settings:

- Proxy IP Address: Enter “127.0.0.1” (localhost).
- Proxy Port: Enter “8080.”

1. Click “Save” to save the new proxy configuration.

2. After saving, you will see the newly added proxy in the FoxyProxy options list.

3. To use the newly configured proxy, simply click on the FoxyProxy icon again and select the desired proxy from the list.

Now, your proxy is set up with Burp Suite, and you can use it for intercepting and analyzing web traffic in Firefox.

In my proxy setup, I’m utilizing the localhost address (127.0.0.1) with port 8080. This configuration is necessary as DVWA is hosted on my local machine. I’ve synchronized both Firefox and Burp Suite to share the same proxy settings under the Proxy and Options tab. Consequently, when I access DVWA in Firefox, it will automatically utilize Burp Suite as the proxy, allowing for seamless interaction and analysis.

![](https://miro.medium.com/v2/resize:fit:875/1*gdN9EU643TUodSo5yQokOQ.png)

proxy configuration

With the proxy connection set up, Burp Suite takes control of all local network traffic on port 8080.

# SQLmap

SQLmap is a free and open-source tool used for penetration testing. Its main purpose is to automate the identification and exploitation of SQL injection vulnerabilities, ultimately gaining control over database servers.

The version I am currently using is 11.7.6#stable. If you do not have sqlmap installed, you can easily install it through the terminal by using the command:

apt-get install sqlmap

![](https://miro.medium.com/v2/resize:fit:875/1*zgseZVBTVE2LNEVqmPE9qg.png)

SQLmap

# Cookie harvesting

After logging into DVWA (Damn Vulnerable Web Application), navigate to the “SQL Injection (Blind)” section. Enter the value ‘2’ for the ‘id’ parameter, and then click the submit button. However, you might notice that nothing happens in the browser. This is because Burp Suite, the interception proxy, has intercepted the GET request in the URL. As a result, you won’t be able to successfully submit the request until you click the “Forward” option in Burp Suite to allow the request to proceed.

![](https://miro.medium.com/v2/resize:fit:875/1*rqAL4eNbqeFUwotOOhv9mw.png)

Now, let’s proceed to the “Proxy” section in our Burp Suite. I have highlighted the cookie and session identifiers. Copy this information as we will use it in the next step.

![](https://miro.medium.com/v2/resize:fit:875/1*Ni_HEE2BaZfx3vNNAiDqiQ.png)

cookie and session identifiers

After clicking the “Forward” option in Burp Suite, go back to our browser. You’ll notice that after forwarding the request, our URL has changed, as indicated by the highlighting. Now, copy the updated URL as we will use it in our next step.

![](https://miro.medium.com/v2/resize:fit:875/1*2HHtL8QIQEU_K1g92EG8lQ.png)

# Scanning for vulnerabilities

In your terminal window, execute the following command:

```bash
sqlmap -u "http://127.0.0.1/dvwa/vulnerabilities/sqli_blind/?id=2&Submit=Submit#" - cookie="security=low; PHPSESSID=9tp5mn3hpkfgh6fddmbampps7m"
```

After pressing Enter, SQL map will establish a connection to the provided URL. It will then check if the GET parameter ‘id’ is dynamic, meaning it can be manipulated. Additionally, during the process, you can observe the actual payloads being submitted by SQL map as it attempts to exploit the SQL injection vulnerability.

From the output below, it is evident that the “id” GET parameter in DVWA is susceptible to Boolean-based blind injection vulnerabilities.

![](https://miro.medium.com/v2/resize:fit:875/1*2xRL3i6rqQdqIU-FDZcylg.png)

SQL map prompt

![](https://miro.medium.com/v2/resize:fit:875/1*GT6TDpjaHXtPIplQ9FZplA.png)

SQL map prompt

![](https://miro.medium.com/v2/resize:fit:875/1*aVa8JX6mKDyw4OlzjFQoCQ.png)

further information

Additionally, from the above output, we can gather further information about the target website. It appears that the website is running on Linux Debian, and it is using Apache version 2.4.57. Furthermore, the back-end database management system (DBMS) is identified as MySQL.

To utilize the default behavior and bypass any interactive prompts, you can add the _“— batch’’_ option to the SQL map command.

# Revealing the database schemas

to enumerate the available database schemas, use the following command:

```bash
sqlmap -u "http://127.0.0.1/dvwa/vulnerabilities/sqli_blind/?id=2&Submit=Submit#" --cookie="security=low; PHPSESSID=9tp5mn3hpkfgh6fddmbampps7m" --schema --batch   
```

![](https://miro.medium.com/v2/resize:fit:875/1*xQgPsVR0ZR0fxYf6St8zcg.png)

schema information

Upon enumeration, two database names were retrieved: “dvwa” and “information_schema.” However, my focus is solely on the “dvwa” database, as it is the one of interest.

# Revealing the dvwa tables

To retrieve the tables in the DVWA database, you can use the following command:

```bash
sqlmap -u "http://127.0.0.1/dvwa/vulnerabilities/sqli_blind/?id=2&Submit=Submit#" --cookie="security=low; PHPSESSID=9tp5mn3hpkfgh6fddmbampps7m" -D dvwa --tables
```  

The command _-D_ was used to enumerate the database, while _— tables_ was used to enumerate the tables within the DBMS database.

![](https://miro.medium.com/v2/resize:fit:875/1*-UAn7bGd0aO4W2X8H-nbig.png)

dvwa tables

The SQLMap output reveals two tables: “guestbook” and “users.” In our case, our focus is on the “users” table.

# Enumerate the “users” table.

To determine the columns created in the “users” table within DVWA, you can use the following command:

```bash
sqlmap -u "http://127.0.0.1/dvwa/vulnerabilities/sqli_blind/?id=2&Submit=Submit#" --cookie="security=low; PHPSESSID=9tp5mn3hpkfgh6fddmbampps7m" --columns -T users --batch
```  

The command _-T_ is used to enumerate the tables within the DBMS database, and _— columns_ is used to enumerate the columns within a specific table.

![](https://miro.medium.com/v2/resize:fit:561/1*OKx38RRGzG63qgrgRNjxkw.png)

table users columns

The SQLMap output indicates a total of 8 columns inside the “users” table. However, for our current interest, we are only concerned with the “user” and “password” columns.

# Dump Usernames and Passwords

To retrieve the usernames and passwords from the “user” and “password” columns in the “users” table, use the following command:

```bash
sqlmap -u "http://127.0.0.1/dvwa/vulnerabilities/sqli_blind/?id=2&Submit=Submit#" --cookie="security=low; PHPSESSID=9tp5mn3hpkfgh6fddmbampps7m" --dump -T users --batch
```

The _-C_ command is used to specify the column to enumerate, and the _— dump_ command is used to dump the table entries,

![](https://miro.medium.com/v2/resize:fit:875/1*YWz3FmCz7Sc4lECiH3ZRVA.png)

table users entries

Sqlmap successfully retrieves the entries from the “user” and “password” columns in the “users” table of the DVWA database. It asks if hashes can be stored (default: no) and offers to crack them using a dictionary attack (default: yes). SQLmap launches the attack using the “wordlist.txt” file (default) or a custom dictionary if specified. It also asks about using common password suffixes (default: no).

**Sqlmap cracks the password hashes using a dictionary-based attack and outputs the plaintext passwords.**

# Conclusion

Thank you for taking the time to read this article. Your feedback and suggestions are greatly appreciated. Please feel free to leave your thoughts and comments.