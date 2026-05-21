# Top-Down / Bottom-Up Assessment Methodology

[← Back to README](../README.md)

---

## Overview

The ICS613 Top-Down / Bottom-Up methodology structures an ICS/OT penetration test to mirror how real threat actors compromise industrial environments. It is modeled after the [SANS ICS Cyber Kill Chain](https://www.sans.org/white-papers/36297/) (Assante and Lee, 2015), which describes how adversaries conduct campaigns against industrial control systems through two stages: an initial cyber intrusion and intelligence-gathering stage, followed by a targeted ICS attack development and execution stage.

By following this structure, the assessment team demonstrates to OT stakeholders how an adversary would progress from external reconnaissance through corporate network compromise and ultimately toward the process environment. This gives asset owners a realistic view of their defensive posture at each layer of the architecture.

**Key principle:** An OT pentest follows threat actor patterns to identify and test protections for the systems, solutions, and data that are important to operations. The methodology is not an exercise in breaking things - it is a structured evaluation that helps asset owners understand where their defenses succeed and where gaps exist, mapped to how a real adversary would exploit them.

---

## Assessment Transition Boundary

The methodology recognizes a critical boundary in how testing is conducted:

- **Purdue Levels 5/4, DMZ, and 3 (Enterprise and Manufacturing Zones)** - Active and passive testing techniques are used, consistent with how threat actors operate in corporate and supervisory network environments.
- **Purdue Levels 2, 1, and 0 (Cell/Area Zone - HMI, Controllers, Field Devices)** - The assessment transitions to a **passive approach**. Testing in the process environment is conducted through interviews with OT stakeholders, observation of demonstrations performed by OT personnel, and supervised activities in OT lab environments where available. Active exploitation of production process equipment is not performed.

This transition reflects both the safety requirements of operational environments and the reality that sophisticated threat actors spend considerable time learning the process before attempting to interact with control systems.

---

## Top-Down Assessment (Aligned to ICS Cyber Kill Chain)

The Top-Down assessment begins from the external perspective and works inward and downward through the Purdue Reference Model, following the adversary's campaign progression.

### Stage 1 - Cyber Intrusion Preparation and Execution

Stage 1 maps to the intelligence and access operations an adversary conducts to gain a foothold in the target environment and gather information about the ICS. In the ICS Cyber Kill Chain, this stage resembles a traditional IT intrusion but with the specific objective of learning enough about the industrial process and control system to enable a follow-on attack.

#### Planning (Reconnaissance)

Reconnaissance is the collection of information about the target through observation, open source research, and other detection methods. In an assessment, this includes gathering publicly available information about the organization, its industrial operations, ICS vendors and technologies in use, and personnel. Tools such as search engines, Shodan, social media, regulatory filings, job postings, and vendor documentation all contribute to building a picture of the target environment.

The objective is to identify weaknesses and information that support targeting, delivery, and exploitation - including human, network, host, account, protocol, policy, and procedural information relevant to both the IT and OT environments.

#### Preparation (Weaponization and Targeting)

Preparation involves developing or selecting the tools, techniques, and attack paths that will be used to gain access based on what was learned during reconnaissance. Weaponization is the modification or configuration of tools and payloads for the specific engagement. Targeting is the process of analyzing and prioritizing which systems, networks, or personnel represent the best path into the environment given the trade-offs between effort, likelihood of success, and risk of detection.

Both activities may occur, but both are not always required. An adversary who discovers valid VPN credentials during reconnaissance may bypass weaponization entirely. An assessment team similarly selects the approach that most realistically tests the defender's controls.

#### Cyber Intrusion (Delivery, Exploit, Install/Modify)

The Cyber Intrusion phase is the attempt to gain access to the defender's network. Delivery is the method used to interact with the target - such as phishing emails, exploitation of external services, or use of compromised credentials. The Exploit step is the means by which the adversary performs the initial malicious action, whether through a software vulnerability, credential abuse, or misuse of legitimate features. A successful exploit leads to installation of a capability (such as a remote access tool) or modification of existing system functionality (such as leveraging built-in tools like PowerShell).

Defenders should not assume the threat is always malware-based. Living-off-the-land techniques using native operating system tools are common in both real-world attacks and assessment activities.

#### Management and Enablement (Command and Control)

With a successful intrusion, the adversary establishes command and control to maintain managed access to the environment. This may use direct connections to installed tools, abuse of trusted communications such as VPN tunnels, or methods that blend into normal network traffic. Capable adversaries establish multiple C2 paths to maintain access if one is detected.

In an assessment, this phase validates whether the organization can detect and respond to unauthorized remote access and lateral communication within their networks.

#### Sustainment, Entrenchment, Development, and Execution (Act)

This phase encompasses the adversary's post-exploitation activities within the compromised environment. Common activities include:

- **Discovery** - Identifying new systems, networks, shares, and data stores, including ICS-related assets accessible from the corporate network
- **Lateral Movement** - Moving between systems using harvested credentials, trust relationships, or network access
- **Credential Capture** - Collecting user credentials, service accounts, and authentication tokens
- **Collection** - Gathering ICS documentation, engineering files, network diagrams, project files, and configuration data that exist on corporate or enterprise networks
- **Privilege Escalation** - Elevating access to administrative or domain-level privileges
- **Data Identification** - Locating information that would enable an adversary to understand the ICS architecture, the industrial process, and the engineering decisions behind system design

This phase is critical because a significant amount of ICS and process information typically resides on enterprise networks. The assessment team identifies what ICS-relevant data an adversary could access from less-protected networks - including engineering drawings, PLC project files, network architecture documentation, vendor remote access configurations, and operational procedures.

**Stage 1 concludes when the assessment team has demonstrated the extent to which an adversary could compromise the security boundary of the ICS from the enterprise network and has catalogued the ICS-relevant intelligence accessible from that position.**

---

### Stage 2 - ICS Attack Development and Execution

Stage 2 of the ICS Cyber Kill Chain describes how an adversary uses knowledge gained in Stage 1 to develop, test, and execute a capability specifically designed to impact the ICS. In the assessment context, Stage 2 activities are conducted **passively** once the assessment reaches the process environment (Purdue Levels 2/1/0).

#### Attack Development and Tuning (Develop)

In a real attack, the adversary develops a tailored capability to affect the specific ICS implementation for a desired impact, typically using exfiltrated data from Stage 1. This development usually occurs off-site and is difficult for defenders to observe.

In the assessment, this phase is represented by the team's analysis of collected ICS data to identify what attack paths, misconfigurations, or vulnerabilities could be leveraged to develop a capability against the control system. The assessment team documents what an adversary *could* develop based on available information, without building or deploying actual ICS attack tools against production systems.

#### Validation (Test)

A real adversary would test their capability against similar or identical ICS configurations before deploying it. For sophisticated attacks, this may involve acquiring physical ICS equipment and software.

In the assessment, validation is addressed through bench testing and lab-based evaluation when OT lab environments are available. Vendor equipment reviews, firmware analysis, and configuration assessments of representative or spare devices provide insight into what an attacker could accomplish without introducing risk to production operations.

#### ICS Attack (Deliver, Install/Modify, Execute)

The final phase of the kill chain is the delivery, installation, and execution of the ICS-specific attack. Real-world ICS attacks may involve multiple concurrent components - enabling attacks that create necessary conditions, initiating attacks that modify process variables or control logic, and supporting attacks that hide the activity from operators (such as spoofing HMI displays).

**In the assessment, this phase is evaluated passively.** The assessment team does not deliver or execute attacks against production ICS equipment. Instead, the team:

- Conducts **interviews** with OT engineers and operators to understand control logic, safety system design, and operational procedures
- Observes **demonstrations** performed by OT stakeholders showing system configurations, access controls, alarm management, and change management processes
- Performs **supervised activities in OT labs** where available, working with OT personnel to evaluate device configurations, test network segmentation, and assess protocol security in isolated environments
- Documents findings regarding the attacker objectives identified in the ICS Cyber Kill Chain, including potential for loss of view, loss of control, denial of view, denial of control, manipulation of view, manipulation of control, denial of safety, manipulation of safety, and manipulation of sensors and instruments

---

## Bottom-Up Assessment

The Bottom-Up assessment complements the Top-Down approach by evaluating the environment from the perspective of the industrial process upward through the architecture. Where the Top-Down approach follows the adversary's intrusion path, the Bottom-Up approach examines the engineering and operational decisions that define the environment's resilience.

### Process and Field Level (Purdue Level 0)

Assessment of sensors, actuators, and field instrumentation through documentation review, interviews with process engineers, and observation. This includes understanding what physical processes are being monitored and controlled, what the consequences of manipulation would be, and what non-cyber safeguards (mechanical relief valves, physical interlocks) exist independent of the control system.

### Control Level (Purdue Level 1)

Evaluation of controllers (PLCs, RTUs), safety instrumented systems (SIS), and their configurations through documentation review, interviews, and supervised lab activities. This includes examining control logic documentation, firmware versions, access controls on controller programming, and the degree of separation between safety and control systems.

### Supervisory Level (Purdue Level 2)

Assessment of HMI systems, engineering workstations, local operator interfaces, and the supervisory network. This includes evaluating host hardening, application configurations, user access controls, alarm management, and network segmentation between the supervisory level and other zones. Depending on the environment and stakeholder agreement, some active host-level assessment may be appropriate for standalone systems in this zone.

### Manufacturing Zone (Purdue Level 3)

Evaluation of site-level servers, historians, application servers, and the operational network infrastructure. This level often represents the boundary where common IT protocols and industrial protocols coexist, and where network architecture decisions significantly impact the overall security posture. Active assessment techniques may be used at this level with appropriate coordination.

### DMZ

Assessment of the demilitarized zone between enterprise and manufacturing networks. This includes evaluating firewall rules, data diode implementations, remote access solutions, patch management servers, and any services that bridge the IT/OT boundary. The DMZ architecture is a critical control point - its design directly determines how much additional effort an adversary must expend to reach the process environment.

### Enterprise Zone (Purdue Levels 4/5)

Evaluation of the corporate and business network infrastructure, including Active Directory, remote access solutions, email systems, and any enterprise applications that store or process ICS-related data. This zone is assessed actively and represents the starting point for the Top-Down assessment path.

---

## Mapping to Attacker Objectives

The ICS Cyber Kill Chain identifies three categories of functional impact an adversary may target. The assessment evaluates defensive controls against each:

| Category | Objectives |
|----------|------------|
| **Loss** | Loss of View, Loss of Control |
| **Denial** | Denial of View, Denial of Control, Denial of Safety |
| **Manipulation** | Manipulation of View, Manipulation of Control, Manipulation of Sensors and Instruments, Manipulation of Safety |

The complexity and difficulty of achieving these objectives varies significantly. Disrupting an ICS through denial of service is considerably easier than manipulating the process in a designed way that circumvents safety mechanisms. The assessment documents which objectives an adversary could realistically pursue based on the access and knowledge obtainable through the assessed environment.

---

## ICS Attack Difficulty Context

Not all ICS impacts are equal. The assessment frames findings against the difficulty scale described in the ICS Cyber Kill Chain:

| Difficulty | Objective |
|------------|-----------|
| Easier | Compromise ICS security boundary |
| | Exfiltrate ICS information |
| | Disrupt ICS operations |
| | Damage ICS components |
| | Achieve low-confidence process effects |
| Harder | Achieve high-confidence process and/or equipment effects |
| Most Difficult | Execute a repeatable attack with re-attack capability |

This context helps asset owners prioritize remediation by understanding not just what vulnerabilities exist, but how much additional effort and sophistication an adversary would need to translate access into meaningful process impact.

---

## References

- Assante, M.J. and Lee, R.M. (2015). *The Industrial Control System Cyber Kill Chain*. SANS Institute.
- Hutchins, E.M., Cloppert, M.J., and Amin, R.M. (2011). *Intelligence-Driven Computer Network Defense Informed by Analysis of Adversary Campaigns and Intrusion Kill Chains*. Lockheed Martin.
- Lee, R.M. (2015). *The Sliding Scale of Cyber Security*. SANS Institute.