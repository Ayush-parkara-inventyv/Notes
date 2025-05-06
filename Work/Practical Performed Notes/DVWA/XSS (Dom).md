# DVWA 1.9+: XSS DOM

![](https://miro.medium.com/v2/resize:fit:875/0*HMKOcvCtkzpYLYrY.jpg)

XSS stands for Cross-Site Scripting. It’s a type of attack in which scripts are injected into trusted web sites. [OWASP has a information about these attacks and its variations](https://www.owasp.org/index.php/Cross-site_Scripting_\(XSS\))

In this article we’ll see how an attacker can discover a XSS DOM vulnerability and take advantage. Let’s get down to work. Fire up DVWA and Kali, open your browser and point to your DVWA address.

# Low Security

Set your security to low and then open the XSS (DOM) page:

![](https://miro.medium.com/v2/resize:fit:875/1*OgEJ80twKvrIEtuYR2DKrg.png)

Pressing the “Select” button and watch the URL. It took the param “default=English”. Changing the select box sets the param default. What happens when we set the default param manually to something else like “test”?

![](https://miro.medium.com/v2/resize:fit:875/1*TyVirNYXXPkpWpE64f2wuw.png)

Now let’s insert our own Javascript to the param, something simple, like an alert “<script>alert(“Houston, we have a problem!!”)</script>”:

![](https://miro.medium.com/v2/resize:fit:875/1*yqS2bJtUPh5eZNL0zK9EOg.png)

We have determine that the site is vulnerable to XSS. Let’s get the cookie with this script “<script>alert(document.cookie)</script>”:

![](https://miro.medium.com/v2/resize:fit:875/1*mGXGpVwDiy3iFS3uzgePnw.png)

# Medium

Visually there’s no difference between the Low and the Medium security. Changing the param in the URL to test renders text in the select box, like before:

![](https://miro.medium.com/v2/resize:fit:875/1*wWkFlixQqcJREtMdo25a4g.png)

Let’s try an alert script… and nothing happens. Seems there’s some kind of filtering going on. We’re not out of options still. Open the Developer Tools and take a look at the html:

![](https://miro.medium.com/v2/resize:fit:875/1*SViUYfWWfyFK5CaFgRqpHQ.png)

We can try several values for our default param and see how the page reacts. We’re looking to break the page’s logic and insert a crafted tag. After some trial and error I’ve managed to insert the following value “<</select><img src=’#’ onclick=alert’Gotcha!!!’>”:

![](https://miro.medium.com/v2/resize:fit:875/1*0xpX4rvzfEEceSkwVTb8MQ.png)

Now change it to get the cookie:

![](https://miro.medium.com/v2/resize:fit:875/1*GwEIiFzFtw336UnOH5QLuA.png)

# High

If it’s not for tag at the bottom of the page I’d have no clue as to the security level. Ok, let’s try to hack the high security level. Start by “test”… Nothing. Seems every try gets filtered out by the server.

It’s time to try something different using the a new param/value (more about [params with GET and POST methods here](https://www.w3schools.com/tags/ref_httpmethods.asp)) “default=English&test”:

![](https://miro.medium.com/v2/resize:fit:875/1*XI6rSNIIaBwdXYptBEynRg.png)

The custom URL seems to circumvent the filter. Let’s obtain the cookie with “default=English&<script>alert(document.cookie)</script>”:

![](https://miro.medium.com/v2/resize:fit:875/1*nXNy5mv8mYXQrfpLjf6fQg.png)

You can read more [about this type of attack here](https://www.owasp.org/index.php/Testing_for_HTTP_Parameter_pollution_\(OTG-INPVAL-004\)).

# Conclusion

This is it, we’ve cracked the DVWA’s three levels of security and obtained the cookie. In the next articles we’ll get to try other types of XSS.
