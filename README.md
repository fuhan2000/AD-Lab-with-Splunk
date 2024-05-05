# AD-Lab-with-Splunk

## Objective
In this project, we have set up one Active Directory Server, one Windows 10 PC, a Kali Linux that simulates an attacker, and a Splunk server running on Ubuntu.

The project is based on the <a href="https://www.youtube.com/@MyDFIR">@MyDFIR</a> Active Directory Project (Home Lab).

### Tools Used
- VirtualBox
- Sysmon
- Splunk
- Active Directory
- Kali Linux's crowbar
- Ubuntu
- Atomic Red Team
- Mitre Att&ck

### Network Diagram
<img src="https://imgur.com/VsW4P2P.jpg" width="400" />

*Ref 1: Network Diagram*

All four VMs are in the same 192.168.100/24 network and are hosted in Virtualbox.

Each VM has access to the internet.

The attacker, positioned at Kali Linux (192.168.100.250), represents a potential threat to the network's security.

The AD Server and Windows 10 will have:
- Splunk Universal Forwarder
- Sysmon

Splunk Server will be at 192.168.100.10 and will run on Ubuntu.

### Summary of Findings
- The timezone settings of the VMs to be monitored are essential. In this Lab, the AD Server and Windows 10 are set to the Singapore time zone. If the timezones are incorrect, the wrong datetime may be reported to Splunk, which can complicate analysis.

- Crowbar uses brute force. In this Lab, only 21 passwords are used to save time. Using a passphrase can reduce the risk of brute-force attacks. The victim here, tsmith, uses a common password.

- Splunk can be used to detect brute-force attacks (many Windows Event Code 4625 within a very short timeframe) and determine where they originated.

- Atomic Red Team has a library of tests mapped to the Mitre Att&ck framework. Splunk can detect those telemetries in the T1136.001 Create Local Account and T1059.001 Powershell tests.

## Lab Details
### Virtualbox NAT Network Settings


<img src="https://i.imgur.com/JRGOMqZ.jpg" width="400" />

*Ref 2: NAT Networks ADProject configuration*

ADProject has an IPv4 Prefix of 192.168.100.0/24.

<img src="https://i.imgur.com/0FSsjvl.jpg" width="400" />

*Ref 3: Windows 10 NAT Network*

All VMs are attached to the NAT Network and still have internet access. However, in Ref 3, only the settings for Windows 10 are shown.

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

Setting the Ubuntu (Splunk) server to use UTC is okay.

<img src="https://i.imgur.com/oNRhNdY.jpg" width="200" />

*Ref 7: Kali Timezone*

I decided to use Singapore timezone for Kali.

### VMs IP address

Follow <a href="https://www.youtube.com/@MyDFIR">@MyDFIR</a>'s youtube instructions at 
- Active Directory Project (Home Lab) | Part 3 for Ubuntu and Windows 10
- Active Directory Project (Home Lab) | Part 4 for AD Server
- Active Directory Project (Home Lab) | Part 5 for Kali Linux

### Sysmon

Follow <a href="https://www.youtube.com/@MyDFIR">@MyDFIR</a>'s Youtube instructions at Active Directory Project (Home Lab) | Part 3.

He uses a sysmon config from Olaf Hartong's <a href="https://github.com/olafhartong/sysmon-modular">Github</a>.

### Splunk

Follow <a href="https://www.youtube.com/@MyDFIR">@MyDFIR</a>'s Youtube instructions at 
- Active Directory Project (Home Lab) | Part 3 for Ubuntu, Windows 10, AD Server

Splunk Enterprise will be installed at Ubuntu.

Splunk Universal Forwarder will be installed on Windows 10 and AD Server.

<img src="https://i.imgur.com/k4yqRMi.png" width="400" />

*Ref 8: Two hosts visible to Splunk* 

If you have configured everything correctly, Splunk should see two hosts (TARGET-PC and ADDC01).

### Using Crowbar against Windows 10's RDP

<img src="https://i.imgur.com/irUZGg5.jpg" width="200" />

*Ref 9: Attacker's password.txt* 

The attacker has this password.txt and will launch an attack on Windows 10's RDP. 


<img src="https://i.imgur.com/u8ePjs1.jpg" width="300" />

*Ref 10: Attacker issues the crowbar command* 

In Ref 10, the attacker manages to crack tsmith's RDP password to the Windows 10 PC.

<img src="https://i.imgur.com/XHZtSef.jpg" width="400" />

*Ref 11: Telemetry on Splunk* 

This attack has generated this telemetry on Splunk. Twenty-three events are seen for index = endpoint tsmith.

<img src="https://i.imgur.com/EJK7hV6.jpg" width="400" />

*Ref 12: Windows Event Codes*

There are twenty counts of EventCode 4625 and one of 4624.

<img src="https://i.imgur.com/oZpTreU.jpg" width="400" />

*Ref 13: Windows Event Code 4625*

According to <a href="https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/event.aspx?eventID=4625">Ultimate Windows Security</a>, 4625 indicates, "An account failed to log on".

4624 is "An account was successfully logged on".

<img src="https://i.imgur.com/VoR28F5.jpg" width="400" />

*Ref 14: Splunk information on Kali*

If you use index=endpoint tsmith EventCode=4625 and scroll down, you will see Kali, as seen in Ref 14.

### Atomic Red Team

To install Atomic RedTeam on Windows 10, follow <a href="https://www.youtube.com/@MyDFIR">@MyDFIR</a>'s YouTube instructions at Active Directory Project (Home Lab) | Part 5.

<img src="https://i.imgur.com/Bvkxpq1.jpg" width="400" />

*Ref 15: Atomic Red Team successful install*

If successful, you will see C:\AtomicRedTeam directory.

The following commands will come in useful before running Invoke-AtomicTest (More details at <a href="https://github.com/redcanaryco/invoke-atomicredteam/wiki/Installing-Invoke-AtomicRedTeam">Install-Invoke-AtomicRedTeam</a>):

*Set-ExecutionPolicy Bypass CurrentUser;*

*IEX (IWR 'https://raw.githubusercontent.com/redcanaryco/invoke-atomicredteam/master/install-atomicredteam.ps1' -UseBasicParsing);*

*Install-AtomicRedTeam -getAtomics -Force*

### Mitre Att&ck T1136.001

<img src="https://i.imgur.com/9h3KGJy.jpg" width="400" />

*Ref 16: Mitre Att&ck T1136.001 Local Account*

<img src="https://i.imgur.com/tRBwhCh.jpg" width="400" />

*Ref 17: Atomic Red Team T1136.001*

The Atomic Red Team T1136.001 simulates an adversary creating a local account.

<img src="https://i.imgur.com/sQQRotl.jpg" width="400" />

*Ref 18: Simulating Local Account Account creation through Atomic Red Team T1136.001*

Notice that NewLocalUser is created.

<img src="https://i.imgur.com/r6XxN1i.jpg" width="400" />

*Ref 19: NewLocalUser detected by Splunk*

With index=endpoint host="TARGET-PC" NewLocaluser, T1136.001's telemetry can be detected. 

### Mitre Att&ck T1059.001

<img src="https://i.imgur.com/OJbYfoA.jpg" width="400" />

*Ref 20: T1059.001 Powershell*

<img src="https://i.imgur.com/gY4RN5K.jpg" width="400" />

*Ref 21: Atomic Red Team T1059.001*

The T1059.001 is used to detect adversaries abusing Powershell commands and scripts. 

<img src="https://i.imgur.com/iluwP5E.jpg" width="400" />

*Ref 22: Invoke-Atomic Test T1059.001*

Running the T1059.001. Notice that Microsoft Defender detects threats. 

<img src="https://i.imgur.com/EGOmz40.jpg" width="400" />

*Ref 23: Powershell with -noprofile flag*

A PowerShell command was executed with the -noprofile flag.

<img src="https://i.imgur.com/CjGFfeX.jpeg" width="400" />

*Ref 24: Detecting the Powershell -noprofile*

Splunk can detect the Powershell -noprofile with index=endpoint host="TARGET-PC" powershell noprofile.

