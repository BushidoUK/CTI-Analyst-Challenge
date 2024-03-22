## Demo Incident Artifacts

### Incident 1: Phishing Email leads to Malware Infection
| Incident Description |
|---|
| A phishing email was sent containing a malicious link that dropped an XLSM file which the user opened an enabled the Excel macros. This launched a malicious DLL that later downloaded and executed another DLL file. The antivirus software says the first DLL was the BazarLoader malware and the second DLL was the IcedID malware. Some reconnaissance activity was also observed inside the network before the malware was blocked. | 

| Victim Hostname | Username |
|---|---|
| USWEST00123 | kreeves1 |

| Host-based IOCs | Network-based IOCs |
|---|---|
| Processes: EXCEL.exe, Rundll32.exe Files: XLSM, BazarLoader DLL, IcedID DLL | IPs: BazarLoader Payload Host, IcedID Payload Host |

---

### Incident 2: Password Spraying leads to Account Takeovers
| Incident Description |
|---|
| Multiple failed login attempts were observed against our unused Service Desk accounts, but resulted in an authorised successful login. The accounts had the same weak passwords given to them from when they were first setup and nobody was actively using these accounts so the adversary enrolled themselves into our MFA system and gained access to our Sharepoint tenant. We have since blocked their access. | 

| Victim Hostname(s) | Username(s) |
|---|---|
| UKSERVDESK, DESERVDESK, IESERVDESK | ukservicedesk, deservicedesk, ieservicedesk |

| Host-based IOCs | Network-based IOCs |
|---|---|
| 10s of failed login attempts, followed by a single successful login | IPs: Residential Proxies |

---

### Incident 3: VPN Exploitation leads to Compromised Account
| Incident Description |
|---|
| We detected unusual activity in our Active Directory and suspect that our internet-facing Fortinet FortiGuard device was potentially compromised due to an unauthorised actor attempting to authenticate to other devices on the network with the credentials on the device. | 

| Victim Hostname(s) | Username(s) |
|---|---|
| FORTIGUARD003, DC01 | FORTIADMIN, DA01 |

| Host-based IOCs | Network-based IOCs |
|---|---|
| Processes: cmd.exe, rundll32.exe, comsvcs.exe, ntds.util Files: NTDS.dit, frp.exe, update.bat  | IPs: SOHO network edge devices |

---

### Incident 4: JavaScript Malware Detected
| Incident Description |
|---|
| The user visited a WordPress blog and downloaded a ZIP file, which contained a malicious JavaScript file that the user executed. Malicious commands were detected by our EDR and the system was automatically quarantined. | 

| Victim Hostname(s) | Username(s) |
|---|---|
| DAVEJOHN | djohnson |

| Host-based IOCs | Network-based IOCs |
|---|---|
| Processes: explorer.exe, wscript.exe File: agreement.zip, agreement.js | Domain: Hijacked Wordpress Site |

---

### Incident 5: USB Malware Detected
| Incident Description |
|---|
| Malware was detected on a user's system after they plugged in a personal USB device. The user stated that the device was recently used at a print copy store to scan documents requested by HR during their onboarding process. The EDR detected malicious commands and the system was quarantined. | 

| Victim Hostname(s) | Username(s) |
|---|---|
| CPBACON | chrispbacon |

| Host-based IOCs | Network-based IOCs |
|---|---|
| Processes: cmd.exe, msiexec.exe FilePath: D:\\USBCHRIS.lnk  | IPs: Compromised QNAP NAS system |
