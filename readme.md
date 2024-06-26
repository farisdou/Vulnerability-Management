# Vulnerability Management For Personal Desktop

## Tools
- Nessus Tenable Professional 
- [HardenTools](https://github.com/hardentools/hardentools)

## Operating System
- Windows 11 

---

Before doing my scans, I did a system hardening to reduce my vectors.
![image](https://github.com/FarisDou/Vulnerability-Management/assets/109401839/cce3a153-6b4d-4da3-bde3-ec0107ecd897)

Additionally, I debloated Windows 11 from unnecessary tools, applications, processes and telemetry that comes prepackaged with windows. 

The last part for this was to run Nessus Teneable to see what vulnerabiltiies are on my system. I found 25 in total, 23 being informational for fingerprinting. 
The main two are as listed below with the solutions;

Before Fixes:
![image](https://github.com/FarisDou/Vulnerability-Management/assets/109401839/f41233db-c782-4ab7-b7ee-8b64b7f713b5)

---

## Vulnerabilties

---
![image](https://github.com/FarisDou/Vulnerability-Management/assets/109401839/732e7561-5f7c-4602-9b96-560f5debdd52)

### SMB Signing not requiredRisk Information

 Risk Factor: Medium 
 CVSS v3.0 Base Score 5.3
 CVSS v3.0 Vector: CVSS:3.0/AV:N/AC:L/PR:N/UI:N/S:U/C:N/I:L/A:N
 CVSS v3.0 Temporal Vector: CVSS:3.0/E:U/RL:O/RC:C
 CVSS v3.0 Temporal Score: 4.6
 CVSS v2.0 Base Score: 5.0
 CVSS v2.0 Temporal Score: 3.7
 CVSS v2.0 Vector: CVSS2#AV:N/AC:L/Au:N/C:N/I:P/A:N
 CVSS v2.0 Temporal Vector: CVSS2#E:U/RL:OF/RC:C
 Vulnerability Information
 Exploit Available: true
 Exploit Ease: Exploits are available
 Vulnerability Pub Date: January 17, 2012

The goal is to prevent man in the middle attacks. Change Local Group Policy Editor, 
![image](https://github.com/FarisDou/Vulnerability-Management/assets/109401839/8fc114ae-a80e-457d-af14-59cf5022c58e)

Computer Configuration, Windows Setting, Security Setting, Security Options, Enable 
" Microsoft Network Server: Digitally Sign Communications (Always)"

![image](https://github.com/FarisDou/Vulnerability-Management/assets/109401839/27328a2b-d1a2-42b1-bfd0-93a0b72a8165)

![image](https://github.com/FarisDou/Vulnerability-Management/assets/109401839/e10a0567-6ea5-4f14-914b-901d6804effe)

---

![image](https://github.com/FarisDou/Vulnerability-Management/assets/109401839/052f00d4-46e6-4cec-8042-b7f10250d3fc)

![image](https://github.com/FarisDou/Vulnerability-Management/assets/109401839/d98cb24e-6182-4578-8ccb-1af9d7d379a2)

### SSL Certificate Cannot Be Trusted
Risk Information
Risk Factor: Medium
CVSS v3.0 Base Score 6.5
CVSS v3.0 Vector: CVSS:3.0/AV:N/AC:L/PR:N/UI:N/S:U/C:L/I:L/A:N
CVSS v2.0 Base Score: 6.4
CVSS v2.0 Vector: CVSS2#AV:N/AC:L/Au:N/C:P/I:P/A:N

![image](https://github.com/FarisDou/Vulnerability-Management/assets/109401839/e477c0c9-d17f-4137-b7f1-8f604427f83d)


We will need a custom CA to resolve this, in this case we will generate a self-signed SSL certificate and then add it into nessus rules.  Follow the below instruction to create self signed certificate for development purpose


1. Open powershell command as administrator. 
If you do not have powershell installed, use this link to install. 

https://learn.microsoft.com/en-us/powershell/scripting/install/installing-powershell?view=powershell-7.3

2. Run this command in powershell
$cert = New-SelfSignedCertificate -CertStoreLocation "cert:\LocalMachine\My"

3. Run this command in powershell
$pwd = ConvertTo-SecureString -String ‘Test@123’ -Force -AsPlainText

4. To see the thumbprint run below 
 $cert

---

Final Notes, No IoC for the following: 
- WannaCry Ransomware
- Ripple20 Remote Scan
- Spectre and Meltdown
- CISA top vulnerabilties scan
- Microsoft Proxy C2C Scan.
- Solorigate
- ContiLeaks
- 2022 Threat Landscape Report (TLR)


No risk factors found and benign/informational alerts.

Certain ports and their applications are more likely to be targeted because they often have weaker credentials and defenses. 

Common vulnerable ports include:

![image](https://github.com/FarisDou/Vulnerability-Management/assets/109401839/cb49e2f4-d798-4a95-9254-b051f78b061d)

``` FTP (20, 21)
SSH (22)
Telnet (23)
SMTP (25)
NetBIOS over TCP (137, 139)
SMB (445)
Ports 1433, 1434 and 3306
Remote desktop (3389) ```

Its best practice to close ports that are unused. You can use netstat -aonb in CMD to see what processes/ports are listening, established, or close wait.
