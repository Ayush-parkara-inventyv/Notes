# Starting the challenge

## Low level — Understanding the application

We access a page asking us to submit the word “success” to win. Below the statement we find a text field and a submit button.

![](https://miro.medium.com/v2/resize:fit:544/0*i6rI1IJEjz-dBbhh.png)

We try to send the word success but we get the message `Invalid token.`. Here is the request sent:

POST /vulnerabilities/javascript/ HTTP/1.1  
Host: localhost  
User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:66.0) Gecko/20100101 Firefox/66.0  
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,***/*;q=0.8**  
Accept-Language: en-US,en;q=0.5  
Accept-Encoding: gzip, deflate  
Referer: http://localhost/vulnerabilities/javascript/  
Content-Type: application/x-www-form-urlencoded  
Content-Length: 65  
DNT: 1  
Connection: close  
Cookie: PHPSESSID=c8f9p19iv3b8s0cm58d7pm1e25; security=low  
Upgrade-Insecure-Requests: 1token=8b479aefbd90795395b3e7089ae0dc09**&phrase=success&send=Submit**

It seems that the token sent is incorrect. After trying different phrases (“success”, “test”, “ChangeMe”), we notice that the token never changes. The length of token is 32 chars, so it might be a MD5 sum:

```bash
$ echo -n "8b479aefbd90795395b3e7089ae0dc09"| wc -c  32
```

After using an online cracking service we find that the MD5 sum is the hash of `PunatrZr`. We can check it for ourselves :

```bash
echo -n "PunatrZr" | md5sum  8b479aefbd90795395b3e7089ae0dc09
```

If we try to replace the token by the MD5 sum of `success`, we still get the error Invalid token.

```bash
echo -n "success" | md5sum  260ca9dd8a4577fc00b7bd5810298076
```

So it is time to inspect the page !

# Low level — Exploiting the vulnerability

We find in the source code of the page that the javascript function `generate_token()` is called. This function gets the value of the element `phrase` and computes the md5 of the phrase coded through a Ceasar cipher (ROT13).

```javascript
function rot13(inp) {  
  return inp.replace(/[a-zA-Z]/g,function(c){return String.fromCharCode((c<="Z"?90:122)>=(c=c.charCodeAt(0)+13)?c:c-26);});  
}function generate_token() {  
  var phrase = document.getElementById("phrase").value;  
  document.getElementById("token").value = md5(rot13(phrase));  
}generate_token();
```

With this new knowledge, let’s see if `PunatrZr` is a ciphertext encoded with `ROT13` :

```shell
$ echo 'PunatrZr' | tr 'A-Za-z' 'N-ZA-Mn-za-m'  
ChangeMe
```

So the token is equal to `md5(rot13("ChangeMe"))`. To facilitate my research I added an alias that computes the rot13 of the standard input.

```bash
alias rot13="tr 'A-Za-z' 'N-ZA-Mn-za-m'"
```

Let’s compute a token for the string `success` and send it to the server.

```bash
echo -n "success" | rot13 | md5sum  
38581812b435834ebf84ebcc2c6424d6
```

We now get “Well Done !” from the server.

# Medium level

In this level, the token is always `XXeMegnahCXX`. If the phrase is not `success` we get the error You got the phrase wrong. When the phrase is `success` we get the error Invalid token. 

so if you look at the string **eMegnahC** is a reverse string of the string **ChangeMe**.
Let's look at the form :

```html
<form name="low_js" method="post">  
  <input type="hidden" name="token" value="" id="token" />  
  <label for="phrase">Phrase</label> <input type="text" name="phrase"value="ChangeMe" id="phrase" />  
  <input type="submit" id="send" name="send" value="Submit" />  
</form>  
<script src="/vulnerabilities/javascript/source/medium.js">  
</script>
```

We can see that a script is called : `/vulnerabilities/javascript/source/medium.js`. Here is the script content :

```javascript
function do_something (e) {  
  for (var t = '', n = e.length - 1; n >= 0; n--)  
    t += e[n];  
  return t;  
}  
setTimeout (function () {  
  do_elsesomething ('XX');  
}, 300);  
function do_elsesomething (e) {  
  document.getElementById ('token').value = do_something (  
    e + document.getElementById ('phrase').value + 'XX'  
  );  
}
```

From the script we understand that :

1. The function `do_elsesomething` is called after 300ms of waiting.
2. The function `do_elsesomething` sets the token value with the help of the function `do_something`

Since the default value of the phrase is `ChangeMe`, and the function `do_elsesomething` is set to execute 300ms after the page loads, this explains why the token is always the same. If we open a console and execute the command `do_elsesomething('XX')`, the token for the value `success` will be computed. 

which will be done when you enter it like this 
new value for **“sseccus”** will be **“XXsseccusXX”**

**token=XXsseccusXX&phrase=success**

Now if we submit the challenge we will get the answer Well done!.

# High level

In this level, here is the request sent when we click on the `Submit` button:

**POST /vulnerabilities/javascript/ HTTP/1.1**  
**Host: localhost**  
**User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:61.0) Gecko/20100101 Firefox/61.0**  
**Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8**  
**Accept-Language: en-US,en;q=0.5**  
**Accept-Encoding: gzip, deflate**  
**Referer: http://localhost/vulnerabilities/javascript/**  
**Content-Type: application/x-www-form-urlencoded**  
**Content-Length: 98**  
**Cookie: PHPSESSID=120fgd8orft160g1ot137588k2; security=high**  
**Connection: close**  
**Upgrade-Insecure-Requests: 1token=28638d855bc00d62b33f9643eab3e43d8335ab2b308039abd8fb8bef86331b14**&phrase=ChangeMe&send=Submit

The token is `28638d855bc00d62b33f9643eab3e43d8335ab2b308039abd8fb8bef86331b14`.

If we try with the input `success` or `test` or anything else we always get the same token. If the phrase is not success, we get the error message "You got the phrase wrong". Otherwise we get the error message "Invalid token". Let's take a look at the javascript code loaded on the page. We can see that the script creates an array that is shifted by the following function :

```javascript
(function(array_to_shift, nb_shift) {  
  var shift_function = function(nb_shift_bis) {  
    // Shift the array "a" 0x1f4 + 1 times = 501 times  
    while (--nb_shift_bis) {  
      array_to_shift["push"](array_to_shift["shift"]());  
    }  
  };  
  shift_function(++nb_shift);  
})(special_array, 0x1f4);

Right after the script defines a function to retrieve an element from the array :

// Return a[index],  
var get_element_from_a = function(index, unused_parameter) {  
  index = index - 0x0;  
  var element = special_array[index];  
  return element;  
};

Finally the script creates a new function from the array with

(function(d, e, f, g, h, i) {
```


And then uses `eval()` to launch the newly crafted function. To get the newly created function we put a breakpoint at the end of the function `function(d, e, f, g, h, i)` and we retrieve the code. We get the following code :

```javascript
function() {  
    // function that seems to define the sha256 function used later  
})();  
// Functions that help crafting the token  
function do_something(e) {  
  for (var t = "", n = e.length - 1; n >= 0; n--) t += e[n];  
  return t;  
}  
function token_part_3(t, y = "ZZ") {  
  document.getElementById("token").value = sha256(  
    document.getElementById("token").value + y  
  );  
}  
function token_part_2(e = "YY") {  
  document.getElementById("token").value = sha256(  
    e + document.getElementById("token").value  
  );  
}  
function token_part_1(a, b) {  
  document.getElementById("token").value = do_something(  
    document.getElementById("phrase").value  
  );  
}// The rest of the code defines how the token are constructed  
document.getElementById("phrase").value = "";  
setTimeout(function() {  
  token_part_2("XX");  
}, 300);  
document.getElementById("send").addEventListener("click", token_part_3);  
token_part_1("ABCD", 44);
```


From the source code we understand that the first part defines some functions that are used later on while the last part crafts the token from the different function. The problem is that the token is generated when the page is loaded with the predefined value “ChangeMe”. When the input value is modified, the token is not updated. We modify the input value to “success” and we will try to craft the token from the code logic. 

JavaScript is performing following 3 steps to generate token:

1. reverse the value of phrase:
2. phrase=success
3. token=sseccus
4. prepend ‘XX’ at start and sha256:
5. token = ‘XX’ + token = ‘XXsseccus’
6. sha256(token) = sha256(“XXsseccus”) = “7f1bfaaf829f785ba5801d5bf68c1ecaf95ce04545462c8b8f311dfc9014068a”
7. append ‘ZZ’ and sha256:
8. token = token + ‘ZZ’ = “7f1bfaaf829f785ba5801d5bf68c1ecaf95ce04545462c8b8f311dfc9014068aZZ”
9. sha256(token) = sha256(“7f1bfaaf829f785ba5801d5bf68c1ecaf95ce04545462c8b8f311dfc9014068aZZ”) = “ec7ef8687050b6fe803867ea696734c67b541dfafb286a0b1239f42ac5b0aa84”
10. token=ec7ef8687050b6fe803867ea696734c67b541dfafb286a0b1239f42ac5b0aa84&phrase=success

In our browser, we open a console to try the defined functions:

```bash
token_part_1("ABCD", 44)  
"sseccus"
```

The first function just scrambles the word “success”, we just generated the first part of the token. Let’s try the second part now :

```scss
token_part_2("XX")  
7f1bfaaf829f785ba5801d5bf68c1ecaf95ce04545462c8b8f311dfc9014068a
```

The second part generates a SHA-256 sum from “XX” concatenated with the first part. We can verify for ourselves:

```bash
echo -n "XXsseccus" | sha256sum  
7f1bfaaf829f785ba5801d5bf68c1ecaf95ce04545462c8b8f311dfc9014068a
```

Now for the last part :

```csharp
token_part_3(null, "ZZ")  
ec7ef8687050b6fe803867ea696734c67b541dfafb286a0b1239f42ac5b0aa84
```

From the code we see that the last part computes a SHA-256 sum of the previous step. We can verify this:

```bash
echo -n "7f1bfaaf829f785ba5801d5bf68c1ecaf95ce04545462c8b8f311dfc9014068aZZ" | sha256sum  
ec7ef8687050b6fe803867ea696734c67b541dfafb286a0b1239f42ac5b0aa84
```

Now if we click Submit we will get the message `Well done!`.




