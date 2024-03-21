## Demo Incident Artifacts

### Incident 1: Phishing Email Leads To Malware Infection
| Incident Description |
|---|
| A phishing email was sent containing a malicious link that dropped an XLSM file which the user opened an enabled the Excel macros. This launched a malicious DLL that later downloaded and executed another DLL file. The antivirus software says the first DLL was the BazarLoader malware and the second DLL was the IcedID malware. Some reconnaissance activity was also observed inside the network before the malware was blocked. | 

| Victim Hostname | Username |
|---|---|
| USWEST00123 | kreeves1 |

| Host-based IOCs | Network-based IOCs |
|---|---|
| Files: XLSM, BazarLoader DLL, IcedID DLL | IPs: BazarLoader Payload Host, IcedID Payload Host |

---

### Incident 2: Password Spraying Leads to Account Takeovers
| Incident Description |
|---|
| Multiple failed login attempts were observed against our unused Service Desk accounts, but resulted in an authorised successful login. The accounts had the same weak passwords given to them from when they were first setup and nobody was actively using these accounts so the adversary enrolled themselves into our MFA system and gained access to our Sharepoint tenant. We have since blocked their access. | 

| Victim Hostname(s) | Username(s) |
|---|---|
| UKSERVDESK, DESERVDESK, IESERVDESK | ukservicedesk, deservicedesk, ieservicedesk |

| Host-based IOCs | Network-based IOCs |
|---|---|
| 10s of failed login attempts, followed by a single successful login | The attacker connetions originated from residential proxies |

---

### Incident 3:
- A
- B
- C

---

### Incident 4:
- A
- B
- C

---

### Incident 5: 
- A
- B
- C
