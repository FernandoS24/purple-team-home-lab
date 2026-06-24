Purple Team Home Lab

Penetration testing lab combining offensive and defensive security techniques using Kali Linux, Metasploitable2, and Splunk Enterprise.

Architecture

| Component | Role | IP |
|---|---|---|
| Kali Linux | Attacker | 192.168.0.128 |
| Metasploitable2 | Victim | 192.168.0.129 |
| Splunk Enterprise | SIEM | localhost:8000 |

Attacks Executed

- **Nmap** — Port scanning and service enumeration
- **Metasploit** — vsftpd 2.3.4 backdoor exploit (CVE-2011-2523) → root shell obtained
- **Hydra** — FTP brute force attack (500+ attempts)

Detections (Splunk SPL)

- Port scan detection — 3,077 events
- FTP exploit detection — 46 events  
- Brute force detection — 5,431 events

Repository Structure
purple-team-home-lab/

├── pentest_report_metasploitable2.docx

├── splunk-queries/

│   └── detecciones.md

└── README.md

Tools Used

Kali Linux · Nmap · Metasploit Framework · Hydra · Splunk Enterprise · iptables · tcpdump

## 👤 Author

Fernando Sanchez Juarez — Cybersecurity Portfolio
