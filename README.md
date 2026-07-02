Purple Team Home Lab

Penetration testing lab combining offensive and defensive security techniques using Kali Linux, Metasploitable2, and Splunk Enterprise.


Architecture

ComponentRoleIPKali LinuxAttacker192.168.0.128Metasploitable2Victim192.168.0.129Splunk Enterprise 10.4.0SIEMlocalhost:8000


Attacks Executed

Phase 1 — Network Reconnaissance & Exploitation


Nmap — Port scanning and service enumeration
Metasploit — vsftpd 2.3.4 backdoor exploit (CVE-2011-2523) → root shell obtained
Hydra — FTP brute force attack (500+ attempts)


Phase 2 — Reverse Shell Techniques


Python reverse shell → Netcat listener (port 4444) — manual technique demonstrating socket-level shell hijacking
msfvenom payload (linux/x86/shell_reverse_tcp) → Metasploit handler (port 4445) — ELF binary delivered via HTTP, executed remotely



Detections (Splunk SPL)

DetectionEventsQuery FilePort scan detection3,077splunk-queries/detecciones.mdFTP exploit detection46splunk-queries/detecciones.mdBrute force detection5,431splunk-queries/detecciones.mdReverse shell detection3splunk-queries/reverse-shell-detection.md


Repository Structure

purple-team-home-lab/
├── pentest_report_metasploitable2.docx
├── splunk-queries/
│   ├── detecciones.md
│   └── reverse-shell-detection.md
└── README.md


Tools Used

Kali Linux · Nmap · Metasploit Framework · msfvenom · Hydra · Python · Netcat · Splunk Enterprise · iptables · tcpdump


Author

Fernando Sanchez Juarez — Cybersecurity Portfolio
