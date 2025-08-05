Local Network Reconnaissance with Nmap

Task Overview

**Objective:**  
Discover open ports on devices in my local network using **Nmap** to understand network exposure and basic port scanning techniques.

**Tools Used:**  
- Kali Linux (in VirtualBox)
- Nmap

**My Local IP:** `10.0.2.15`  
**Network Range:** `10.0.2.0/24`

Find Local IP Range

Used:
ip addr

command:
sudo nmap -sS 10.0.2.0/24 > nmap.txt

result:
135/tcp  open  msrpc
445/tcp  open  microsoft-ds
1042/tcp open  afrog
1043/tcp open  boinc
7778/tcp open  interwise1

Port	Service			Description
135		MS RPC			Windows Remote Procedure Call
445		Microsoft-DS	SMB file sharing
53		DNS (Domain)	Domain Name System
1042	Afrog			Uncommon — likely ephemeral
1043	Boinc			Uncommon — likely ephemeral
7778	Interwise		Used by collaboration/VoIP tools

Port				Risk
135					Exposes Windows RPC → exploits possible
445					SMB → vulnerable to malware (EternalBlue)
53					If misconfigured → DNS poisoning
1042/1043/7778		May indicate unnecessary open services — should be checked/blocked if unused

