Thing i understood till now is that ettercap is used for the mitm attacks.
which is done while you are in the network in which u want to do the mitm attacks.

so first thing we need to do install is wireshark and ettercap if not installed and also try to use virtual box instead of vm as you have to use the bridged network config

now set the network config to bridged and then start the kali

then open terminal and do the command to start ettercap 
and also run the same using sudo 
**sudo ettercap -G**
where according to me G is for the user gui
then you will be prompted the screen to select 
![[Pasted image 20250108170557.png]]
so deselect the sniffing at startup and select the primary interface according to the network in which the network adapter is set to bridged 
so i have nat and bridged both so i will be doing that and then click on the check button so you will be redirected to another ui
![[Untitled.png]]
here 1st is the start and stop button for sniffing 
then 2nd is for the select of type of attack or sniff
then 3rd is for stopping the sniffing and attack completely 
then 4th is for select target scanning hosts and all

here thing we need will be targets hosts and the 1,2,3 buttons 

first go to 3 dots and check each option
then go to **hosts** and **scan hosts** 
then click on **Hosts list** this will give you the complete list of ip and mac address on the network you are currently on
so now for the target part select the target you want sniff by **angry ip scanner**
Now for selecting targets now in the network i am right now 
the routers are **192.168.0.1** and **192.168.0.200** so these routers will be target 2
and in target 1 today only the 3 ios people came so ill pick two of them for the sniffing part 
so their ip's are 
**192.168.0.214** and **192.168.0.211**
add them to target 1

Now after adding the targets go to 3 dots and click on target and then **Current targets** so here we will be able to see the targets
![[Pasted image 20250108172120.png]]
now the 2nd option i showed you click on that and then **arp poisoning** and then this will appear![[Pasted image 20250108172228.png]]
so let this be as it is and click ok![[Pasted image 20250108172252.png]]
so now open **wireshark** first and get ready to check the sniffing
now click on the 1st option or button i showed you to start the sniffing and then open wireshark and click on **start capturing packets**
dont forget to click to **eth1** 
so this will be the list of packets in wireshark
![[Pasted image 20250108172918.png]]
so here i have added the field **ip.addr** to filter the ip address and also **&& dns** will show all the dns request or packets recieved from the targets 

now further will be taught in future 
things we need to check right now is **http, MDNS, DNS**
so for stopping this sniffing first click on the red stop button in wireshark and close wireshark
then go to the ettercap and then first click on 1st options to stop the sniffing and then click on 3rd option to stop the arp poisoning 
dont forget to do these steps cause it can trouble in future so be attentive and alert to not to do the mistakes.
