# What is Content-Security-Policy?

Content-Security-Policy is the name of a HTTP response header that modern browsers use to enhance the security of the document (or web page). The Content-Security-Policy header allows you to restrict how resources such as JavaScript, CSS, or pretty much anything that the browser loads.

Although it is primarily used as a HTTP response header, you can also apply it via a [meta tag](https://content-security-policy.com/examples/meta/).

The term Content Security Policy is often abbreviated as CSP.

# What types of attacks does Content-Security-Policy help mitigate?

CSP was first designed to reduce the attack surface of Cross Site Scripting (XSS) attacks, later versions of the spec also protect against other forms of attack such as Click Jacking.

# Exploiting the Low level

We will be welcomed by the following message and a text input:

You can include scripts from external sources, examine the Content Security Policy and enter a URL to include here:

![](https://miro.medium.com/v2/resize:fit:845/0*bHvNiwJxgwJtptlV.png)

We can enter any random text and click on include and as we are using burp suit we can see the CSP rules in the response headers.

![](https://miro.medium.com/v2/resize:fit:875/1*_y-Jv3gMcUvc9aybXrQVpA.png)

Burp suite

Here we see that scripts can be loaded from the following sites :

- [**https://pastebin.com**](https://pastebin.com/)
- example.com
- code.jquery.com
- [**https://ssl.google-analytics.com**](https://ssl.google-analytics.com/) ;

The vulnerability here is that pastebin is a site that lets us create our own content. We can go on hastebin.com and click on new and paste the following script :

alert(“”hacked”);

![](https://miro.medium.com/v2/resize:fit:875/1*j372GCKQzeOFM1JZvCmmKw.png)

hastebin website

Once it’s created,In the url we get an “oracutubuw.less” for it. We can access to the raw paste by appending `/raw/` before the “oracutubuw.less”.

![](https://miro.medium.com/v2/resize:fit:858/1*sGx8gGycxGEvVUdKd99cyQ.png)

We get the link to the `raw` paste and put it in our input. When we click Include the page reloads, downloads our script from hastebin and executes it and we can see a pop-up with the text “hacked”.

![](https://miro.medium.com/v2/resize:fit:584/1*o9uVk6mnu2EOShiupxio7g.png)

# Low level — Vulnerable code

```
```<?php**$headerCSP = "Content-Security-Policy: script-src 'self' https://pastebin.com  example.com code.jquery.com https://ssl.google-analytics.com ;"; // allows js from self, pastebin.com, jquery and google analytics.header($headerCSP);## https://pastebin.com/raw/R570EE00**?>**  
**<?php**  
if (isset ($_POST['include'])) {  
$page[ 'body' ] .= "  
    <script src='" . $_POST['include'] . "'></script>  
";  
}  
$page[ 'body' ] .= '  
<form name="csp" method="POST">  
    <p>You can include scripts from external sources, examine the Content Security Policy and enter a URL to include here:</p>  
    <input size="50" type="text" name="include" value="" id="include" />  
    <input type="submit" value="Include" />  
</form>  
';
```
Scripts from third party websites should not be accepted unless it is necessary.

# Medium level

A simple XSS like below wont work...
```js
<script>alert(1)</script>
```

```html
<h1>Vulnerability: Content Security Policy (CSP) Bypass</h1>  
<div class="vulnerable_code_area">  
<script>alert(1)</script>
```

It might be because we are in the div with the class vulnerable_code_area ? If we try to escape from it with `</div><script>alert(1)</script><div>`, it doesn't work either:

```html
<h1>Vulnerability: Content Security Policy (CSP) Bypass</h1><div class="vulnerable_code_area">  
</div><script>alert(1)</script><div>
```

Let’s take a look at the Content-Security-Policy from the response headers. The CSP is the following :

``` javascript
script-src 'self' 'unsafe-inline' 'nonce-TmV2ZXIgZ29pbmcgdG8gZ2l2ZSB5b3UgdXA=';
```

Here is the official documentation for each of these parameters.

CSP parameterDescriptionselfAllows loading resources from the same origin (same scheme, host and port).self-inlineAllows use of inline source elements such as style attribute, onclick, or script tag bodies (depends on the context of the source it is applied to) and javascript: URIsnonceAllows script or style tag to execute if the nonce attribute value matches the header value. For example: 
```js
<script nonce="2726c7f26c">alert("hello");</script>
```

Apparently for our script tag to work we need to add the tag `nonce` with the correct value. The thing is... the nonce never changes ! So it is extra easy to do it, we just paste the following code in the input and sure enough we get our pop-up:
``` javascript
<script nonce="TmV2ZXIgZ29pbmcgdG8gZ2l2ZSB5b3UgdXA=">alert("hacked");</script>
```

# High level

**_The page makes a call to ../..//vulnerabilities/csp/source/jsonp.php to load some code. Modify that page to run your own code. 1+2+3+4+5=_**

When we click **Solve the sum**, we get :

**_The page makes a call to ../..//vulnerabilities/csp/source/jsonp.php to load some code. Modify that page to run your own code. 1+2+3+4+5=15_**

When we take a look at the event triggered upon clicking the button, here is the code we find:

```
function clickButton() {  
    var s = document.createElement("script");  
    s.src = "source/jsonp.php?callback=solveSum";  
    document.body.appendChild(s);  
}
```

When we click on the button, a script tag is created. The source of the script is set to the file `jsonp.php`. When we click the button, the form is sent with the callback function within its parameter `callback` :

```
GET /vulnerabilities/csp/source/jsonp.php?callback=solveSum HTTP/1.1  
Host: localhost  
User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:61.0) Gecko/20100101 Firefox/61.0  
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,***/*;q=0.8**  
Accept-Language: en-US,en;q=0.5  
Accept-Encoding: gzip, deflate  
Cookie: PHPSESSID=120fgd8orft160g1ot137588k2; security=high  
Connection: close  
Pragma: no-cache  
Cache-Control: no-cache
```

If we intercept the request and change the callback function from `solveSum` to `alert("hacked")//`, we manage to create a pop up despite the Content Security Policy.