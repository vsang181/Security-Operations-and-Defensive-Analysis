# Attacker Methodology

## The Network as a Whole

### The DMZ 

As a majority of the enterprises offer Internet-facing services. In a defense-in-depth deployment, these services are hosted in a protected environment separated from the internal network. Also known as demilitarized zone (DMZ). 

![image](https://github.com/vsang181/Security-Operations-and-Defensive-Analysis/assets/28651683/79d8e6fb-10e8-458b-ad2a-6734c132a6af)

The diagram shows a DMZ with a web server and an email server connected to a router. Email traffic destined for corporate users will come to this email server. Publicly-available corporate services and information will be hosted on the web server. All three devices including the router are flanked by firewalls on both sides.

> Note: Some suggest implementing front-end and back-end firewalls from different vendors since one vulnerability will rarely affect both firewalls. However, this may increase the initial cost and complicate maintenance.

### Deployment Environments

Deployment environments separate the development and deployment environments. They implement deployment phases that aim to protect the production environment from negative consequences introduced by the development process. 

![image](https://github.com/vsang181/Security-Operations-and-Defensive-Analysis/assets/28651683/19e40f6c-d339-48e1-b070-85e159ceb1e0)

### Core and Edge Network Devices

Autonomous systems send network traffic through core Internet network devices to intended destinations. Content delivery networks, commercial services, government resources, public utilities, and even personally-owned web resources all send and receive traffic without tracking (or caring about) the interconnected paths. Similarly, enterprises leverage similar core devices on their networks. These are often referred to as core or edge network devices. 

![image](https://github.com/vsang181/Security-Operations-and-Defensive-Analysis/assets/28651683/b8809e49-23f6-406f-bec0-6251c1937a18)

### Virtual Private Networks and remote sites

A Virtual Private Network (VPN) provides a means of securely accessing a private network while being geographically removed from it.

![image](https://github.com/vsang181/Security-Operations-and-Defensive-Analysis/assets/28651683/2aa32aaa-f53f-494a-852f-b293e7530343)

## The Lockheed-Martin Cyber Kill-Chain

### Importance

Use of a “chain”, means that each phase of the cyber attack is dependent on the next.

At a high level-
  
- The process of a cyber attack begins with an APT leveraging some form of vulnerability. 
- A vulnerability is a flaw in a system or its software. 
- APTs use exploit tools against these vulnerabilities to cause unintended behavior and ultimately gain unauthorized access to the system.
- They then often attempt to install malicious software (malware) to secure their access.
- Malware is designed to perform or enable undesirable actions on a user’s system.
- The process can repeat as an APT expands its access within the network.

> Note: The kill-chain model of cybersecurity is fragile from the viewpoint of the adversary. Mistakes made during any phase will prevent them from achieving their objective.

The phases of the Lockheed Martin Kill-Chain-

- Reconnaissance
- Weaponization
- Delivery
- Exploitation
- Installation
- Command & Control (C2)
- Actions on Objectives 

### Case Study: Monero Cryptomining

#### Kill-chain for Jenkins Exploitation

[Jenkins Mimner: One of the Biggest Mining Operations ever discovered](https://research.checkpoint.com/2018/jenkins-miner-one-biggest-mining-operations-ever-discovered/)

|Phase|Indicators|
|---|---|
|Reconnaissance|Scanning for public-facing Jenkins servers|
|Weaponization|PowerShell script to download the Monero miner|
|Delivery|HTTP POST to CLI directory containing code to fetch the weaponized binary|
|Exploitation|CVE-2017-1000353 [PowerShell script]|
|Installation|C:\Windows\minerxmr.exe|
|C2|222.184.79.11:5329|
|Actions on Objectives|Generate Monero cryptocurrency|

#### Kill-chain for WebLogic Exploitation

[XMRig Miner Now Targeting Oracle WebLogic and Jenkins Servers to Mine Monero](https://www.f5.com/labs/articles/threat-intelligence/xmrig-miner-now-targeting-oracle-weblogic-and-jenkins-servers-to-mine-monero)

|Phase|Indicators|
|---|---|
|Reconnaissance|Scanning for public-facing Oracle WebLogic servers|
|Weaponization|PowerShell script to download the Monero miner|
|Delivery|HTTP POST of XML containing code to fetch the weaponized binary|
|Exploitation|CVE-2017-10271 [PowerShell script]|
|Installation|C:\minerxmr.exe|
|C2|222.184.79.11:5319|
|Actions on Objectives|Generate Monero cryptocurrency|

### Case Study: Petya, Mischa, and GoldenEye

#### Kill-chain for Red-Petya

[Petya – Taking Ransomware To The Low Level](https://www.malwarebytes.com/blog/news/2016/04/petya-ransomware)
|Phase|Indicators|
|---|---|
|Reconnaissance|N/A|
|Weaponization|Encryption Payload in a self-extracting archive. Victim key generator|
|Delivery|Phishing email with DropBox link for job applicant|
|Exploitation|main executable run with administrator privileges|
|Installation|Setup.dll, Fake CHKDSK Encryptor|
|C2|N/A|
|Actions on Objectives|Encode, backup, overwrite MBR. Generate key. Force reboot. Encrypt MFT|

#### Kill-chain for Mischa Ransomware

[Petya and Mischa – Ransomware Duet (Part 1)](https://www.malwarebytes.com/blog/news/2016/05/petya-and-mischa-ransomware-duet-p1)

[Petya and Mischa: ransomware duet (part 2)](https://www.malwarebytes.com/blog/news/2016/06/petya-and-mischa-ransomware-duet-p2)

|Phase|Indicators|
|---|---|
|Reconnaissance|N/A|
|Weaponization|Encryption Payload in a self-extracting archive. Victim key generator|
|Delivery|Phishing email with DropBox link for job applicant|
|Exploitation|Main executable run without privileges|
|Installation|Setup.dll, Mischa.dll (Reflective DLL Load)|
|C2|N/A|
|Actions on Objectives|Generate key. Encrypt whitelisted filetypes. Avoid deny list directories|

#### Kill-chain for Goldeneye Ransomware

[Goldeneye Ransomware – the Petya/Mischa combo rebranded](https://www.malwarebytes.com/blog/news/2016/12/goldeneye-ransomware-the-petyamischa-combo-rebranded)

|Phase|Indicators|
|---|---|
|Reconnaissance|N/A|
|Weaponization|Encryption Payload in a self-extracting archive. Victim key generator|
|Delivery|Phishing email with DropBox link for job applicant|
|Exploitation|Main executable run without privileges|
|Installation|core.dll, elevate_x86.dll, elevate_x64.dll, Fake CHKDSK Encryptor|
|C2|N/A|
|Actions on Objectives|Generate key. Encrypt files like Mischa. Encrypt MBR/MFT like Petya|

## MITRE ATT&CK Framework

Unlike Lockheed-Martin’s Kill-Chain, MITRE ATT&CK describes the technology-specific actions executed on a target (in explicit technical detail) and aligns them back to an overarching goal or tactic.

The Lockheed-Martin Cyber Kill-Chain describes why attackers follow a particular series of steps, from external reconnaissance to exploiting target machines. The MITRE ATT&CK Framework describes how attackers perform these actions. The latter also serves as an introductory reference to the attacker methodology while providing practical attack examples .

### Tactics, Techniques, and Sub-Techniques

There are fourteen enterprise-specific tactics that resemble Lockheed-Martin’s model, or expand on it.

- Reconnaissance
- Resource Development
- Initial Access
- Execution
- Persistence
- Privilege Escalation
- Defense Evasion
- Credential Access
- Discovery
- Lateral Movement
- Collection
- Command and Control
- Exfiltration
- Impact 

Within each tactic are techniques and sub-techniques.

[MITRE ATT&CK Framework](https://attack.mitre.org/)

> Note: Security professionals often refer to APT behavior as TTPs: Tactics, Techniques, and Procedures. While tactics and techniques align with the MITRE ATT&CK Framework, subtechniques are a generalized form of procedures.

### Case Study: OilRig

[QUADAGENT](https://attack.mitre.org/software/S0269/)

#### OilRig Mitre ATT&CK

|Tactic|Technique|Sub-technique|
|---|---|---|
|Initial Access|Phishing|Spear-phishing attachment|
|Execution|User Execution|Malicious Link|
||Command and Scripting|PowerShell (QUADAGENT)|
||Interpreter||
|Command and Control|Application Layer Protocol|Web Protocols|
|||DNS|

### Case Study: APT3

[APT3](https://attack.mitre.org/groups/G0022/)

[Operation Clandestine Wolf – Adobe Flash Zero-Day in APT3 Phishing Campaign](https://www.mandiant.com/resources/blog/operation-clandestine-wolf-adobe-flash-zero-day)

#### APT3 Mitre ATT&CK

|Tactic|Technique|Sub-technique|
|---|---|---|
|Persistence|Create Account|Local Account|
||Valid Account|Domain Accounts|
|Privilege Escalation|Scheduled Task/Job|Scheduled Task|
|Command and Control|Application Layer Protocol|Web Protocols|
|||DNS|

### Case Study: APT28

[Exploitation of Remote Services](https://attack.mitre.org/techniques/T1210/)

[APT28 Targets Hospitality Sector, Presents Threat to Travelers](https://www.mandiant.com/resources/blog/apt28-targets-hospitality-sector-presents-threat-travelers)

#### APT28 Mitre ATT&CK

|Tactic|Technique|Sub-technique|
|---|---|---|
|Credential Access|Network Sniffing|N/A (Responder)|
|Lateral Movement|Exploitation of Remote|N/A (MS17-010)|
||Services||
||Remote Services|Remote Desktop Protocol|
|||SMB/Windows Admin Shares|
|||Windows Remote Management|
|Command and Control|Application Layer Protocol|Web Protocols|

## Let's Connect

I welcome your insights, feedback, and opportunities for collaboration. Together, we can make the digital world safer, one challenge at a time.

- **LinkedIn**: (https://www.linkedin.com/in/aashwadhaama/)

I look forward to connecting with fellow cybersecurity enthusiasts and professionals to share knowledge and work together towards a more secure digital environment.
