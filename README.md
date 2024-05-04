# AD-Lab-with-Splunk

## Objective

In this project, we have set up one Active Directory Server, one Windows 10 PC, a Kali Linux that simulates an attacker, and a Splunk server running on Ubuntu. 

The project is based on <a href="https://www.youtube.com/@MyDFIR"> @MyDFIR </a> Active Directory Project (Home Lab).

### Skills Learned

- Basic introduction to Splunk
- Basic introduction to Active Directory
- Basic introduction to Kali Linux and Ubuntu
- Basic intruduction to crowbar, Windows Event Code
- Basic introduction to Atomic Red Team and MITRE ATT&CK framework

### Tools Used
- Splunk
- Active Directory
- Kali Linux's crowbar
- Ubuntu
- Atomic Red Team

## Network Diagram
<img src="https://imgur.com/VsW4P2P.jpg" width="400" />

*Ref 1: Network Diagram*

All four VMs are in the same 192.168.100/24 network and are hosted in Virtualbox.  

### Virtualbox NAT Network Settings


<img src="https://i.imgur.com/JRGOMqZ.jpg" width="400" />

*Ref 2: NAT Networks ADProject configuration*

ADProject has a IPv4 Prefix of 192.168.100.0/24.

<img src="https://i.imgur.com/0FSsjvl.jpg" width="400" />

*Ref 3: Windows 10 NAT Network*

All VMs are attached to NAT Network and still have internet access. In *Ref 3*, only the settings for Windows 10 is shown.

### Time Zone Settings
Do check the timezone settings of:
- AD Server
- Windows 10

As I am located in Singapore, these are my settings:

<img src="https://i.imgur.com/dNumorv.png" width="400" />

*Ref 4: AD Server Timezone*


<img src="https://i.imgur.com/WxuQ2UG.png" width="400" />

*Ref 5: Windows 10 Timezone*

<img src="https://i.imgur.com/1tIauaw.jpg" width="400" />

*Ref 6: Ubuntu Timezone*

It is ok to set the Ubuntu (Splunk) server to use UTC.

<img src="https://i.imgur.com/oNRhNdY.jpg" width="200" />

*Ref 7: Kali Timezone*

I decided to use Singapore timezone for Kali.

### VMs IP address

Follow @MyDFIR's youtube instructions at 
- Active Directory Project (Home Lab) | Part 3 for Ubuntu and Windows 10
- Active Directory Project (Home Lab) | Part 4 for AD Server
- Active Directory Project (Home Lab) | Part 5 for Kali Linux

### Sysmon

Follow @MyDFIR's youtube instructions at Active Directory Project (Home Lab) | Part 3.

He uses a sysmon config from <a href="https://github.com/olafhartong/sysmon-modular"> Olaf Hartong's github </a>

### Splunk

Follow @MyDFIR's youtube instructions at 
- Active Directory Project (Home Lab) | Part 3 for Ubuntu, Windows 10, AD Server

Splunk Enterprise will be installed at Ubuntu.

Splunk Universal Forwarder will be installed at Windows 10 and AD Server.

<img src="https://i.imgur.com/k4yqRMi.png" width="400" />

*Ref 8: Two hosts visible to Splunk* 

If you have configured everything correctly, Splunk should see two hosts.

### Using Crowbar against Windows 10's RDP

<img src="https://i.imgur.com/irUZGg5.jpg" width="200" />

*Ref 9: Attacker's password.txt* 

The attacker has this password.txt and will launch an attack on Windows 10's RDP. 


<img src="https://i.imgur.com/u8ePjs1.jpg" width="300" />

*Ref 10: Attacker issues the crowbar command* 

In Ref 10, the attacker manages to crack tsmith's RDP password to the Windows 10 PC.

<img src="https://i.imgur.com/XHZtSef.jpg" width="400" />

*Ref 11: Telemetry on Splunk* 

This attack has generated this telemetry on Splunk. 23 events are seen for index = endpoint tsmith.

<img src="https://i.imgur.com/EJK7hV6.jpg" width="400" />

*Ref 12: Windows Event Codes*

There are twenty counts of EventCode 4625 and one of 4624.

<img src="https://i.imgur.com/oZpTreU.jpg" width="400" />

*Ref 13: Windows Event Code 4625*

According to <a href="https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/event.aspx?eventID=4625"> Ultimate Windows Security </a>, 4625 is used to indicate "An account failed to log on".

4624 is "An account was successfully logged on".

<img src="https://i.imgur.com/VoR28F5.jpg" width="400" />

*Ref 14: Splunk information on Kali*

If you were to use index=endpoint tsmith EventCode=4625 and scroll down, you will see Kali as seen in Ref 14. 

### Atomic Red Team

Follow @MyDFIR's youtube instructions at Active Directory Project (Home Lab) | Part 5 to install Atomic RedTeam on Windows 10.

<img src="https://i.imgur.com/Bvkxpq1.jpg" width="400" />

*Ref 15: Atomic Red Team successful install*

If successful, you will see C:\AtomicRedTeam directory.

### Mitre Att&ck T1136.001

<img src="https://i.imgur.com/9h3KGJy.jpg" width="400" />

*Ref 16: Mitre Att&ck T1136.001 Local Account*

<img src="https://i.imgur.com/tRBwhCh.jpg" width="400" />

*Ref 17: Atomic Red Team T1136.001*

The Atomic Red Team T1136.001 is used to simulate a local account creation.

<img src="https://i.imgur.com/sQQRotl.jpg" width="400" />

*Ref 18: Simulating Local Account Account creation through Atomic Red Team T1136.001*

Notice that NewLocalUser is created.

<img src="https://i.imgur.com/r6XxN1i.jpg" width="400" />

*Ref 19: NewLocalUser detected by Splunk*

With index=endpoint host="TARGET-PC" NewLocaluser, T1136.001's telemetry can be detected. 

