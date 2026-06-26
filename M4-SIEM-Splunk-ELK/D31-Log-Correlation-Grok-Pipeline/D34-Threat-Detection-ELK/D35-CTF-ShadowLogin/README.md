# D35 — CTF: Operation ShadowLogin (M4 Capstone)

## Objective
Investigate a simulated multi-stage attack using ELK Stack. Reconstruct the full attack chain from SSH brute force to reverse shell.

## Environment
- Ubuntu WSL2
- Elasticsearch 8.x
- Kibana
- Filebeat

## Scenario
A threat actor targeted a Linux web server. As SOC L1 analyst, investigate the logs and answer 8 questions to reconstruct the attack chain.

## Attack Chain Reconstructed

| Step | What Happened | MITRE ATT&CK |
|---|---|---|
| 1 | SSH brute force — 8 failed attempts | T1110 — Brute Force |
| 2 | Successful login as webadmin | T1078 — Valid Accounts |
| 3 | whoami, id — privilege recon | T1033 — System Owner Discovery |
| 4 | cat /etc/passwd + /etc/shadow | T1003 — Credential Dumping |
| 5 | wget backdoor.sh from 185.220.101.99 | T1105 — Ingress Tool Transfer |
| 6 | Cron job added for persistence | T1053 — Scheduled Task |
| 7 | nc reverse shell on port 4444 | T1059 — Command & Scripting |

## CTF Results
Score: 8/8 ✅

## Key Findings
- Attacker IP: 185.220.101.45
- Compromised user: webadmin
- Malicious file: /tmp/backdoor.sh
- C2 IP: 185.220.101.99
- Reverse shell port: 4444

## Tools Used
- ELK Stack (Elasticsearch, Kibana, Filebeat)
- KQL (Kibana Query Language)

## Key Learning
Full attack chain investigation using SIEM. Mapped findings to MITRE ATT&CK. SOC analyst must correlate multiple log sources to reconstruct attacker timeline.
