For installation mod security
right now i am implementing it into the dvwa and on the apache server that is used in the dvwa web application.

so first step is the installation of mod security
```bash
sudo apt update
sudo apt install libapache2-mod-security2 -y
```
then check whether it is installed properly or not
```bash
apachectl -M | grep security
```

![](https://i.imgur.com/sm0gsBb.png)

here security2_module is shown means it is installed
then check whether it is enabled or not
```bash
sudo a2enmod security2
```

![](Z)
now restart the apache it is not required right now as we will setup and configure rules after installing it from the owasp crs and then configure it and then final implementation.
```bash
sudo systemctl restart apache2
```

now locating the conf file and setting it up for use
```bash
ls /etc/modsecurity/
```

![](https://i.imgur.com/qUpNQO7.png)

this recommended is the original file so dont change it just make a new copy of it and then will use it

```bash
cp modsecurity.conf-recommended modsecurity.conf 
```

now after this open the conf file and then we have to set it to on which will do all the things instead of just detection

![](https://i.imgur.com/cvbYKyc.png)

change **SecRuleEngine** to **On**

Now installing the rules set from the OWASP
**OWASP Core Rule Set (CRS)**
```bash
sudo apt install modsecurity-crs -y
```

after installing this go the directory and add this file which was missing in my case 
```bash
ls /usr/share/modsecurity-crs/
```
**modsecurity-crs.conf.example**
which is the connection file for the rules

you can find the same on this github 
https://github.com/coreruleset/coreruleset

then create a symbolic link for enabling the rules and that can be done by this command
```bash
sudo ln -s /usr/share/modsecurity-crs/modsecurity-crs.conf /etc/modsecurity/crs-setup.conf
```

then again restart the apache

i got an error of duplicate id 
![](https://i.imgur.com/MpgEJ4x.png)

so changed the id by incrementing it to 1

now lets try in the dvwa
so restart the mariadb and then start the dvwa 
```bash
apachectl configtest
```
this command is used for the testing of properly working of the apache
![](https://i.imgur.com/5EhvDbv.png)

it was not working as i did not change the apache file 
so as i saw the video we need to set the rule engine on and also give the rules conf file into it

```bash
cd /etc/apache2/mods-enabled/
nano security2.conf
```

![](https://i.imgur.com/QDDAg4N.png)

add these things and then lets try again

after that we have to add one more thing i.e. the default.conf file of the apache2
```
DocumentRoot /var/www/html/

        <Directory /var/www/html/dvwa>
                Require all granted
                AllowOverride All
        </Directory>
```

```
<IfModule security2_module>
                SecRuleEngine On
                Include /usr/share/modsecurity-crs/crs-setup.conf
                Include /usr/share/modsecurity-crs/rules/*.conf
        </IfModule>
```
![](https://i.imgur.com/JAro4PE.png)

![](https://i.imgur.com/6mmlolG.png)

and also one more file that is there available
![](https://i.imgur.com/OtsLd18.png)

after doing this just cross check this thing
![](https://i.imgur.com/z0WmJrP.png)

now last thing to do is to check the **owasp-crs.load** file as it is necessary and without that it was not giving the forbidden message
```
nano /etc/apache2/mods-available/owasp-crs.load
```

![](https://i.imgur.com/e2Qdtmd.png)

then save it
and follow these steps and thenrestart the apache2
```bash
a2enmod security2
a2enconf owasp-crs
systemctl restart apache2
```

and now lets check whether it is working or not
```

```

so lets do that and then it will be working

![](https://i.imgur.com/Qf0jc5B.png)

```
curl "http://localhost/?id=1' OR '1'='1"

curl "http://localhost/?q=<script>alert('XSS')</script>"

```
