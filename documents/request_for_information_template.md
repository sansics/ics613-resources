# Project Contacts

| Name   | Role               | Email              |
| ------ | ------------------ | ------------------ |
| %NAME% | %TITLE%            | <email@domain.com> |
| %NAME% | Engagement Manager | email@domain.com   |
| %NAME% | Engagement Lead    | email@domain.com   |
| %NAME% | %TITLE%            | email@domain.com   |
# Request for Information
A formal Request for Information (RFI) collects relevant documentation or information before starting a project. These RFI artifacts provide a preliminary understanding of the target environment, decrease the time spent on discovery and enumeration activities during the engagement, and ensure a safe, efficient, and valuable OT assessment.

# Background
%COMPANY% is under contract with %CLIENT% to deliver a %ASSESSMENT_TYPE% as specified in the Scope of Work (SOW) or Work Authorization Form (WAF) signed on %DATE%. 
To facilitate this assessment, %COMPANY% requires access to documentation, configuration, and sample data from the target site. The requested items are documented in the Requested Artifacts section below. 
%COMPANY% understands that you may not have all the items requested. Please provide as much as you can, even if it is still in draft. If you cannot provide an item, please clarify this with you Engagement Lead. If you have additional data that will be helpful, feel free to provide it. 

# Transferring RFI
Data can be shared using your selected file sharing system, or %COMPANY% can provide you access to our secure file sharing platform. Sensitive data should be encrypted before transmitting. Encryption passwords should be shared via an alternate means such as Signal, text message, or another out-of-band communication system. 

# Requested Artifacts

| **ID** | **Item Requested**                          | **Date Requested** | **Description**                                                                                                                                                           |
| ------ | ------------------------------------------- | ------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| R1     | In-scope target list                        |                    | An explicit list of domains, subnets, and assets considered in scope for the assessment.                                                                                  |
| R2     | Out-of-scope target list                    |                    | An explicit list of out-of-scope domains, subnets, and assets considered out-of-scope should not be touched during the assessment.                                        |
| R3     | Network diagrams                            |                    | Diagrams that describe the overall network topology                                                                                                                       |
| R4     | Zone and conduit diagrams                   |                    | Diagrams describing network zones, data flow, and communications allowed between zones.                                                                                   |
| R5     | Asset inventory                             |                    | A list of all in-scope assets. Should include IP addresses, hostnames, and device function if available.                                                                  |
| R6     | IP schema                                   |                    | List of significant IP ranges and network zones to help interpret network traffic captures.                                                                               |
| R7     | Network packet captures (PCAPs)             |                    | Network traffic captures collected both north-south and east-west from key network infrastructure devices. See Network Traffic Capture Guidance section for more details. |
| R8     | Piping & Instrumentation Diagrams (P&ID)    |                    | Diagrams describing the industrial process.                                                                                                                               |
| R9     | Firewall, router, and switch configurations |                    | Exported configuration files for key network devices (routers, firewalls, and switches) used in or at the perimeter of the target OT environment.                         |
| R10    | Test accounts                               |                    | Test accounts for assumed breach foothold. See Accounts section for more details.                                                                                         |
| R11    | Test workstations                           |                    | Test machines (laptops, VDI, etc.) for assumed breach foothold. See Test Machines section for more details.                                                               |
# Test Machines
%COMPANY% assessors will require workstations from which to perform assessment activities. These can be dedicated testing laptops provided by %COMPANY%, or testing can be performed from %CLIENT%-provided hosts. 
Below are the basic requirements for %CLIENT%-provided hosts:

| **ID** | **Spec**               | **Requirement**                  | **Description**                                                                                                                       |
| ------ | ---------------------- | -------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------- |
| A      | Number of workstations | 2                                | There are two assessors; each will require a workstation                                                                              |
| B      | Operating System       | Windows                          | Modern version of Windows (I.e., Windows 11)                                                                                          |
| C      | Memory                 | 16GB                             | These workstations will host multiple VMs, and need sufficient memory to do so                                                        |
| D      | CPU                    | 4 cores minimum                  | Modern CPU requirements. Multiple cores required for VM hosting                                                                       |
| E      | Storage                | 80GB minimum                     | Necessary for storing VM files and testing artifacts                                                                                  |
| F      | Permissions            | Local Administrator              | Necessary to facilitate installation of additional tools, setting configuration, or other administrative actions during testing       |
| G      | Network Interfaces     | Ethernet port                    | Necessary to connect to the target network for assessment activities. May need to enable promiscuous mode for passive traffic capture |
| H      | Domain membership      | Domain-joined                    | Domain membership may be required to facilitate assumed breach scenario                                                               |
| I      | Software               | VirtualBox or VMWare Workstation | Necessary for hosting virtual machines                                                                                                |


# Test Accounts

| ID  | **Account**                        | **Quantity** | **Description**                                                                      |
| --- | ---------------------------------- | ------------ | ------------------------------------------------------------------------------------ |
| A   | IT Domain User                     | 2            | Standard domain user to facilitate assumed breach foothold scenario in the IT domain |
| B   | OT DMZ Domain User                 | 2            | Standard domain user to facilitate assumed breach foothold scenario in the OT DMZ    |
| C   | DCS Application Read-only Operator | 2            | Read-only Operator account for target DCS application                                |
| D   | Local Workstation Administrator    | 2            | Local administrator account for management of dedicated assessment workstations      |

# Network Traffic Capture Guidance

| ID  | Location             | Target Traffic Description                                                                                                                                                                                                                                                                             | Duration                                  |
| --- | -------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ----------------------------------------- |
| A   | Perimeter            | **Traffic entering or exiting the industrial network.** Capture all network choke points to external networks, e.g., a perimeter firewall. Consider if additional points exist such as interfaces to 3rd parties, or neighboring utilities.                                                            | Target: 6 hours<br>Max: 24 hours or 50GB  |
| B   | DMZ                  | **Asset and protocol inventory.** Identify critical digital assets such as DCS servers, engineering stations, or PLCs and the control system switches where this traffic converges. Capture bulk endpoint traffic for all nodes. Capturing traffic from multiple switches is typically necessary here. | Target: 6 hours<br>Max: 24 hours or 50GB  |
| C   | Control Network      | **Asset and protocol inventory.** Identify field SCADA networks and the switches where this SCADA traffic converges with the control system. Capturing traffic from multiple switches is typically necessary here.                                                                                     | Target: 6 hours<br>Max: 24 hours or 50GB  |
| D   | Other Critical Nodes | **Detailed Node Traffic.** Specific assets of interest have been identified. Capture all traffic on all interfaces for these nodes. Consider redundant NICs or dual-homed interfaces where applicable.                                                                                                 | Target: 24 hours<br>Max: 72 hours or 50GB |
