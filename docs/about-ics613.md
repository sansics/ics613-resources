# About ICS613

[← Back to README](../README.md)

---

## Course Overview

ICS613: ICS/OT Penetration Testing & Assessments is a five-day, hands-on course that trains security professionals to safely and effectively conduct penetration tests and security assessments in industrial control system environments. This is not a course about running enterprise pentest tools against flat networks - it is about understanding how industrial operations work, what matters most to the people who run them, and how to assess the security of environments where the consequences of mistakes are measured in safety incidents, environmental releases, and operational shutdowns.

The course is built around a realistic engagement scenario that follows students from initial bench testing and planning, through an active top-down penetration test of the enterprise-to-OT path, into a bottom-up assessment of the control systems protecting a physical process. Every phase is aligned to the [SANS ICS Cyber Kill Chain](https://www.sans.org/white-papers/36297/), which means students learn to model their assessments after real-world threat actors - the same actors that are already inside critical infrastructure networks. Groups like Volt Typhoon have demonstrated that patient, stealthy operators can pre-position themselves deep within utility and infrastructure networks using nothing more than native tools and stolen credentials, while groups like CyberAv3ngers have shown that even unsophisticated attacks against internet-exposed PLCs can compromise water systems and force operators into manual control. ICS613 teaches students to think and operate the way these adversaries do - so they can find the paths and weaknesses before someone else does.

Students leave the course with practical experience conducting ICS/OT assessments, the methodology to plan and execute them safely, and the communication skills to deliver results that actually improve operational resilience.

---

## Welcome to 613LNG, LLC

![Ronnie MacDonnie](../attachments/ics613_Ronnie_MacDonnie.png)

Meet **Ronnie MacDonnie**, Engineering Manager at **613LNG, LLC**.

Ronnie is good at what he does. He keeps the plant running, his operators trust him, and when leadership told him to "go figure out cybersecurity," he rolled up his sleeves and got to work. He has spent months researching hardening guides, tightening configurations, and pushing his engineering team to make the environment more secure. He has done okay - better than most, given that he is working without dedicated cybersecurity expertise on staff.

But Ronnie knows what he does not know. He has made changes to the environment and he needs someone to tell him if those changes actually work. He needs subject matter experts who understand both offensive security and industrial operations to come in, assess what his team has built, and give him honest, actionable feedback. That is where you come in.

You are a consultant with **SixOneThree Security, Inc.**, and 613LNG has hired your team to evaluate their environment. Ronnie is your primary point of contact. He is collaborative, he wants to learn, and he will give you the access you need - but he is also protective of his operations, and he expects you to be careful. This is a production environment. Things break when you are careless.

### The Operation

613LNG, LLC operates **LNG Onshore Terminals** deployed at sites around the world. These terminals are built around **Automation Direct CLICK PLC PLUS** controllers running custom I/O boards, and they serve as your introduction to the engagement. You will get hands-on with these devices during bench testing - pulling apart firmware, reviewing device configurations, analyzing protocols, probing for vulnerabilities, and understanding how the devices communicate before you ever touch a production network. Every student gets their own terminal (the student kit) to work with throughout the course.

The crown jewel of 613LNG's operation is the **LNG Liquefaction Plant**, a full-scale industrial process controlled by an **ABB 800xA Distributed Control System (DCS)**. The plant is augmented with **Rockwell PLCs** running **PowerFlex Variable Frequency Drives** that control **permanent magnet (PM) motors** - real industrial equipment with real physics. This is not a simulation running in a virtual machine. The DCS is live, the drives are spinning, and the process has consequences.

---

## What You Will Actually Do

### Bench Testing - Know Your Target

Before you touch the client network, you get to know the technology. The LNG Onshore Terminal student kits put real ICS hardware in your hands. You will tear into device configurations, extract and analyze firmware, map out communication protocols (known and unknown), and identify vulnerabilities - all in a controlled lab environment where you can be aggressive without operational risk. This matters because the kinds of weaknesses you will find - default credentials, exposed services, unauthenticated protocols - are exactly what gets exploited in the real world. When CyberAv3ngers compromised water utilities across the United States, they did not need zero-days. They walked in through default passwords on internet-exposed PLCs. Bench testing is where you learn to find those issues before an adversary does.

### Top-Down Penetration Test - Find the Path

Ronnie's team has built layered defenses between the enterprise network and the OT environment. Your job is to find out if they hold.

Starting from an assumed breach foothold in the enterprise, you will work your way through the layers - remote access infrastructure, jump hosts, segmentation boundaries - toward the systems and workstations that manage the DCS. The focus here is on Living-off-the-Land (LotL) techniques. You are not loading up Kali and firing off exploit frameworks. You are using what the environment already has - Windows native binaries, built-in OS commands, PowerShell, RDP, and the OT vendor software installed on engineering and operator workstations. Dedicated cybersecurity tools are used sparingly, only when they genuinely save time or accomplish something the native environment cannot. This is how real adversaries operate, and it is how effective ICS/OT assessments are conducted.

Along the way, you will practice the skills that matter in converged IT/OT environments: credential theft using native Windows tools, privilege escalation through misconfigurations, persistence via built-in mechanisms, pivoting through legitimate remote access paths, and leveraging OT-specific software for information gathering and process interaction. Every step is documented, and every technique maps to something an operator could do with nothing more than a valid set of credentials and the tools already installed.

This phase follows the top-down methodology aligned to the ICS Cyber Kill Chain. You are modeling real adversary behavior - the same approach that Volt Typhoon used to burrow into U.S. energy, water, and communications infrastructure without deploying custom malware. They moved through enterprise networks using native operating system tools, harvested credentials, and maintained persistent access for years before being discovered. The top-down phase of ICS613 teaches you to follow that same kill chain, testing whether the defenses Ronnie's team has put in place would detect or stop an operator with that level of patience and discipline.

### The Shift - When Offense Meets Operations

There is a moment in every ICS/OT assessment where the rules change. You have worked your way through the enterprise, past the jump hosts, and now you are looking at systems that touch the physical world. This is where traditional pentesters get people hurt, and this is where ICS613 teaches you to shift your approach.

When your access gets close to live production systems, you transition from the top-down penetration test into a **bottom-up assessment methodology**. Instead of "what can I compromise next," the question becomes "what are the physical consequences of compromise, and are the protections adequate?"

### Bottom-Up Assessment - Understand the Consequences

The bottom-up methodology flips the perspective. You are no longer thinking like an attacker climbing toward the target - you are thinking like an engineer who needs to understand what could go wrong at the process level and whether the cyber-physical protections actually prevent it.

You will work through the configuration of the LNG lab environment, understanding the physical impacts that the process safety systems are designed to prevent. You will evaluate the cyber connectivity of devices, systems, and data flows to understand risks to operations and safety that a threat actor could leverage. This is not theoretical - CyberAv3ngers expanded from targeting Unitronics PLCs at water utilities to exploiting authentication bypass vulnerabilities in Rockwell Automation controllers, the same family of controllers running in the 613LNG environment. The bottom-up methodology teaches you to evaluate whether the protections around those controllers would hold, and what happens to the physical process if they do not. You will develop realistic attack scenarios with expected physical consequences, and where safe to do so, you will demonstrate them.

This is also where the assessment maps directly to **ISA/IEC 62443**. The passive and active information gathering techniques you use during the bottom-up phase translate directly to both high-level gap assessments and detailed vulnerability assessments under the standard. You are not just finding vulnerabilities - you are producing output that Ronnie and his leadership can use to make risk-informed decisions and demonstrate compliance.

### Capstone - Put It All Together

The course culminates in a capstone event that pulls everything together. You will apply the full methodology - bench testing knowledge, top-down offensive skills, and bottom-up operational analysis - against the complete 613LNG environment. The capstone is scored, competitive, and the top finishers earn the **ICS613 Challenge Coin**.

---

## Course Sections at a Glance

| Section | Title | What You Will Do |
|:-------:|-------|------------------|
| 1 | Bench and Lab Testing | Hardware analysis, firmware extraction, protocol analysis, and custom tool development on the student kit |
| 2 | Preparing for ICS/OT Assessments | Crown Jewel Analysis, assessment planning, stakeholder engagement, and the LNG process environment |
| 3 | Top-Down Active Methodology | Assumed breach, credential attacks, privilege escalation, lateral movement, and pivoting through IT/OT boundaries |
| 4 | Security and Vulnerability Assessment | Passive analysis, network traffic review, host analysis, Active Directory assessment, and ISA/IEC 62443 alignment |
| 5 | Bottom-Up Operations Assessment and Capstone | Attack scenario development, process impact analysis, adversarial demonstrations, and the scored capstone |

---

## Who Is This Course For

ICS613 is designed for professionals who are - or want to be - responsible for assessing the security of industrial environments. That includes OT security engineers, IT pentesters moving into ICS, ICS security analysts, red team operators working critical infrastructure engagements, and consultants delivering ICS security assessments.

You do not need to be an ICS engineer. You do not need to be an expert pentester. But you need a foundation in both. See the [Pre-Requisites and Course Progression](prerequisites-and-progression.md) page for the recommended training path.

---

## Course Details

- **Course:** [ICS613: ICS/OT Penetration Testing & Assessments](https://www.sans.org/cyber-security-courses/ics-ot-penetration-testing-assessments)
- **Duration:** 5 days (30 hours), 27 hands-on labs
- **Skill Level:** Advanced
- **CPEs:** 30
- **Authors:** [Jason Dely, Tyler Webb, Don C. Weber](authors.md)
- **Format:** In-person or virtual, instructor-led
