# D34 — Threat Detection with ELK Stack (SSH Brute Force)

## Objective
Simulate an SSH brute force attack chain and investigate it using Kibana KQL queries and detection rules.

## Environment
- Ubuntu WSL2
- Elasticsearch 8.x
- Kibana
- Filebeat

## Attack Chain Simulated
1. SSH brute force — multiple failed login attempts
2. Successful login after credential guessing
3. Privilege escalation via sudo
4. Credential dumping (/etc/shadow)
5. Reverse shell established

## What was built
- Ingested simulated SSH attack logs into ELK via Filebeat
- Built KQL queries to detect brute force patterns
- Created detection rule: "SSH Brute Force Detection - D34"
- Investigated full attack chain in Kibana Discover

## KQL Queries Used
```
event.action: "ssh_failed_login"
message: "Failed password"
message: "Accepted password"
```

## Detection Rule
Name: SSH Brute Force Detection - D34  
Condition: More than 5 failed SSH logins from same IP in 1 minute

## Tools Used
- ELK Stack (Elasticsearch, Kibana, Filebeat)
- KQL (Kibana Query Language)

## Key Learning
ELK Stack can detect brute force attacks through log correlation and threshold-based detection rules. Kibana's rule engine fires alerts when attack patterns match defined conditions.
