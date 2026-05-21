# Pre-Requisites and Course Progression

[← Back to README](../README.md)

---

## Pre-Requisites

ICS613 is an advanced course. Students should arrive with a working knowledge of both IT offensive security and ICS/OT fundamentals. At minimum, the following are expected:

- Familiarity with Windows and Linux command-line environments
- Understanding of TCP/IP networking and common protocols
- Basic knowledge of Active Directory concepts (domains, trusts, group policy)
- Exposure to penetration testing tools and methodologies
- Understanding of industrial control system architectures and terminology (PLC, DCS, HMI, RTU)

## Recommendations

- **Second Monitor:** Strongly recommended. The course involves simultaneous reference to course material, lab instructions, and virtual machine environments. A second monitor significantly improves the lab experience.
- **Note-Taking Tool:** A structured tool such as [Obsidian](https://obsidian.md/), CherryTree, or OneNote for organizing assessment findings during labs and the capstone. See [Offensive Obsidian](https://github.com/zyenai/offensive_obsidian) for a quick pentest note-taking configuration.
- **Familiarity with Virtualization:** Course labs use virtual machines. Prior experience with VMware Workstation or VirtualBox will help.

## Laptop Requirements

Review the official [ICS613 laptop requirements](https://www.sans.org/cyber-security-courses/ics-ot-penetration-testing-assessments#faqs) on the SANS course page.

---

## Suggested Course Progression

ICS613 sits at the intersection of offensive security and ICS/OT domain knowledge. The progression below builds the necessary foundation across both tracks before converging on ICS/OT penetration testing.

### Offensive Security Track

Builds from network monitoring through enterprise penetration testing.

| Order | Course | Title | Focus |
|:-----:|--------|-------|-------|
| 1 | [SEC503](https://www.sans.org/cyber-security-courses/network-monitoring-threat-detection-in-depth/) | Network Monitoring and Threat Detection In-Depth | Network traffic analysis and intrusion detection |
| 2 | [SEC504](https://www.sans.org/cyber-security-courses/hacker-techniques-incident-handling/) | Hacker Tools, Techniques, and Incident Handling | Attack techniques and incident response |
| 3 | [SEC560](https://www.sans.org/cyber-security-courses/enterprise-penetration-testing/) | Enterprise Penetration Testing | Enterprise pentesting methodology |

### ICS/OT Security Track

Builds ICS/OT domain knowledge from fundamentals through active defense.

| Order | Course | Title | Focus |
|:-----:|--------|-------|-------|
| 1 | [ICS310](https://www.sans.org/cyber-security-courses/ics-scada-cyber-security-essentials/) | ICS/SCADA Security Essentials | ICS security fundamentals |
| 2 | [ICS410](https://www.sans.org/cyber-security-courses/ics-scada-security-essentials/) | ICS/SCADA Security Essentials | ICS security operations and defense |
| 3 | [ICS612](https://www.sans.org/cyber-security-courses/ics-cybersecurity-in-depth/) | ICS Cybersecurity In-Depth | Deep ICS security analysis |

### Convergence

| Course | Title | Focus |
|--------|-------|-------|
| **[ICS613](https://www.sans.org/cyber-security-courses/ics-ot-penetration-testing-assessments/)** | **ICS/OT Penetration Testing & Assessments** | **ICS/OT offensive assessments** |

### Continuing Education

| Course | Title | Focus |
|--------|-------|-------|
| [ICS515](https://www.sans.org/cyber-security-courses/industrial-control-system-visibility-detection-response/) | ICS Visibility, Detection, and Response | ICS active defense and incident response |

> **Note:** Not all courses are strictly required. Students with strong backgrounds in either offensive security or ICS/OT may skip earlier courses in the track they already know. At minimum, students should have equivalent knowledge to SEC504 or SEC560 **and** ICS410 before attending ICS613.