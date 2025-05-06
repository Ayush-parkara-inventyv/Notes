Tor, short for The Onion Router, is an open-source privacy network designed for anonymous web browsing.

Tor routes internet traffic through a series of volunteer-operated servers, called relays, to conceal a user's location and usage from anyone performing network surveillance or traffic analysis.

Tor is used for various reasons, including protecting privacy and security on the Internet. It is particularly useful for whistleblowers, journalists, and others who need to communicate confidentially and protect their identity. Additionally, Tor helps individuals in regions with strict internet censorship access information freely.

Tor's layered encryption approach makes it difficult for any single point on the Internet to see both where traffic originated from and where it is ultimately going to at the same time.This conceals a user's location and usage, protecting their freedom and ability to communicate confidentially.

Right now the version of tor ongoing is version v3
what is it lemme show you

|Feature|v2 (Deprecated)|v3 (Current)|
|---|---|---|
|**Address Length**|16 characters|56 characters|
|**Algorithm**|SHA-1 (insecure) + RSA1024|SHA-3/ed25519 + curve25519 (secure)|
|**Security**|Outdated, vulnerable|Strong, future-proof cryptography|
|**Onion Address Format**|`abc123def456ghi.onion`|`abcdefghijklmnopqrstuvwxyzabcdef1234567890abcdef.onion`|
|**Directory Lookup**|Uses centralized HSDirs|Uses distributed, encrypted design|
|**Authentication Support**|Limited (basic)|Strong authentication (client-auth)|
|**Checksum and Versioning**|No|Yes (built into the address)|
|**DNS resistance**|Weak|Strong|

next step is we will be installing tor into our kali machine

### Overview: What You’ll Be Doing
1. **Install Tor on a Linux VM**
2. **Set up a basic web server** (e.g., using Apache or Python's built-in HTTP server)
3. **Configure Tor to create a v3 Onion service**
4. **Find your .onion address**
5. **Access the site using the Tor Browser**

Follow the commands below for the installation part: 
1st. apt update and install tor
```bash
sudo apt update
sudo apt install tor -y
```
2nd. start and enable tor
```bash
sudo systemctl start tor
sudo systemctl enable tor
```

next what we need to do download brave browser and for that i have used the original site downloaded the browser and then using it using the below method

1st. visit https://www.torproject.org/
2nd. download tor browser for linux machine
3rd. extract the file 
```bash
tar -xvf tor-browser-linux-x86_64-14.5.1.tar.xz
```
4th. install any remaining dependencies if required and then start the browser
5th.start the browser
```bash
┌──(kali㉿kali)-[~/Downloads/tor-browser]
└─$ chmod +x start-tor-browser.desktop 
                                                                                                                                                  
┌──(kali㉿kali)-[~/Downloads/tor-browser]
└─$ ./start-tor-browser.desktop       
Launching './Browser/start-tor-browser --detach'...
```
 next starting a service
 so for that we need to do various steps for getting the service ready and usable
 1st. make a hiddenservice folder and then create a page to get loaded when the url is visited and then python or any type of hosting to make it available or to make it public 
```bash
mkdir ~/myhiddenservice
cd ~/myhiddenservice
echo "<h1>Hello, Tor World!</h1>" > index.html
python3 -m http.server 8080
```
2nd. Then edit the torrc file for that hidden service to tell tor what do you want to call when you visit the url
```bash
sudo nano /etc/tor/torrc
```
then enter these lines
```
HiddenServiceDir /var/lib/tor/hidden_service/
HiddenServicePort 80 127.0.0.1:8080
```
![](https://i.imgur.com/JnZJtoN.png)
This tells Tor:
Create a hidden service
Host it in the directory /var/lib/tor/hidden_service/
Forward port 80 on the .onion address to your local web server at port 8080

Next restart the tor
```bash
sudo systemctl restart tor
```
now check the URL or say hostname that you got from the tor to visit that particular hidden service 
```bash
sudo cat /var/lib/tor/hidden_service/hostname
```
and i got this
![](https://i.imgur.com/czqFTtL.png)

and when we visit this in the browser "tor browser"
![](https://i.imgur.com/OTnV9pv.png)


