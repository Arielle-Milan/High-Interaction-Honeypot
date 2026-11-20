# Behavioural Analysis of Threat Actors via High-Interaction Segmented Honeypots

**Module:** ITPNA3-B34, Information Technology
**Institution:** Eduvos
**Group:** This was a group project which I completed with 4 of my peers.

---

## Academic integrity notice
This repository contains coursework submitted for formal assessment at Eduvos. Eduvos retains ownership of this assessed work under institutional policy. 
Please do not copy, submit, or adapt any part of it for your own academic work. Doing so is plagiarism and a breach of academic integrity at any institution. If you are a current ITPNA3-B34 student, please do your own work.

---

## Project Overview
Traditional signature-based intrusion detection is structurally blind to post-exploitation activity carried out with legitimate administrative tools ("living-off-the-land" techniques). CrowdStrike's 2024 Global Threat Report records an average *breakout time* of 79 minutes — the time it takes an attacker to move from an initially compromised host to another system — leaving defenders very little margin to notice and respond.

This project designs, builds, tests and evaluates a segmented, high-interaction honeypot intended to close that visibility gap: a deliberately exposed Windows Server and decoy PC, isolated from a simulated production network by VLAN segmentation and access control lists, instrumented with Zeek, Suricata, Wireshark and Splunk, and exercised against controlled attack scenarios mapped to the MITRE ATT&CK framework.

Testing found that the project's own central security claim was, for a time, not actually enforced — an access control list had been written correctly but never applied to its interface. Finding and correcting that gap, rather than a uniformly clean result, is the project's clearest evidence that adversarial testing — not design review alone — is what verifies a segmentation claim.

## Repository structure
├── README.md
├── Mini-Dissertation_Full_Project.docx      # Consolidated write-up of all 7 deliverables
├── Deliverable1_Proposal/                   # Research proposal: problem, aim, objectives, scope
├── Deliverable2_LiteratureReview/           # Related work, MITRE ATT&CK, critical gaps
├── Deliverable3_ThreatModel/                # Proposed model, STRIDE threat model, NIST 800-30 risk assessment
├── Deliverable4_Prototype/                  # Network configs (GNS3), honeypot & monitoring stack build
├── Deliverable5_Testing/                    # Vulnerability assessment, pentest, config audit, IR, compliance
├── Deliverable6_Conclusion/                 # Evaluation, lessons learned, recommendations
└── Deliverable7_Presentation/               # Final presentation speaking notes


## Architecture summary
| Zone | VLAN | Subnet |
|---|---|---|
| Production | 10 | 192.168.10.0/26 |
| Monitoring | 20 | 192.168.10.64/27 |
| Honeypot | 30 | 192.168.10.96/28 |
| Attacker | 40 | 192.168.10.112/28 |
| Management | 99 | 192.168.10.136/29 |

Built in GNS3 with VirtualBox-hosted Windows Server (honeypot), Windows decoy PC, Ubuntu (Suricata/Zeek/Wireshark/Splunk), and Kali Linux (attacker).

## Key Findings
- Honeypot successfully compromised via RDP, SSH brute-force and SMB enumeration.
- An access control list on the Organisation Router was found, during testing, to be defined but never applied to its interface — the single root cause behind several other findings (honeypot over-exposure, an unauthenticated SMTP service reachable from the untrusted zone).
- Configuration audit: 10/12 checks passed. Compliance mapping (POPIA/ECTA/ISO 27001): partial, tracing to the same root cause.
- Full findings, remediation and recommendations are in Deliverable5_Testing/ and Deliverable6_Conclusion/.

## LicensE
No open-source license is granted. All rights reserved by the authors and Eduvos. This repository exists for the group's own reference and submission purposes only.
