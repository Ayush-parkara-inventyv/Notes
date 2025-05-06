![](https://miro.medium.com/v2/resize:fit:875/0*dlEGfSQk6r6e839c.jpg)
# DVWA Reflected XSS Exploit

![dvwa-reflected-xss](https://ethicalhacs.com/media/2020/05/featured-image-1.jpg "dvwa-reflected-xss-exploit")

In this article, I am going to show you how to exploit Reflected XSS (Cross Site Scripting) vulnerability in the DVWA web application (a vulnerable web application for enhancing hacking skills offline). Before we begin, let us know something about XSS like,

> What is XSS attack?

> What are the types of XSS attack?

> Why this vulnerability arises?

Let’s move toward our first question

## What is XSS attack??

==[**OWASP**](https://owasp.org/www-community/attacks/xss/)== (Open Web Application Security Project an online community that produces freely-available articles, methodologies, documentation, tools, and technologies in the field of web app security) ==defines XSS as an injection attack in which an attacker tries to inject some malicious code in the website’s input field==. This code is merely one or two-line script written in any language that a web browser executes. Most importantly, in an XSS attack, an attacker tries to inject some HTML code or some JavaScript code in the input field of the website. The input field can be anywhere in the website’s page. The user gives his input using this input field. The input field can be an HTML form, a Signup button, or an HTTP header like HOST, COOKIE, USER-AGENT, etc.

Even if an attacker can inject any client-side code in the input field, but his main focus is on injecting such code that will give a popup on the user’s browser screen. The popup is an alert box that proves that the given website is vulnerable to an XSS attack. The universal and most basic method to get a pop-up window on the browser screen is to include `**alert()**` function inside the script tag (`**<script>…. </script>**`). So, whenever an attacker injects payload (a group of words & characters) **`<script> alert() </script>`** in any of the input fields of the website, it will pop up an alert box on the browser screen which confirms that the website is vulnerable to XSS attack. Let’s move towards our second question which is, **what are the types of XSS attacks?**

## **What are the types of XSS attacks?**

There are three types of XSS attacks namely:-

1. Reflected XSS
2. Stored XSS
3. DOM Based XSS

Let us understand the concept behind each type of attack

### **1. Reflected XSS**

Reflected XSS occurs when the input supplied by the user reflects back in the browser window or inside **page source** of the web page. **What does it mean?** Let us understand it with an **example**, suppose I have entered some value let’s say `**thisisreflecting**` in the input field of the website, now open the source of the page by pressing **`CTRL+U`** and search for the string **`thisisreflecting`** in the **page source.** If this word (**thisisreflecting**) is reflected or present in the **page source** then that parameter which is accepting the input may be vulnerable to reflected XSS. Now, you can try the payload **`<script> alert() </script>`** in place of **`thisisreflecting`** in the same input field. If it is vulnerable it will give a popup.

### **2. Stored XSS**

Stored XSS occurs when the input supplied by the user is stored on the server side without performing proper **sanitization** or **HTML encoding**. The storage place can be a **database**, a **message** **forum**, a **visitor log**, a **comment** field, etc. Instead of reflecting back immediately, it may reflect back when you login into the website the next time. When you visit the vulnerable web page you will get a pop-up as alert window. Stored XSS is more dangerous than reflected XSS because it will harm the whole community by popping an alert box on every user’s browser who visits the vulnerable page. The payload used in stored XSS is same as reflected XSS.

For more info on Stored XSS and its exploitation on the DVWA app check **[this](https://ethicalhacs.com/dvwa-stored-xss-exploit/)** article.

### **3. DOM-Based XSS**

**DOM** XSS stands for Document Object Model based Cross-Site Scripting. Here, the vulnerability appears in the DOM instead of part of the HTML. It is different from the other two types of XSS in the way that in DOM-based XSS user-supplied input doesn’t reflect back in the source code but the user can see the data in the HTML DOM tree using **`CTRL+SHFT+C`** and taking mouse pointer to the input field where he has entered his data and observing the DOM tree.

According to W3C – _“**The W3C Document Object Model (DOM) is a platform and language-neutral interface that allows programs and scripts to dynamically access and update the content, structure, and style of a document.**“_

To exploit this vulnerability, an attacker tries to inject some **JavaScript code** into the **Document Object Model** or **HTML DOM**. It has the capability to dynamically modify html tags, CSS properties, and content of HTML documents. And hence, can be used to inject JavaScript code to get a popup on the screen.

For more info on **DOM-Based XSS** and its exploitation on the DVWA app check **[this](https://ethicalhacs.com/dvwa-dom-xss-exploit/)** article.

## **Why XSS vulnerability arises?**

XSS vulnerability occurs in those websites which take input from the user and while reflecting back in the browser they don’t perform any sanitization or content encoding. **Sanitization** means removing some unwanted characters like `**>,<,“,”,%**`, etc. from the input string. And encoding means changing bad characters ( `**>,<,“,”,%**` ) to some other similar meaning characters including numbers and letters so that they will not reflect back in the same format as they were entered.

Now I will show you how you can exploit Reflected XSS vulnerability in the DVWA application. Although, there are various ways to exploit Reflected XSS in Dvwa but we are going to cover the most basic one. So let’s begin,

## How to Exploit Reflected XSS in DVWA?

### **Hacking Steps**

**These are the general steps that have to be followed every time while testing for reflected XSS on any website**:

1. Firstly, input some unique string in the form field and submit it.
2. Open the `**page**` **`source`** by right click or by pressing CTRL+U and search the unique string in the Page Source.
3. Use CTRL+F to find the unique string which you have entered in step one. If the unique string reflects back in the browser screen or in the page source then the site may be vulnerable to Reflected XSS.
4. At last, enter the basic payload of XSS viz, **`<script>alert()</script>`** and submit it to get further response in the browser. If the site is vulnerable you will get an alert box.

Now it’s time to demonstrate our attack on our vulnerable Web Application which is running on our **local host**.

Login in your DVWA web app by default creds, username **`admin`** and password **`password`**, or use your own **creds** if you have created them.

![dvwa-login-page](https://ethicalhacs.com/media/2020/05/dvwa-login.jpeg "dvwa-login-page")

## **Low Security**

We will begin from very basic level so set the security level to **`low`** and click on **`submit`** button.

![dvwa-security-set](https://ethicalhacs.com/media/2020/05/xss-low-sec.jpeg)

Select **Reflected XSS** challenge on the left button.

Input `**hackme**` [unique string ] in the input section and submit it as shown below.

![reflection-check-low](https://ethicalhacs.com/media/2020/05/xss-low-check.jpeg)

Now, press **`CTRL+U`** to view the **page source and find the** unique string which we have entered. Our unique string is present in the page source as shown below so this may be vulnerable to XSS attack.

![reflection in source code](https://ethicalhacs.com/media/2020/05/xss-low-source.jpeg)

Now enter the payload `**<script> alert() </script**`**`>`** in the same field and submit the request.

![Reflected-xss-exploit-poc](https://ethicalhacs.com/media/2020/05/xss-low-exploit.jpeg)

You can see a popup box below which confirms that it is vulnerable to **reflected XSS** and we have successfully exploited it at low-level security.

![reflected-xss-alert in low security](https://ethicalhacs.com/media/2020/05/xss-alert-1.jpeg)

Reflected

You can replace `**alert(“Hacked”)**`  function with **`alert(document.cookie)`** in the above payload to get the cookie of the logged-in user on the victim browser, as can be shown below. Moreover, this cookie can be used to login into the same web app from another web browser which is called [**Session Hijacking**](https://owasp.org/www-community/attacks/Session_hijacking_attack) attack.

![reflected-xss-cookie-alert-low-level](https://ethicalhacs.com/media/2020/05/xss-cookie-alert.jpeg)

Let us analyze the **source code** of low level and find how the application is accepting user input.

![reflected-xxs-low-level-source- code](https://ethicalhacs.com/media/2020/05/xss-source-low.jpeg)

**Line 1:**  Contains the function that will accept input from the user [`$_GET` ]

**Line 2:**  Reflect back the input provided by the user exactly in the same way as it was entered in the input field inside **`<pre>..</pre>`**  tag without performing any sanitization or encoding.

**Explanation:** As we can see that the input string is **echoed** back exactly in the same way as it was entered by the attacker [us], as a result, the **`<script>`** tag in our payload also gets reflected back without any filtering or any encoding. As we know, that web browser has the capability to execute any tag which is passed to it, that’s why we got an **alert** box on the screen after the execution of the **`alert()`** function inside the **`<script>`** tag. The **`<script>...</script>`** tag tells the browser that the code present inside it is JavaScript.

## **Medium Level**

To exploit reflected XSS at the security level medium change the security level to **`medium`** from **`DVWA Security`** button as shown below.

![dvwa-medium-security-set](https://ethicalhacs.com/media/2020/05/xss-medium-sec.jpeg)

Choose the challenge **`XSS Reflected`** from the left pane.

We will follow the same general steps which we have mentioned above to exploit reflected XSS. So, let us type our unique string [here, **`hackme`**] in the input field to check how our input is being treated by the application.

![dvwa-reflected-xss-medium-check](https://ethicalhacs.com/media/2020/05/xss-low-check.jpeg)

Check the source code by pressing `**CTRL+U**` and search for the unique string. In our **source code** below, we found the unique string is reflecting. So, it may be vulnerable to reflected XSS.

![reflected-xss-medium-source-code](https://ethicalhacs.com/media/2020/05/reflect-2.jpeg)

Now use the same basic payload which we have used in the low-level security i.e., **`<script>alert("Hacked")</script>`**.

![medium-exploit-check](https://ethicalhacs.com/media/2020/05/xss-medium-check.jpeg)

We didn’t get the popup. This may be due to the reason that the application is **`sanitizing`** or **`encoding`** our input string. Let us analyse the **source** **code** to confirm what is actually happening in the backend.

![reflected-xss-medium-source-code](https://ethicalhacs.com/media/2020/05/xss-source-med.jpeg)

**Line 1:**  Simply contains the function to accept input from the user which is `**$_GET**`

**Line 2:** Replaces all occurrences of **`<script>`** tag in the user input by `**null**`. As a result, when we used our payload `**<script>alert("Hacked")</script>**` in the input field it became **`alert("Hacked")</script>`** and **`<script>`** tag is filtered. Also, as we know that `**<script>**` tag is a **container tag** therefore it requires both an **opening** and **closing** tag for its execution. This is the reason our payload didn’t work.

**Line3:** Reflects back the input provided by the user inside **`<pre>..</pre>`** tag after performing filtering in the above line.

Let’s try to bypass the filter. After analyzing the **source code** we found that our `**<script>**` tag is filtered.

**Bypass**: To bypass it we can use **`<Script>`** or `**<SCRipt>**` or **`<ScRiPt>`** in place of **`<script>`** in our payload**.** It will successfully bypass this filter. So our final payload becomes **`<Script> alert("Hacked") </Script>`.** Enter this payload in the input field and **Submit** the request.

![dvwa-reflected-xss-medium-security-poc](https://ethicalhacs.com/media/2020/05/xss-medium-exploit.jpeg)

You can see that we have bypassed the filter and got an alert box. In this way, we can exploit Reflected XSS vulnerability in DVWA at medium level. So our medium-level payload is **`<Script>alert("Hacked")</Script>`**.

![dvwa-medium-reflected-xss-poc](https://ethicalhacs.com/media/2020/05/xss-alert-2.jpeg)

## **High Level**

To exploit reflected XSS at **high-level** security change the security level to high from the **DVWA Security** button as shown below.

![Security set to high level](https://ethicalhacs.com/media/2020/05/xss-high-sec.jpeg)

Choose **XSS Reflected** on the left pane.

Again, input the unique string [here **`hackme`**] to confirm that it is reflecting or not.  

![reflection check in high security](https://ethicalhacs.com/media/2020/05/xss-high.jpeg)

Open the **source code** by `**CTRL+U**` and search for the string `**hackme**`**.** We will find our unique string `**hackme**` is reflecting in the page source. So this may be vulnerable to XSS attack.

Let us try our very basic payload in the input field which is, **`<script> alert("Hacked") </script>`** and check what happens.

![xss-reflected-high-check](https://ethicalhacs.com/media/2020/05/xss-medium-check.jpeg)

We didn’t get an alert box. This may be due to the reason that some type of filtering or encoding is being performed on the server side. Let’s check the source code to see what is actually happening with user input.

![dvwa-reflected-xss-high-source](https://ethicalhacs.com/media/2020/05/xss-high-source.jpeg)

From the above, it is observed that our injected **`<script>`** tag is blocked in the page source. Let us analyze the source code of the blackened to check exactly how our input is processed.

![dvwa-reflected-xss-high-source-code](https://ethicalhacs.com/media/2020/05/xss-source-high.jpeg)

**Line1:** Simply accepts input from the user.

**Line2:** Replaces all occurrences of **`<script>`** tag whether capital or small or both mixed with **null**. So, the **`<script>`** tag is useless.

**Bypass:** To bypass it we can use some other HTML tag with `**event handlers**` to print the **alert** box on the screen. Let’s try the payload **`<img src=x onerror=alert("Hacked")>`** with the event handler **`onerror`** in the input field.

![reflected-xss-high-exploit](https://ethicalhacs.com/media/2020/05/xss-high-exploit.jpeg)

We got an alert box. So we have successfully exploited Reflected XSS in DVWA at high-level security.

![reflected-xss-high-poc](https://ethicalhacs.com/media/2020/05/xss-alert-3.jpeg)

Above are the basic methods by which you can exploit reflected XSS in DVWA at low, medium, and high security.