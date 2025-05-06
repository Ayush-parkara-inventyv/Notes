Task 2: Burp Suite Login Analysis & PoC

Objective: To demonstrate proficiency in intercepting, manipulating, and testing login requests using Burp Suite's Proxy, Repeater, and Intruder tools, and provide proof of concept for findings.

Target Application: http://testphp.vulnweb.com/


Instructions:

Perform the following actions using Burp Suite:

1.  Intercept Login Request:
     Configure your browser to proxy through Burp Suite.
     Navigate to the login page of the target application (e.g., /login.php or /userinfo.php).
     Enter test credentials (e.g., uname=test, pass=test) and attempt to log in.
     In Burp Suite's Proxy > HTTP History, locate and identify the POST request containing these login credentials.

2.  Manual Password Testing (Repeater):
     Send the captured login POST request from the Proxy history to Repeater.
     In Repeater, keep the username fixed (e.g., test).
     Manually modify the password parameter value and send requests to try 2-3 different password combinations. Observe the responses carefully to see if they differ, which might indicate success or failure.

3.  Password Brute-Force Setup (Intruder):
     Send the same original login POST request to Intruder.
     Configure an Intruder attack (e.g., Sniper type):
         Set the payload position only on the password parameter. Leave the username fixed.
         Select a small, simple password list (e.g., Burp's built-in "Passwords" -> "Simple list" or create a list with 5-10 common passwords like test, password, 12345).
         For this task, correctly configuring the Intruder attack serves as the Proof of Concept (PoC) that you know how to set up a brute-force attempt. You do not need to run the full attack, but ensure the setup is correct.


Provide clear screenshots as Proof of Concept for each step:

 Screenshot 1: The captured login POST request (showing parameters like uname and pass) in the Proxy > HTTP history or Repeater.
 Screenshot 2: Repeater showing at least one attempt with a manually modified password and the resulting server response. (This demonstrates manual testing).
 Screenshot 3: Intruder showing the request with the password parameter correctly marked as the payload position.
 Screenshot 4: Intruder showing the Payloads tab with your chosen password list configuration targeting the password parameter. (This demonstrates correct brute-force setup).