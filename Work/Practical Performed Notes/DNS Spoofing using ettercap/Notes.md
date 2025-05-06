so first lets check our ip and the subnet for our ip
and then check the active machines on our subnet which is of vmware only
![](https://i.imgur.com/L22WwOr.png)

for proof  side i have checked the ip of the victim
![](https://i.imgur.com/nBIaGb5.png)

here 192.168.177.137 is the victim machine whose dns will be spoofed in this practice. and the routers are 192.168.177.1 and .2

so now lets start ettercap using command
```
sudo ettercap -G
```
now select eth 0 and start scanning for hosts
![](https://i.imgur.com/hqUSYQn.png)

found 4 in which router and 137 is the victim one.

so lets start the process

now add the ip's into their target pos
![](https://i.imgur.com/LGG41gL.png)

192.168.177.2 and 192.168.177.1 into target 2 and 192.168.177.137 to target 1

then we will use the plugin dns_spoof
![](https://i.imgur.com/RX97POf.png)
go to the path plugins/manage_plugins/dns_spoof click on it
![](https://i.imgur.com/nRmm4Aj.png)

now change the file /etc/ettercap/etter.conf
in that the linux part 
![](https://i.imgur.com/qGafGJ9.png)

next thing to change is the dns file which is in the same directory and it is etter.dns

![](https://i.imgur.com/gjJGDEq.png)

now change this things


as i want to do spotify site dns spoof
i will use the blackeye python tool from aman sir to generate the python server for telegram phishing site and will enter that into the dns file

![](https://i.imgur.com/COXVscc.png)

see i have started the server for phishy login page of spotify
![](https://i.imgur.com/Ci3o5dH.png)

as in the first attempt i did some mistake so i made another two i.e. netflix and snapchat and then did the process again after adding both the routeres in target 2.

![](https://i.imgur.com/MHHw2Vv.png)

now will enter this to the dns file
![](https://i.imgur.com/542Heuz.png)

i have entered this into the dns file now save this and then lets try it

then do the poisoning 
![](https://i.imgur.com/sAMRRNR.png)

then start sniffing
![](https://i.imgur.com/RNO5Ryl.png)

and then visit the victim machine i.e. second vm in my vmware and then visit the sites in the incognito window

you will get this and this means that the dns has got spoofed successfully

![](https://i.imgur.com/jTJYMYl.png)

![](https://i.imgur.com/U8TcRra.png)
and after you have stopped the ettercap sniffing and attack

reload the site and it will get you to the original site.