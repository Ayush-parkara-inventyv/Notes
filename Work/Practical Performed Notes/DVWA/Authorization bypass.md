https://www.youtube.com/watch?v=Qcgu34eWQa4
## What is Access Control

Access control is a fundamental security practice that regulates who or what can view or use resources in a computing environment. It ensures that only authorized individuals, systems, or devices can access specific data, applications, or physical locations, thereby protecting sensitive information and maintaining system integrity.

**Key Components of Access Control:**

1. **Identification:** Recognizing a user or device attempting to access a resource, typically through unique credentials like usernames or ID numbers.
    
2. **Authentication:** Verifying the claimed identity, often by matching passwords, biometric data, or security tokens.
    
3. **Authorization:** Determining the permissions or privileges assigned to the authenticated entity, defining what actions they are allowed to perform.
    
4. **Accountability:** Tracking and logging user activities to ensure actions can be traced back to the responsible party, aiding in audits and compliance.
    

**Types of Access Control Models:**

1. **Discretionary Access Control (DAC):** The resource owner determines access permissions. While flexible, it can lead to security risks if not managed carefully.
    
2. **Mandatory Access Control (MAC):** A central authority enforces strict access policies based on classifications. Users cannot alter access permissions, making it suitable for highly sensitive environments.
    
3. **Role-Based Access Control (RBAC):** Access rights are assigned based on user roles within an organization, streamlining management and ensuring users have only necessary permissions.
    
4. **Attribute-Based Access Control (ABAC):** Access decisions are based on evaluating attributes (e.g., user characteristics, resource types, environmental factors), offering fine-grained control.
    

**Importance of Access Control:**

Implementing robust access control measures is crucial for:

- **Protecting Sensitive Data:** Preventing unauthorized access reduces the risk of data breaches and leaks.
    
- **Ensuring Compliance:** Many regulations require strict access controls to protect personal and financial information.
    
- **Maintaining System Integrity:** Restricting access minimizes the potential for malicious activities and accidental errors.
    
- **Enhancing Trust:** Clients and partners are more likely to trust organizations that demonstrate strong security practices.
    

**Best Practices for Access Control:**

- **Principle of Least Privilege:** Grant users only the access necessary for their roles to minimize potential risks.
    
- **Regular Audits and Reviews:** Continuously monitor and adjust access rights to align with current roles and responsibilities.
    
- **Multi-Factor Authentication (MFA):** Implement additional verification steps to strengthen authentication processes.
    
- **Centralized Access Management:** Utilize unified systems to oversee and enforce access policies consistently across the organization.
    

By adhering to these principles and practices, organizations can effectively safeguard their resources and maintain a secure operational environment.

### Authorization Bypass: Definition and Explanation

**Authorization bypass** is a security vulnerability that allows an attacker to gain unauthorized access to restricted resources or functionalities by circumventing access control mechanisms. This flaw often occurs due to improper implementation of authentication and authorization checks, enabling attackers to exploit weaknesses in a system to perform actions they shouldn't have permission for.

---

### How Authorization Bypass Works

In a secure system, a proper authorization process ensures that users can only access specific resources based on their roles and privileges. However, authorization bypass vulnerabilities arise when:

1. **Missing or Weak Authorization Checks** – The application assumes that a user is authorized without verifying their permissions properly.
2. **Direct URL Access (Insecure Direct Object References - IDOR)** – Attackers manipulate URLs to access unauthorized resources.
3. **Parameter Tampering** – Modifying parameters in requests (e.g., user IDs, access tokens) to escalate privileges.
4. **Cookie Manipulation** – Altering session cookies to gain unauthorized access.
5. **Client-Side Security Reliance** – Relying only on client-side checks, which attackers can easily bypass using browser tools.
6. **Unrestricted API Endpoints** – APIs exposing sensitive operations without proper authentication controls.

### Low

### **DVWA - Low Level Authorization Bypass Exploitation**

#### **Vulnerability Type: Authorization Bypass (Insecure Direct Object Reference - IDOR)**

**Description:**  
In the **Low security level** of Damn Vulnerable Web Application (DVWA), the authentication mechanism lacks proper authorization checks. This allows attackers to bypass authentication and gain unauthorized access to restricted pages by directly navigating to the target URL.

---

### **Steps to Exploit Authorization Bypass in DVWA (Low Level)**

1. **Access the DVWA Admin Panel:**
    - Log in as an administrator and navigate to the admin panel to identify the URL format of sensitive pages.
    - Example URL:
  ```
http://dvwa.local/admin.php
```

1. **Extract the URL for Exploitation:**
    
    - Check if any sensitive page URLs are accessible without proper authentication.
    - If the application does not validate session tokens or user roles properly, an attacker can access these pages directly.
3. **Login as a Different User (Attacker Account):**
    
    - Open another browser or use **Incognito Mode** to log in as a normal user, such as `gordonb`.
4. **Manually Enter the Admin URL:**
    
    - Copy the admin panel URL from the first session and enter it in the second session (attacker's browser).
    - Example:
```bash
http://dvwa.local/admin.php
```

If the application does not validate user roles properly, the page will load, granting unauthorized access.
![](https://i.imgur.com/U6M7SGd.png)

![](https://i.imgur.com/Epl6ZUL.png)


![](https://i.imgur.com/yM2lzwF.png)
![](https://i.imgur.com/yUDs8Ya.png)
### Medium

#### Vulnerability Type: Authorization Bypass (Direct Access to Unprotected Endpoints)

#### **Description:**

In the **Medium security level** of DVWA, direct access to certain backend endpoints is possible, even though the main authorization check is enforced on the **parent page** (`authbypass/index.php`). Attackers can exploit this by directly requesting sub-files that handle sensitive operations.

---

### **Steps to Exploit Authorization Bypass in DVWA (Medium Level)**

1. **Access the Restricted Page:**
    
    - Navigating to:
        `http://localhost/DVWA/vulnerabilities/authbypass/`
        
        shows `"Unauthorised"` and returns **403 Forbidden**.
2. **Analyze the Backend Code & Requests:**
    
    - The PHP code references two files:
        `get_user_data.php change_user_details.php`
        
    - These may be handling sensitive operations **without additional authentication checks**.
3. **Directly Access the Backend Endpoint:**
    
    - Enter the URL manually:
    `http://localhost/DVWA/vulnerabilities/authbypass/get_user_data.php`
        
    - If this file lacks proper authorization checks, it will execute and return user data.
4. **Confirm the Exploit Works:**
    
    - If successful, sensitive user data is retrieved without admin privileges.
    - The attacker can also try `change_user_details.php` to modify user credentials.

![](https://i.imgur.com/oulpBfG.png)

![](https://i.imgur.com/MrNVMEO.png)


http://localhost/DVWA/vulnerabilities/authbypass/

![](https://i.imgur.com/eMk2SgY.png)

http://localhost/DVWA/vulnerabilities/authbypass//get_user_data.php

![](https://i.imgur.com/3WaFu05.png)


### High

![](https://i.imgur.com/s55oCoi.png)

check high source code 
![](https://i.imgur.com/Lh1G6xF.png)
this is source code
```php 
 <?php
/*

Only the admin user is allowed to access this page.

Have a look at this file for possible vulnerabilities: 

* vulnerabilities/authbypass/change_user_details.php

*/

if (dvwaCurrentUser() != "admin") {
    print "Unauthorised";
    http_response_code(403);
    exit;
}
?>
```
check the site from admin user and get a post request to update db
![](https://i.imgur.com/OCOYH7i.png)

```
curl 'http://localhost/DVWA/vulnerabilities/authbypass/change_user_details.php' -X POST -H 'User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:128.0) Gecko/20100101 Firefox/128.0' -H 'Accept: application/json' -H 'Accept-Language: en-US,en;q=0.5' -H 'Accept-Encoding: gzip, deflate, br, zstd' -H 'Referer: http://localhost/DVWA/vulnerabilities/authbypass/' -H 'Content-Type: application/json' -H 'Origin: http://localhost' -H 'Connection: keep-alive' -H 'Cookie: PHPSESSID=q2rt5eeucotrpq2vefvlnsfh9s; security=high' -H 'Sec-Fetch-Dest: empty' -H 'Sec-Fetch-Mode: cors' -H 'Sec-Fetch-Site: same-origin' -H 'Priority: u=0' --data-raw '{"id":5,"first_name":"Bob","surname":"Smith"}'
```
then visit same link but it will give unauthorized to gordonb user 
![](https://i.imgur.com/3Gyqiv2.png)

also it will show result fail like below photo so the other user is not allowed to do changes in db
![](https://i.imgur.com/qKSyUQk.png)

now collect the post request from admin to change the values in  db
![](https://i.imgur.com/O1eqW93.png)

use the same post request and change the user and password means account of gordonb and use his seesion cookie in the post request we copied
![](https://i.imgur.com/sWBhj2q.png)
then try to submit the request you will get the db updated showing in below image 
![](https://i.imgur.com/fk4B52F.png)
this way we can exploit the high level vuln