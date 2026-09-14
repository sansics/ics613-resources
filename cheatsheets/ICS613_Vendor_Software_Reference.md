# ICS613 Vendor Software and Protocol Reference

**ICS/OT Penetration Testing and Assessments**

Vendor engineering, HMI, historian, and DCS software introduced in the ICS613 courseware, primarily in 613.3 (Common ICS Applications) and 613.5 (DCS Enumeration). These tools are the most effective enumeration instruments in an OT environment because they are already installed, already authorized, and already configured to communicate with production devices. Adversaries use the same tools. The courseware's guidance: learn each tool in a lab before arriving on-site, and prefer tools already present in the environment over external tooling.

This reference is in two parts. **Protocol Enumeration Tools** is organized by OT protocol; **Vendor suites** (Rockwell, ABB, Siemens, and so on) are organized by ecosystem.

Most vendor tools are publicly downloadable from the vendor's site, though many require registration or a license. The "Source" column points to the vendor product page; specific download portals vary by product and version.


## Protocol Enumeration Tools

Organized by OT protocol, because that is how the tool gets chosen in the field: an assessor discovers a protocol on the wire, then reaches for the client that speaks it. Each protocol lists the tool(s) the courseware names, plus obvious free companions an assessor would carry. Python protocol libraries appear here in brief; full command examples live in `ICS613_Tool_Command_Cheat_Sheet.md`.

Vendor-specific configuration tools that happen to speak a protocol (RSLinx, PowerLogic ION Setup) live under their vendor suites below since they are ecosystem tools, not general protocol interrogators.

### OPC (Classic DA/HDA/A&E and UA)

The 613.5 800xA lab uses MatrikonOPC Explorer against the ABB OPC DA server (`ABB.AfwOpcDaServer.1`). OT sites run both Classic and UA, so carry one client for each.

| Tool | Specs | Notes | Source |
|------|-------|-------|--------|
| MatrikonOPC Explorer | Classic DA, HDA, A&E | The lab tool. Classic (DCOM) only — no UA. | https://www.matrikonopc.com/products/opc-desktop-tools/opc-explorer.aspx |
| Matrikon OPC UA Explorer | OPC UA | UA-only counterpart from the same vendor; separate free download. | https://www.matrikonopc.com/opc-ua/opc-ua-explorer.aspx |
| OPC Expert | UA + Classic (DA, A&E, HDA) + OPC Xi | Single free tool spanning UA and all Classic specs; portable, no install, no DCOM config. | https://opcexpert.com/ |
| UaExpert | OPC UA (DA, A&C, HDA, methods) | De facto standard free UA test client; cross-platform. UA only. Free account required. | https://www.unified-automation.com/products/development-tools/uaexpert.html |
| opcua-asyncio (FreeOpcUa) | OPC UA (scripting) | Python UA client/server library used in 613.5. | https://github.com/FreeOpcUa/opcua-asyncio |

### PROFINET

| Tool | Notes | Source |
|------|-------|--------|
| PRONETA | Siemens free tool for passive PROFINET topology discovery; named in 613.3. | https://support.industry.siemens.com/ |
| Wireshark (PN-DCP dissector) | Decodes PROFINET Discovery (PN-DCP) to enumerate device names and IPs from captured traffic. | https://www.wireshark.org/ |

### Modbus (TCP / RTU)

| Tool | Notes | Source |
|------|-------|--------|
| pymodbus | Python Modbus client/server used in the course; reads and writes coils/registers. | https://github.com/pymodbus-dev/pymodbus |
| pyModbusTCP | Lightweight Python Modbus TCP client/server used in the course. | https://github.com/sourceperl/pyModbusTCP |
| Metasploit `scada/modbus*` modules | Banner grab, unit-ID enumeration, and register read/write; used in the Workbook. | https://github.com/rapid7/metasploit-framework |
| modbus-cli | Simple free command-line Modbus reader/writer. | https://github.com/tallakt/modbus-cli |

### EtherNet/IP and CIP

| Tool | Notes | Source |
|------|-------|--------|
| pylogix | Python library that reads/writes tags on Rockwell ControlLogix/CompactLogix over EtherNet/IP CIP; used in the course. | https://github.com/dmroeder/pylogix |
| Metasploit `scada/enip_identity` | Enumerates EtherNet/IP device identity; used in the Workbook. | https://github.com/rapid7/metasploit-framework |
| Nmap `enip-info` script | Free NSE script that pulls CIP identity from an EtherNet/IP device. | https://nmap.org/nsedoc/scripts/enip-info.html |
| cpppo | Python EtherNet/IP CIP library/CLI for tag read/write. | https://github.com/pjkundert/cpppo |

### Siemens S7 (S7comm / S7comm-plus)

| Tool | Notes | Source |
|------|-------|--------|
| python-snap7 | Python wrapper for the Snap7 library; reads S7 PLC data blocks. Used in the course. | https://github.com/gijzelaerr/python-snap7 |
| Snap7 (library) | The underlying open-source S7 communication suite (client, server, partner). | https://snap7.sourceforge.net/ |

### BACnet

| Tool | Notes | Source |
|------|-------|--------|
| BAC0 | Python BACnet library used in the course to read device object properties. | https://github.com/ChristianTremblay/BAC0 |
| YABE (Yet Another BACnet Explorer) | Free Windows BACnet discovery/browse tool. | https://sourceforge.net/projects/yetanotherbacnetexplorer/ |

### DNP3

| Tool | Notes | Source |
|------|-------|--------|
| dnp3 (stepfunc) | Rust DNP3 stack (with bindings) shipping runnable master/outstation examples; named in the course. | https://github.com/stepfunc/dnp3 |

### IEC 60870-5-104

| Tool | Notes | Source |
|------|-------|--------|
| iec104-python (c104) | Python IEC-104 client/server library used in the course. | https://github.com/fraunhofer-fit-dien/iec104-python |

## Rockwell Automation

| Tool | Purpose | Source |
|------|---------|--------|
| Studio 5000 | Programming and configuration environment for Logix controllers (successor to RSLogix 5000). | https://www.rockwellautomation.com/en-us/products/software/factorytalk/designsuite/studio-5000.html |
| RSLogix | Legacy programming software for Rockwell PLC families. | https://www.rockwellautomation.com/ |
| RSLinx / FactoryTalk Linx | Communication server that browses the ControlLogix backplane and enumerates devices on an EtherNet/IP segment; the practical entry point for Rockwell-network enumeration (named in 613.3). | https://www.rockwellautomation.com/en-us/products/software/factorytalk/linx.html |
| FactoryTalk (View, ME, Optix) | HMI, visualization, and connectivity suite spanning operator interfaces and cloud/edge connectivity. | https://www.rockwellautomation.com/en-us/products/software/factorytalk.html |

## ABB

| Tool | Purpose | Source |
|------|---------|--------|
| System 800xA | DCS platform whose operator workstation and application server reveal asset topology, controller data, and process configuration. | https://new.abb.com/control-systems/system-800xa |
| ABB Ability | Digital platform and connectivity portfolio across ABB automation products. | https://global.abb/group/en/ability |

## Siemens

| Tool | Purpose | Source |
|------|---------|--------|
| Totally Integrated Automation (TIA) Portal | Unified engineering framework for SIMATIC controllers, HMI, and drives. | https://www.siemens.com/tia-portal |
| SIMATIC | Controller, HMI, and industrial-software product family. | https://www.siemens.com/simatic |
| STEP 7 | Programming environment for SIMATIC S7 PLCs. | https://www.siemens.com/ |
| Insights Hub (formerly MindSphere) | Industrial IoT / analytics platform. | https://www.siemens.com/ |
| COMOS | Plant engineering and lifecycle-management software. | https://www.siemens.com/ |

## Schneider Electric

| Tool | Purpose | Source |
|------|---------|--------|
| EcoStruxure | Architecture and platform spanning Schneider automation, power, and building products. | https://www.se.com/ww/en/work/campaign/innovation/overview.jsp |
| PowerLogic ION Setup | Configuration tool for PowerLogic ION power-metering devices (named in 613.3). | https://www.se.com/ |
| AVEVA Insight | Cloud-based operations and analytics platform. | https://www.aveva.com/ |
| AVEVA Historian | Time-series process data historian. | https://www.aveva.com/ |

## Emerson

| Tool | Purpose | Source |
|------|---------|--------|
| DeltaV DCS | Distributed control system for process industries. | https://www.emerson.com/en-us/automation/deltav |
| AMS Device Manager | Asset-management software for field-device configuration and diagnostics. | https://www.emerson.com/ |
| OSIsoft PI | Widely deployed process data historian (now AVEVA PI System). | https://www.aveva.com/en/products/pi-system/ |

## Honeywell

| Tool | Purpose | Source |
|------|---------|--------|
| Experion PKS | Process knowledge / control system platform. | https://process.honeywell.com/ |
| Forge | Enterprise performance-management and analytics platform. | https://process.honeywell.com/ |
| Uniformance Suite | Historian and process-data analytics suite. | https://process.honeywell.com/ |

## GE Vernova

| Tool | Purpose | Source |
|------|---------|--------|
| Proficy Suite | HMI/SCADA, historian, and MES software family. | https://www.gevernova.com/software |
| Digital Twin Technology | Asset-modeling and simulation platform. | https://www.gevernova.com/ |
| GridOS ADMS | Advanced distribution management system for electric utilities. | https://www.gevernova.com/ |

## Yokogawa

| Tool | Purpose | Source |
|------|---------|--------|
| CENTUM VP | Integrated production control (DCS) system. | https://www.yokogawa.com/ |
| Plant Resource Manager | Asset-management and device-diagnostics software. | https://www.yokogawa.com/ |
| Exaquantum | Plant information management system / historian. | https://www.yokogawa.com/ |

## Phoenix Contact

| Tool | Purpose | Source |
|------|---------|--------|
| PLCnext Technology | Open control platform and engineering ecosystem. | https://www.phoenixcontact.com/ |
| mGuard | Industrial security appliances and firewall/VPN devices. | https://www.phoenixcontact.com/ |

## Schweitzer Engineering Laboratories (SEL)

| Tool | Purpose | Source |
|------|---------|--------|
| acSELerator | Configuration and analysis software for SEL protective relays and devices. | https://selinc.com/products/5030/ |
| BlueFrame Application Platform | Compute platform for hosting OT applications at the substation edge. | https://selinc.com/ |

## Other Automation Platforms Referenced

| Tool | Vendor | Purpose | Source |
|------|--------|---------|--------|
| CLICK Programming Software | AutomationDirect | Programming environment for the CLICK PLUS PLC used in the ICS613 labs. | https://www.automationdirect.com/support/software-downloads |
| CODESYS | CODESYS GmbH | Vendor-neutral IEC 61131-3 programming platform embedded in many third-party controllers. | https://www.codesys.com/ |

---

## Non-ICS Troubleshooting Tools Found on OT Hosts (613.3)

Operations teams install these ad hoc; they double as a ready-made recon toolkit for an assessor.

| Tool | Purpose | Source |
|------|---------|--------|
| Wireshark | Captures traffic directly from the workstation. | https://www.wireshark.org/ |
| TCPView | Sysinternals tool showing live TCP/UDP endpoints per process. | https://learn.microsoft.com/en-us/sysinternals/downloads/tcpview |
| Sysinternals Suite | Reveals processes, network connections, and logged-on users. | https://learn.microsoft.com/en-us/sysinternals/downloads/sysinternals-suite |
| AngryIPScanner | Maps reachable devices on the local control network. | https://angryip.org/ |
| PuTTY | SSH/serial/telnet client for device access. | https://www.putty.org/ |
| Java Runtime Environment | Dependency for many ICS applications; often outdated and vulnerable. | https://www.java.com/ |

---

*Source material: ICS613 courseware, © 2026 SANS Institute. Vendor product pages are provided for reference; download portals, licensing, and product names change between releases and should be verified against the current vendor site.*
