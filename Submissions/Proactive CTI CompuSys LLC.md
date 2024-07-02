
### Proactive CTI (RFI) Submission by Justin Hysler (@dfir_justin)

---

This submission is based on the CompuSys, LLC organization profiled [here](https://github.com/BushidoUK/CTI-Analyst-Challenge/blob/main/Demos/DemoStakeholders.md).

`PIR Provided by CompuSys LLC: Based on recent (past 18 months) Cyber attacks against government, military, and telecommunications organizations, which parts of CompuSys LLC's internet-facing infrastructure are most likely to be exploited for use in follow-on Cyber attacks against our company, and what are the associated TTPs?`

### Executive Summary

Over the past 18 months, multiple threat actor groups (including ecrime and nation state actors) have specifically targeted peer organizations and the sectors of our primary customers, with the intention of espionage and ransomware operations. Of particular concern are espionage operations attributed to Russian Federation intelligence services. These actors have successfully exploited many of the same internet facing\perimeter infrastructure that CompuSys relies on for collaboration tools and remote access. These groups have been observed pivoting from these perimeter devices to on-premises to achieve persistence even after impacted devices are patched or rebuilt.

To mitigate these threats, we recommend implementing a faster patching cadence/escalation procedures for perimeter services/devices, additional monitoring of identity log sources, implementing detections specific to EWS, and implement policies that allow your SOC to rotate sensitive credentials.

## Key Findings

### Widely Exploited Internet Facing Devices for Initial Access

#### 1. Citrix Netscaler and ADC Gateways (CVE-2023-4966, Buffer Overflow/Session Hijacking)

_Link to CompuSys: Technology asset_

Mandiant is tracking 4 uncategorized (UNC) threat actors exploiting this vulnerability, including those deploying Lockbit ransomware. This vulnerability is also in CISA's Known Exploited Vulnerability (KEV) Catalog as "Known" to be involved in ransomware campaigns. It was added to the KEV only after 8 days of a published CVE, this pattern is consistent with commoditty and widely publicized exploits being used by ransowmare threat actors.

Note: Citrix Netscalers and ADC Gateways also had an unauthenticated remote code execution vulnerability reported in Summer 2023[1].  Given the continued success of eCrime actors and interest into these devices. It is expected that this Citrix Netscaler/ADC appliances will be of continued interest to threat actors.

| MITRE ATT&CK Tactic | Technique |
|----------|----------|
| Initial Access | T1133: External Remote Services |
| Initial Access | T1190: Exploit Public Facing Application |

---

#### 2. VPN Appliances (Ivanti Pulse Secure)
_Link to Compusys: Technology Asset_

 - In addition to technical vulnerabilities listed in CISA's KEV[2] threat actors are using stolen credentials, password sprays, and MFA fatigue techniques to achieve a point of presence in victim networks. Patching these vulnerabilities as they are disclosed and monitoring for these compromised credentials/user identities should be a high priority.

| MITRE ATT&CK Tactic | Technique |
|----------|----------|
| Initial Access | T1133: External Remote Services |
| Initial Access | T1190: Exploit Public Facing Application |
| Resource Development | T1586: Compromise Accounts |
| Resource Development | T1650: Acquire Access |
| Credential Access | T1110.003 Password Spraying |
| Credential Access | T1621: Multi-Factor Request Generation |

---

 ### Collaboration tools used for Resource Development, Discovery, and Collection

#### 3. Confluence
_Link to CompuSys: Technology Asset, Peer victim (Cloudflare)_

 - It is unclear if CompuSys's Confluence instance is publicly accessible. However, identity (tokens/service principals/user credentials) based attacks for the purpose of information gathering/collection and subsequent persistence using higher level privileges (that are documented in Confluence) have been observed by threat actors associated to software supply chain attacks (Okta) leading into Cloudflare's Confluence environment.

 In Cloudflare's incident, the TA used a service token and abandoned accounts that were stolen from Okta to authenticate to Confluence. The TA's end goal was to discover secrets related to Cloudflare's infrastructure (specifically remote access and end user management tools). For persistence, the actor created a new user in Confluence and added the to multiple groups. They were also able to pivot to JIRA and install a script runner plugin that was then used to activate C2 (Silver Framework) between the Atlassian server and TA controlled infrastructure.

| MITRE ATT&CK Tactic | Technique |
|----------|----------|
| Initial Access | T1133: External Remote Services |
| Initial Access | T1190: Exploit Public Facing Application |
| Resource Development | T1586: Compromise Accounts |
| Resource Development | T1650: Acquire Access |
| Persistence | T1136.003: Create Account |
| Persistence | T1505 Server Software Component |
| Discovery | T1087: Account Discovery |
| Discovery | T1526: Cloud Service Discovery |
| Discovery | T1069: Permission Groups Discovery |

---

`Author's note: For the purposes of this CTF, it is assumed that Exchange On-Prem is federated with an O365 tenant`

#### 4. Exchange On-Prem
_Link to CompuSys: Technology Asset, Peer victim (Microsoft), Nation State interest victims in customer's verticals._

- There is significant overlap (similar tech stack, victim peer organization, and victim customer industries) between Microsoft and CompuSys as it relates to potential targeting by Russian Federation Intelligence services. Microsoft's detailed report on Midnight Blizzard's (or Nobelium) attacks against Microsoft's corporate e-mail environment highlights several areas of risk that align to CompuSys' organizational profile. The intended goal of this attack was espionage/intelligence collection on Microsoft customers in which the Russian Federation may have strategic interests.

The initial access for this incident was a legacy account used for testing in a non-production O365 tenant "test tenant". The TA gathered was able to gain access to that account through the use of "low and slow" password spraying (using distributed infrastructure to test passwords against a limited number of accounts). This level of patience and sophistication is more commonly associated with nation state threat actors.

Lateral movement from the "test tenant" was accomplished via an OAuth application that was federated to a Microsoft corporate tenant. This OAuth application had unnecessarily high privileges that allowed the TA to use create a new OAuth applications in the "test tenant" for persistence. It also allowed the TA to create a new "test tenant" user and grant them the  `full_access_as_app` role for Office 365 Exchange Online in the Microsoft corporate tenant.

This `full_access_as_app` allows both full access to mailboxes in the environment but also to the Exchange Web Services (EWS) API. This API can be used to programmatically query and interact with Exchange resources (E-mail, Calendar, Contacts) in a way that can be easily scripted.

The end goal for this threat actor appears to be espionage and intelligence gathering on Microsoft customers. The available intelligence did not name specific customers.

Given that both CompuSys and Microsoft provide software services to government and military organizations, it is of critical important that CompuSys be aware of this event.

| MITRE ATT&CK Tactic | Technique |
|----------|----------|
| Resource Development | T1650: Acquire Access (Test Tenant Account) |
| Resource Development | T1583.001 Acquire Infrastructure (Test Tenant Domain) |
| Initial Access | T1078: Valid Accounts |
| Execution | T1059.009 : Command and Scripting Interpreter (Cloud API) |
| Persistence | T1136.003: Create (Cloud) Account |
| Defense Evasion | T1550.001: Application Access Token (OAuth) |
| Credential Access | T1110.003: Brute Force: Password Spraying |
| Discovery | T1087: Account Discovery (Cloud and E-mail) |
| Lateral Movement | T1021.007: Remote Services (Cloud) |
| Collection | T114.002: Email Collection: Remote Email Collection |
---

### Courses of Action (CoAs)
- Recommendations

1. Create an escalated patching procedure for internet edge facing infrastructure using risk based criteria. This criteria should be agreed upon prior to an event by Security, IT, and Business Owners. 

    - The Citrix Netscaler example listed above had an eight day turnaround time before being listed on the KEV. By the time the reports have reached CISA, these attempts have typically been known for days by security researchers.
  
    - Criteria for an escalated patching cycle can be based on 1. Known TTP and alignment prior TA objectives, 2. Ease of Exploit, 3. Difficulty of Patching, and 4. Operational Impact of downtime.
  
       - Example: New unauthenticated RCE vulnerability is disclosed for an edge facing VPN product. 

       -  1.  Known TTP of Ransomware threat actors and alignment to past campaigns would make this vulnerability highly likely to be used against our organization
       -  2.  Ease of Exploit: Proof of Concept (PoC) is in public circulation. It would be trivial for a threat actor to weaponize.
       -  3.  Difficulty of Patching: Patch takes about 20 minutes and requires a reboot. Patch itself is easy, but need to test and validate in a QA environment prior to moving to production. (*Note: In rare instances some patches require a rebuild of the appliance and restore from a prior config)
       -  4. Operational Impact of Downtime: To patch all of these appliances will create a 2 hour rolling outage of our VPN services. Remote workers will be unable to connect to the corporate environment.

    - Given this scenario, it would be prudent to being testing and planning to patch during normal working hours. 

2. Ensure you are monitoring and creating detections for the following identity log sources.

    - VPN Appliance and VPN User Session Logs - Infrastructure used by threat actors could be compared to those of legitimate users. Any discrepancies would need to be investigated. (Ex: A user typically authenticates from a CONUS based ISP, now they have sessions in the middle of the night from OVH Hosting out of France). Appliance audit logging are also helpful in detecting new local users/changes to privileged accounts that could be used by TA's for persistence or lateral movement.
    - Netscaler audit and application Logs - Application logs are necessary to identify what traffic may be hitting vulnerable endpoints. Post disclosure, are the exploit attempts we are seeing the commodity PoC? An EASM tool? or are they weaponized attempts by a threat actor?
    - Identity Provider / MFA Logs - These logs are necessary to determine if a compromised user's authentication factors have changed. Compusys needs to be able to detect if users have enrolled new devices, have new access policies applied, and/or are they being bombarded by MFA attempts or have a large number of MFA failures. 
    - Granting of OAuth Applications and Service Principals - OAuth applications are commonly used as persistence mechanisms and are often "fire and forget" once these applications are approved/roles are granted. This makes them wonderful targets for threat actors to use for persistence and defense evasion. Logging and alerting on new OAuth Applications or similar access methods tokens is important to make sure A) Those approved are properly scoped and have a legitimate use case and B) New approvals are done by a limited number of authorized accounts. (Ex: Alert the SOC on if an approval is done by an unknown user)

3. Monitor consumption of the EWS API - Review EWS Event Logs and correlate events to specific users and determine a baseline of expected activity. Admittedly, this may be a noisy detection (Employee gets a new laptop and pulls down their entire mailbox). However, you may be able to tune it to see long running syncs and/or excessive consumption that could suggest a compromised identity. Alternatively, investigating throttling events could be used to identify suspicious behavior. If using O365 for some users, then Defender for Cloud Apps has some built in detections.

4. Create policies\runbooks that allow the Security Operations Center (SOC) to rotate suspected or confirmed compromised credentials (basic auth, multi factors, OAuth, Service Principals, Tokens, etc..) - It is common for Security Operation Centers to have standing rights in AD or Entra in order to disable or rotate user credentials. However, alternative access methods (tokens, secrets) tend to be less visible in GUI driven workflows and some SOCs may have visibility or operational gaps when remediating issues. We recommend creating scripted workflows that can granularly revoke access. 

    - Scenario we want to avoid : Employee is tricked by a TA into accepting a 2FA request via MFA bombing. The TA is able to gain access to the user's O365 account. An alert fires in the SOC, but the analyst only disables the account in on-prem AD. The threat actor will still have access to O365 until either A) The primary refresh token for O365 expires and/or B) EntraAD syncs with on-prem and then disables the accounts O365 access. 

If the SOC has the ability to disable the account in AD "and" revoke all O365 / Azure sessions then we can avoid the TA having access to their mailbox for a prolonged period of time.

### Conclusion

This Priority Intelligence Requirement (PIR) has identified several incidents that align to CompuSys LLC's organizational profile and external threat surface. Based on our research of relevant incidents in the past 18 months, we conclude that CompuSys LLC has several high value technology assets that would be of interest to threat actors who target software companies. 

Midnight Blizzard (Nobelium) in particular has the highest degree of overlap with CompuSys based on the provided PIR.

Despite the challenges in facing Nation State actors, Compusys has the opportunity to create detections that have been demonstrated as effective against similar techniques and procedures.

### Additional Sources outside of CTF Demo Sources
[1, https://nvd.nist.gov/vuln/detail/CVE-2023-3519]
[2, https://www.cisa.gov/news-events/cybersecurity-advisories/aa24-060b]
[3, https://learn.microsoft.com/en-us/exchange/client-developer/exchange-web-services/ews-throttling-in-exchange]