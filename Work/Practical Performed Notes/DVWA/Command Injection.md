First of all what is **Command Injection**:
**Command Execution or Command injection** is an attack in which the goal is **execution** of arbitrary **commands** on the host operating system via a vulnerable application. **Command injection** attacks are possible when an application passes unsafe user supplied data (forms, cookies, HTTP headers etc.) to a system shell.

I hope you know how to change the security level as well as how to see the source code on the page itself.
# Low
Now check the source code for low and see what is the vulnerability here:

![](https://miro.medium.com/v2/resize:fit:678/1*XhtD8kutaGUD6266NUnPfA.png)

we can see that the code does not check if $target matches an IP Address. No filtering on special characters. `;` in Unix/Linux allows for commands to be separated.

so lets try an ip with command following it and a seperator like **;**
```
127.0.0.1; ls -la /root
```
 This will list all the files in the root directory :

![](https://miro.medium.com/v2/resize:fit:819/1*r6FZIkQQ3PmKgX8JxSDHtw.png)

we can also run various commands to verify that this works for others as well.

```
127.0.0.1 ; cat /etc/passwd 
```
Displays the contents of **/etc/passwd** on the webpage.

Alternatives to **;** is as below:
**&&** - AND Operator  
**|** PIPE Operator
# Medium

Now check the source code for medium and see what is the vulnerability here:

![](https://miro.medium.com/v2/resize:fit:875/1*_zkGeSm1utGcR6AsccPieg.png)

we can see that a blacklist has been set to exclude `&&` and `;`. As noted above, we can use `|` as a replacement:  
`127.0.0.1| cat /etc/passwd`. Double `||` can also be used,

![](https://miro.medium.com/v2/resize:fit:819/1*iHneg3iZCsASTEmDo_Tkfg.png)
 and see it worked perfectly fine without any issues.
# High

Viewing source code, more extensive blacklist has been set. Slightly trickier, however the answer is in the view source ,  
**'|  '**
note that there is a space after the **|** character. If we try **| pwd**, no output is returned, however if we use **|pwd** we are including our command within this space, as shown below:

![](https://miro.medium.com/v2/resize:fit:816/1*iJ7a0s4KfmzuVA1pjGuTrg.png)

also lets try other commands to verify it.
```
127.0.0.1|ls ../../../*
```
![](https://i.imgur.com/J1orwRP.png)
# Points to note:

1. Ensure you are using commands specific to the target you are trying to attack, all of the above are Linux, Windows commands will be different.
2. Try commands with and without a space between them
3. You will not always have access to the source code.

Happy hacking,****