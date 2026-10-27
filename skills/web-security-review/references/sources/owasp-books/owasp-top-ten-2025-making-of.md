# PowerPoint Presentation

> Converted source material; attribution: OWASP. Snapshot processed 2026-10-27.
> Source SHA-256: `5da64a692453bca3a5d7fc495676fd8de23e0ef38b6f40a6c759ae9ff1df07eb`.
> Conversion: Native text extraction, all 24 text segments. Source opinions and historical examples are preserved, not automatically endorsed.


The Making Of
The OWASP 
Top Ten 2025Brian Glas

---

The OWASP Top Ten 2025
A01:2025 Broken Access Control
A02:2025 Security Misconfiguration
A03:2025 Software Supply Chain Failures
A04:2025 Cryptographic Failures 
A05:2025 Injection
A06:2025 Insecure Design
A07:2025 Authentication Failures
A08:2025 Software or Data Integrity Failures
A09:2025 Logging & Alerting Failures
A10:2025 Mishandling of Exceptional Conditions

---

The Changes
•Access Control stays at #1
•Security Misconfiguration goes from 5 -> 2 (data score)
•Software Supply Chain goes from 6 -> 3 (data was 10th, but survey was 1st )
•Cryptographic Failures goes from 2 -> 4 (data score is 3)
•Injection goes from 3 -> 5 (data was 4th)
•Insecure Design goes from 4 -> 6 (data was 6th)
•Authentication stays at 7th (data was 5th)
•Software and Data Integrity Failures stays at 8th (data was 9th, survey was 2nd)
•Logging & Alerting Failures stays at 9th (data was 11th, survey was 3rd)
•Mishandling of Exceptional Conditions is added at 10th (data was 7th, but survey was 5th )

---

The History

---

High Level Process
• We ask for data...    It takes 14-16 months
• Normalize the data
• Pull National Vulnerability Database for CVE -> CWE
• Normalize Exploit and Impact from CVSS
• Pull the CWE dictionary and group CWEs into logical categories
• Determine the formula weighting
• Build a data Top Ten
• Run Community Survey
• Weigh the survey with the data
• Determine the new Top Ten
• Write a lot, discuss, write more, review, feedback, discuss, release

---

Data Collection
CWEs (968ish)
- 2017 = 30 CWEs
- 2021 = 390 CWEs
- 2025 = 686 CWEs
•Accenture (Prague)
•Anonymous (multiple)
•Bugcrowd
•Contrast Security
•CryptoNet Labs
•Intuitor SoftTech Services
•Orca Security
•Probley
•Semgrep
•Sonar
•usd AG
•Veracode
•Wallarm

---

Data Collection
Contributions
- 2.8 million applications (conservatively)
- Min Data Request
- Year
- CWE
- Population tested
- Apps found with at least one instance of the CWE
Example: 2021, CWE-89, 1000, 200

---

Risk
Likelihood x Impact = Risk

---

Likelihood
Frequency vs Incidence Rate
- Frequency is not our friend in this case
- Tool-assisted Human (TaH) vs Human-assisted Tool (TaH)
How to normalize?
- Epidemiology
- Incidence Rate to determine impact in a population
What percentage of the population of tested apps has the 
vulnerability?

---

Likelihood
Incidence Rate: Incidence rate is the percentage of applications 
vulnerable to that CWE from the population tested by that org for 
that time period.
(Testing) Coverage:The percentage of applications tested by all 
organizations for a given CWE.
Total Occurrences:Total number of applications found to have the 
CWEs mapped to a category.

---

Impact
Impact
- Can vary wildly
- We use the NVD for Exploit and Impact
NVD Stats
- 219,291 CVEs
- 159,544 have CVSSv2 scores
- 156,407 have CVSSv3 scores
- 6,299 have CVSSv4 scores

---

Impact
Top 10 CWEs by count of CVE mappings
CWE-79 - Improper Neutralization of Input During Web Page Generation ('Cross-site Scripting')  30431
NVD-CWE-Other        24309
NVD-CWE-noinfo        21869
CWE-89 - Improper Neutralization of Special Elements used in an SQL Command ('SQL Injection') 14145
CWE-119 - Improper Restriction of Operations within the Bounds of a Memory Buffer  9550
CWE-20 - Improper Input Validation       7446
CWE-787 - Out-of-bounds Write       7199
CWE-200 - Exposure of Sensitive Information to an Unauthorized Actor    6246
CWE-22 - Improper Limitation of a Pathname to a Restricted Directory ('Path Traversal')  5982
CWE-352 - Cross-Site Request Forgery (CSRF)      5847

---

Impact
Top 10 CWEs by count of CVE mappings
CWE-125 - Out-of-bounds Read       4722
CWE-264 - Permissions, Privileges, and Access Controls (Prohibited)   3979
CWE-94 - Improper Control of Generation of Code ('Code Injection’)    3886
CWE-416 - Use After Free        3460
CWE-434 - Unrestricted Upload of File with Dangerous Type     2547
CWE-287 - Improper Authentication       2536
CWE-862 - Premature Release of Resource During Expected Lifetime    2482
CWE-284 - Improper Access Control       2298
CWE-310 - Cryptographic Issues (Prohibited)      2275
CWE-78 - Improper Neutralization of Special Elements used in an OS Command ('OS Command Injection') 2006

---

Impact
Weighted Exploit: The Exploit sub-score from CVSSv2 and CVSSv3 
scores assigned to CVEs mapped to CWEs, normalized, and placed on 
a 10-point scale.
Weighted Impact: The Impact sub-score from CVSSv2 and CVSSv3 
scores assigned to CVEs mapped to CWEs, normalized, and placed on 
a 10-point scale.
Total CVEs: Total number of CVEs in the NVD DB that were mapped 
to the CWEs mapped to a category.

---

Data Factors
CWEs Mapped: The number of CWEs mapped to a category by the Top Ten team.
Incidence Rate: Incidence rate is the percentage of applications vulnerable to that CWE from the population 
tested by that org for that time period.
Weighted Exploit: The Exploit sub-score from CVSSv2 and CVSSv3 scores assigned to CVEs mapped to CWEs, 
normalized, and placed on a 10pt scale.
Weighted Impact: The Impact sub-score from CVSSv2 and CVSSv3 scores assigned to CVEs mapped to CWEs, 
normalized, and placed on a 10pt scale.
(Testing) Coverage:The percentage of applications tested by all organizations for a given CWE.
Total Occurrences:Total number of applications found to have the CWEs mapped to a category.
Total CVEs: Total number of CVEs in the NVD DB that were mapped to the CWEs mapped to a category.
Formula: (Max Incidence Rate % * 1000) + (Max Coverage % * 100) + (Avg Exploit * 10) + (Avg Impact * 20) + 
(Sum Occurrences / 10000) = Risk Score

---

Data Factors

---

Data Factors

---

Survey Data

---

Survey Data
Count Purpose
145 It helps provides structure for standards, requirements, security tests, test results, etc
129 Mostly for education of developers
85 We have to adhere to compliance that references the Top Ten
79 We build processes around it
46 We build tools to test for it
19 It doesn’t, but I think it’s important
1 Awareness and examples
1 Helps me discuss with clients finding and how best to harden systems in order to 
provide Cyber insurance
1 in immature companies/orgs its a starting point. mature orgs mature into org specific 
top 5 for each 6 month release cycle
1 None of the above
1 we have a SAST tool and our users are interested in how our categories map to the 
OWASP Top 10
1 We use it to focus people on the important issues in appsec and make sure they're 
not confused about what area of cybersecurity we are talking about.

---

Survey Data

---

Survey Data

---

The Survey Results

---

What can you do?
• Map findings to CWEs
• Map findings to good CWEs (stop using prohibited)
• Map findings to root cause CWEs
• There is so much more than the Top Ten
• OWASP SAMM (https://owaspsamm.org) 
• Many other OWASP projects

---

THANK YOU!
That’s
All
FolksQR Code here 
for slides
