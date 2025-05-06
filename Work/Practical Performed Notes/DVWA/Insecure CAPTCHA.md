Damn Vulnerable Web Application (DVWA): Testing CAPTCHA Vulnerabilities

When entering a website, we have probably experienced filling combinations of letters and numbers or selecting which images contain traffic lights or chimneys a thousand times. These whole verification steps are called Completely Automated Public Turing test to tell Computers and Humans Apart or simply **CAPTCHA**.

# CAPTCHA

 **_CAPTCHA is a type of challenge–response test used in computing to determine whether the user is human or not [_(Wikipedia)_](https://en.wikipedia.org/wiki/CAPTCHA)**

That means, in today’s world, it is clearly possible that computer users are, in fact, machines, which can act maliciously, such as performing a Distributed Denial of Service (DDoS) attack, or mimicking most human actions like scrolling, clicking, filling forms, or even brute forcing passwords and sending lot of automated requests to overwhelm a website with great traffic.

CAPTCHA is a result from trained model from a huge amount of data of some of our most common activities, even the way we move our mouse on the computer screen, especially for the “I’m not a robot” CAPTCHA. Robots tend to work automatically via a program where they click particular buttons without mouse movement, unlike real human do.

![](https://miro.medium.com/v2/resize:fit:709/0*K5uTz2_mo2uMSY96.png)

Source: [Glints](https://www.google.com/url?sa=i&url=https%3A%2F%2Fglints.com%2Fid%2Flowongan%2Fcaptcha-adalah%2Fsoluna%2F&psig=AOvVaw1c7Jz6U77Tv7rIZzn2wkHc&ust=1632748978787000&cd=vfe&ved=0CA0Q3YkBahcKEwjwi6iJ3pzzAhUAAAAAHQAAAAAQDQ)

With all the sophistication features of this particular technology, it turns out that this technology is still unsafe, and those vulnerabilities will be simulated by using [**DVWA (Damn Vunerable Web Application)**](http://dvwa.co.uk/).

# DVWA

**_Damn Vulnerable Web Application (DVWA) is a PHP/MySQL web application that is damn vulnerable, whose main goal is to be an aid for security professionals to test their skills and tools in a legal environment, help web developers better understand the processes of securing web applications, and to aid both students and teachers to learn about web application security in a controlled class room environment _(DVWA)_**

![](https://miro.medium.com/v2/resize:fit:365/0*QozH5vIKcm98eLdQ.png)

With DVWA, we can simulate and practice some of the most common web vulnerabilities with various levels of difficulty, namely **low, medium, high, and impossible.** In this case, we will mainly focus in **insecure CAPTCHA vulnerabilities**.

![](https://miro.medium.com/v2/resize:fit:513/1*Si7zAJhlCEF2BxHBaT3eQg.png)

The test will be a step for changing our DVWA password and we have to go through the verification process using CAPTCHA.

## Low Security

As said in the DVWA documentation,

> The issue with this CAPTCHA is that it is easily bypassed. The developer has made the assumption that all users will progress through screen 1, complete the CAPTCHA, and then move on to the next screen where the password is actually updated. By submitting the new password directly to the change page, the user may bypass the CAPTCHA system.

The source code for low security case:
```php
<?phpif( isset( $_POST[ 'Change' ] ) && ( **$_POST[ 'step' ] == '1'** ) ) {  
 // Hide the CAPTCHA form  
 $hide_form = true;// Get input  
 $pass_new  = $_POST[ 'password_new' ];  
 $pass_conf = $_POST[ 'password_conf' ];// Check CAPTCHA from 3rd party  
 $resp = recaptcha_check_answer(  
  $_DVWA[ 'recaptcha_private_key'],  
  $_POST['g-recaptcha-response']  
 );// Did the CAPTCHA fail?  
 if( !$resp ) {  
  // What happens when the CAPTCHA was entered incorrectly  
  $html     .= "<pre><br />The CAPTCHA was incorrect. Please try again.</pre>";  
  $hide_form = false;  
  return;  
 }  
 else {  
  // CAPTCHA was correct. Do both new passwords match?  
  if( $pass_new == $pass_conf ) {  
   // Show next stage for the user  
   $html .= "  
    <pre><br />You passed the CAPTCHA! Click the button to confirm your changes.<br /></pre>  
    <form action=\"#\" method=\"POST\">  
     <input type=\"hidden\" name=\"step\" value=\"2\" />  
     <input type=\"hidden\" name=\"password_new\" value=\"{$pass_new}\" />  
     <input type=\"hidden\" name=\"password_conf\" value=\"{$pass_conf}\" />  
     <input type=\"submit\" name=\"Change\" value=\"Change\" />  
    </form>";  
  }  
  else {  
   // Both new passwords do not match.  
   $html     .= "<pre>Both passwords must match.</pre>";  
   $hide_form = false;  
  }  
 }  
}if( isset( $_POST[ 'Change' ] ) && ( **$_POST[ 'step' ] == '2'** ) ) {  
 // Hide the CAPTCHA form  
 $hide_form = true;// Get input  
 $pass_new  = $_POST[ 'password_new' ];  
 $pass_conf = $_POST[ 'password_conf' ];// Check to see if both password match  
 if( $pass_new == $pass_conf ) {  
  // They do!  
  $pass_new = ((isset($GLOBALS["___mysqli_ston"]) && is_object($GLOBALS["___mysqli_ston"])) ? mysqli_real_escape_string($GLOBALS["___mysqli_ston"],  $pass_new ) : ((trigger_error("[MySQLConverterToo] Fix the mysql_escape_string() call! This code does not work.", E_USER_ERROR)) ? "" : ""));  
  $pass_new = md5( $pass_new );// Update database  
  $insert = "UPDATE `users` SET password = '$pass_new' WHERE user = '" . dvwaCurrentUser() . "';";  
  $result = mysqli_query($GLOBALS["___mysqli_ston"],  $insert ) or die( '<pre>' . ((is_object($GLOBALS["___mysqli_ston"])) ? mysqli_error($GLOBALS["___mysqli_ston"]) : (($___mysqli_res = mysqli_connect_error()) ? $___mysqli_res : false)) . '</pre>' );// Feedback for the end user  
  $html .= "<pre>Password Changed.</pre>";  
 }  
 else {  
  // Issue with the passwords matching  
  $html .= "<pre>Passwords did not match.</pre>";  
  $hide_form = false;  
 }((is_null($___mysqli_res = mysqli_close($GLOBALS["___mysqli_ston"]))) ? false : $___mysqli_res);  
}?>
```

It basically divide the process into two steps, the first is using reCAPTCHA to check whether the users fill the correct credentials and pass the CAPTCHA.

![](https://miro.medium.com/v2/resize:fit:511/1*vu53vX_Y83syi6XUOH46Lw.png)

If they do, they will be required to submit the change button and then the server finishes the operation.

![](https://miro.medium.com/v2/resize:fit:750/1*whdhI_fXcvApNXnM0NVESg.png)

![](https://miro.medium.com/v2/resize:fit:225/1*xU2UPIED7EzeyO0orArq5A.png)

This level can be easily manipulated as the server only checks the “Change” button and “step” parameter, so we can easily change the step parameter value from 1 to 2 so that after submitting the password, users can go straight to the second page without having to pass the CAPTCHA system.

![](https://miro.medium.com/v2/resize:fit:710/1*qrPGe9826kNWMMLgOQg5Rw.png)

## Medium Security

As said in the DVWA documentation,

> The developer has attempted to place state around the session and keep track of whether the user successfully completed the CAPTCHA prior to submitting data. Because the state variable passed_captcha is on the client side, it can also be manipulated by the attacker by modifying the value to “True” or 1.

The source code for medium security case:

```php
<?phpif( isset( $_POST[ 'Change' ] ) && ( $_POST[ 'step' ] == '1' ) ) {  
 // Hide the CAPTCHA form  
 $hide_form = true;// Get input  
 $pass_new  = $_POST[ 'password_new' ];  
 $pass_conf = $_POST[ 'password_conf' ];// Check CAPTCHA from 3rd party  
 $resp = recaptcha_check_answer(  
  $_DVWA[ 'recaptcha_private_key' ],  
  $_POST['g-recaptcha-response']  
 );// Did the CAPTCHA fail?  
 if( !$resp ) {  
  // What happens when the CAPTCHA was entered incorrectly  
  $html     .= "<pre><br />The CAPTCHA was incorrect. Please try again.</pre>";  
  $hide_form = false;  
  return;  
 }  
 else {  
  // CAPTCHA was correct. Do both new passwords match?  
  if( $pass_new == $pass_conf ) {  
   // Show next stage for the user  
   $html .= "  
    <pre><br />You passed the CAPTCHA! Click the button to confirm your changes.<br /></pre>  
    <form action=\"#\" method=\"POST\">  
     <input type=\"hidden\" name=\"step\" value=\"2\" />  
     <input type=\"hidden\" name=\"password_new\" value=\"{$pass_new}\" />  
     <input type=\"hidden\" name=\"password_conf\" value=\"{$pass_conf}\" />  
     **<input type=\"hidden\" name=\"passed_captcha\" value=\"true\" />**  
     <input type=\"submit\" name=\"Change\" value=\"Change\" />  
    </form>";  
  }  
  else {  
   // Both new passwords do not match.  
   $html     .= "<pre>Both passwords must match.</pre>";  
   $hide_form = false;  
  }  
 }  
}if( isset( $_POST[ 'Change' ] ) && ( $_POST[ 'step' ] == '2' ) ) {  
 // Hide the CAPTCHA form  
 $hide_form = true;// Get input  
 $pass_new  = $_POST[ 'password_new' ];  
 $pass_conf = $_POST[ 'password_conf' ];// Check to see if they did stage 1  
 if( !$_POST[ 'passed_captcha' ] ) {  
  $html     .= "<pre><br />You have not passed the CAPTCHA.</pre>";  
  $hide_form = false;  
  return;  
 }// Check to see if both password match  
 if( $pass_new == $pass_conf ) {  
  // They do!  
  $pass_new = ((isset($GLOBALS["___mysqli_ston"]) && is_object($GLOBALS["___mysqli_ston"])) ? mysqli_real_escape_string($GLOBALS["___mysqli_ston"],  $pass_new ) : ((trigger_error("[MySQLConverterToo] Fix the mysql_escape_string() call! This code does not work.", E_USER_ERROR)) ? "" : ""));  
  $pass_new = md5( $pass_new );// Update database  
  $insert = "UPDATE `users` SET password = '$pass_new' WHERE user = '" . dvwaCurrentUser() . "';";  
  $result = mysqli_query($GLOBALS["___mysqli_ston"],  $insert ) or die( '<pre>' . ((is_object($GLOBALS["___mysqli_ston"])) ? mysqli_error($GLOBALS["___mysqli_ston"]) : (($___mysqli_res = mysqli_connect_error()) ? $___mysqli_res : false)) . '</pre>' );// Feedback for the end user  
  $html .= "<pre>Password Changed.</pre>";  
 }  
 else {  
  // Issue with the passwords matching  
  $html .= "<pre>Passwords did not match.</pre>";  
  $hide_form = false;  
 }((is_null($___mysqli_res = mysqli_close($GLOBALS["___mysqli_ston"]))) ? false : $___mysqli_res);  
}?>
```

In this security level, when we modify the step value from 1 to 2 only, the process will fail. But the flaw is that the developer rely on the **passed_captcha** parameter to tell whether the users have passed the CAPTCHA or not, and because it is on the client side, it can be easily manipulated by setting the step value to 2 and adding the passed_captcha and assign “True” as the value.

![](https://miro.medium.com/v2/resize:fit:654/1*XpCLriHwny7qETeQzIpHhA.png)

## High Security

As said in the DVWA documentation,

> There has been development code left in, which was never removed in production. It is possible to mimic the development values, to allow invalid values in be placed into the CAPTCHA field.
> 
> You will need to spoof your user-agent **reCAPTCHA** as well as use the CAPTCHA value of **hidd3n_valu3** to skip the check.

The source code for high security case:

```php
<?phpif( isset( $_POST[ 'Change' ] ) ) {  
 // Hide the CAPTCHA form  
 $hide_form = true;// Get input  
 $pass_new  = $_POST[ 'password_new' ];  
 $pass_conf = $_POST[ 'password_conf' ];// Check CAPTCHA from 3rd party  
 $resp = recaptcha_check_answer(  
  $_DVWA[ 'recaptcha_private_key' ],  
  $_POST['g-recaptcha-response']  
 );if (  
  $resp ||   
  (  
   **$_POST[ 'g-recaptcha-response' ] == 'hidd3n_valu3'  
   && $_SERVER[ 'HTTP_USER_AGENT' ] == 'reCAPTCHA'**  
  )  
 ){  
  // CAPTCHA was correct. Do both new passwords match?  
  if ($pass_new == $pass_conf) {  
   $pass_new = ((isset($GLOBALS["___mysqli_ston"]) && is_object($GLOBALS["___mysqli_ston"])) ? mysqli_real_escape_string($GLOBALS["___mysqli_ston"],  $pass_new ) : ((trigger_error("[MySQLConverterToo] Fix the mysql_escape_string() call! This code does not work.", E_USER_ERROR)) ? "" : ""));  
   $pass_new = md5( $pass_new );// Update database  
   $insert = "UPDATE `users` SET password = '$pass_new' WHERE user = '" . dvwaCurrentUser() . "' LIMIT 1;";  
   $result = mysqli_query($GLOBALS["___mysqli_ston"],  $insert ) or die( '<pre>' . ((is_object($GLOBALS["___mysqli_ston"])) ? mysqli_error($GLOBALS["___mysqli_ston"]) : (($___mysqli_res = mysqli_connect_error()) ? $___mysqli_res : false)) . '</pre>' );// Feedback for user  
   $html .= "<pre>Password Changed.</pre>";} else {  
   // Ops. Password mismatch  
   $html     .= "<pre>Both passwords must match.</pre>";  
   $hide_form = false;  
  }} else {  
  // What happens when the CAPTCHA was entered incorrectly  
  $html     .= "<pre><br />The CAPTCHA was incorrect. Please try again.</pre>";  
  $hide_form = false;  
  return;  
 }((is_null($___mysqli_res = mysqli_close($GLOBALS["___mysqli_ston"]))) ? false : $___mysqli_res);  
}// Generate Anti-CSRF token  
generateSessionToken();?>
```

In this level, there are some modifications by the developer, this time focusing on the resp parameter containing the **user agent** and **g-recaptcha-response**. If the user agent parameter or HTTP header is not equal to reCAPTCHA, the system would consider the verification fails hence would not pass the CAPTCHA. Also, the g-recaptcha-response parameter value must equal to hidd3n_valu3 as stated on the code.

![](https://miro.medium.com/v2/resize:fit:875/1*YzBlulye3Dx_cdoj1ykLBg.png)

## Impossible Security as Mitigation

As said in the DVWA documentation,

> In the impossible level, the developer has removed all avenues of attack. The process has been simplified so that data and CAPTCHA verification occurs in one single step. Alternatively, the developer could have moved the state variable server side (from the medium level), so the user cannot alter it.

The source code for impossible security case:

```php
<?phpif( isset( $_POST[ 'Change' ] ) ) {  
 // Check Anti-CSRF token  
 checkToken( $_REQUEST[ 'user_token' ], $_SESSION[ 'session_token' ], 'index.php' );// Hide the CAPTCHA form  
 $hide_form = true;// Get input  
 $pass_new  = $_POST[ 'password_new' ];  
 $pass_new  = stripslashes( $pass_new );  
 $pass_new  = ((isset($GLOBALS["___mysqli_ston"]) && is_object($GLOBALS["___mysqli_ston"])) ? mysqli_real_escape_string($GLOBALS["___mysqli_ston"],  $pass_new ) : ((trigger_error("[MySQLConverterToo] Fix the mysql_escape_string() call! This code does not work.", E_USER_ERROR)) ? "" : ""));  
 $pass_new  = md5( $pass_new );$pass_conf = $_POST[ 'password_conf' ];  
 $pass_conf = stripslashes( $pass_conf );  
 $pass_conf = ((isset($GLOBALS["___mysqli_ston"]) && is_object($GLOBALS["___mysqli_ston"])) ? mysqli_real_escape_string($GLOBALS["___mysqli_ston"],  $pass_conf ) : ((trigger_error("[MySQLConverterToo] Fix the mysql_escape_string() call! This code does not work.", E_USER_ERROR)) ? "" : ""));  
 $pass_conf = md5( $pass_conf );$pass_curr = $_POST[ 'password_current' ];  
 $pass_curr = stripslashes( $pass_curr );  
 $pass_curr = ((isset($GLOBALS["___mysqli_ston"]) && is_object($GLOBALS["___mysqli_ston"])) ? mysqli_real_escape_string($GLOBALS["___mysqli_ston"],  $pass_curr ) : ((trigger_error("[MySQLConverterToo] Fix the mysql_escape_string() call! This code does not work.", E_USER_ERROR)) ? "" : ""));  
 $pass_curr = md5( $pass_curr );// Check CAPTCHA from 3rd party  
 $resp = recaptcha_check_answer(  
  $_DVWA[ 'recaptcha_private_key' ],  
  $_POST['g-recaptcha-response']  
 );// Did the CAPTCHA fail?  
 if( !$resp ) {  
  // What happens when the CAPTCHA was entered incorrectly  
  $html .= "<pre><br />The CAPTCHA was incorrect. Please try again.</pre>";  
  $hide_form = false;  
 }  
 else {  
  // Check that the current password is correct  
  $data = $db->prepare( 'SELECT password FROM users WHERE user = (:user) AND password = (:password) LIMIT 1;' );  
  $data->bindParam( ':user', dvwaCurrentUser(), PDO::PARAM_STR );  
  $data->bindParam( ':password', $pass_curr, PDO::PARAM_STR );  
  $data->execute();// Do both new password match and was the current password correct?  
  if( ( $pass_new == $pass_conf) && ( $data->rowCount() == 1 ) ) {  
   // Update the database  
   $data = $db->prepare( 'UPDATE users SET password = (:password) WHERE user = (:user);' );  
   $data->bindParam( ':password', $pass_new, PDO::PARAM_STR );  
   $data->bindParam( ':user', dvwaCurrentUser(), PDO::PARAM_STR );  
   $data->execute();// Feedback for the end user - success!  
   $html .= "<pre>Password Changed.</pre>";  
  }  
  else {  
   // Feedback for the end user - failed!  
   $html .= "<pre>Either your current password is incorrect or the new passwords did not match.<br />Please try again.</pre>";  
   $hide_form = false;  
  }  
 }  
}// Generate Anti-CSRF token  
generateSessionToken();?>
```

In this level, it is almost impossible to manipulate the system, as the whole process can only be done in one single step. The state variable has also been moved from the client side so it can’t be modified. With all the improvements and modifications regarding the level, I would say might be the closest representation to a secure CAPTCHA verification process.

![](https://miro.medium.com/v2/resize:fit:518/1*WnF9zQPsoPxLbf9sENPMWw.png)

Although the explanation of this blog is still very brief and shallow, I hope we all can learn about the proper use of ethical web hacking using DVWA, especially on the insecure CAPTCHA case.