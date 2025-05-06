ssh to bandit24 from the password in [[Bandit23]]
According to the hint we get that a service is on on the 30002 in which daemon is there sitting checking our password and the pin that we provide
so lets check the service first
**ss -tuln | grep 30002**
and the output was this
then we checked doing **nc**  to the port
**nc localhost 30002**
input string format: **gb8KRRCsshuZXI0tUuR6ypOFjiZbf3G8 1234** where the 1234 is the pin and we have to bruteforce it
![[Pasted image 20250106123742.png]]
so it is for sure from the hint itself that we need a script to run and as the **home/bandit24** wont allow us to make a file so lets check all the directories that can allow +rwx for our file by default
**find / -type d -perm 777**
and the output was this in which we got few directories
![[Pasted image 20250106124000.png]]
i used this directory and then used **chatgpt** to make a script for the same
i am not going to give you the prompt also okayy!!!!
The script looks somewhat like this![[Pasted image 20250106124311.png]]
if possible you will find this script there only so u can use that directly also idm
okay so as the script is ready lets start the procedure brute forcing to the daemon
command: **python3 bruteforce.py**
![[Pasted image 20250106124426.png]]
an example of how the bruteforcing was done and the final password for that
![[Pasted image 20250106124501.png]]
password:**iCi86ttT4KSNe1armKiwbQNmB3YJP3q4**
