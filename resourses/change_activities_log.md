# Change Activities Log

[← Back to README](../README.md)

---

## Overview

The Change Activities Log tracks all system and application modifications made during a penetration test or security assessment. Every change introduced to the target environment must be recorded here so it can be rolled back by the assessment team or followed up on by an administrator if the team cannot reverse the change themselves.

This log is a living document maintained throughout the engagement. It should be updated in real time as changes occur, not reconstructed after the fact.

## What Constitutes a Change

Any action that alters the state of a system or application in the target environment is a change. This includes but is not limited to:

- Adding, modifying, or disabling user accounts or credentials
- Uploading files, scripts, tools, or payloads to target systems
- Modifying system or application configurations (firewall rules, registry keys, service settings, PLC logic, HMI configurations)
- Enabling or disabling features, services, or protocols
- Installing software, agents, implants, or services
- Updating, inserting, or deleting data in databases
- Creating scheduled tasks, cron jobs, or persistence mechanisms
- Modifying network configurations (routes, DNS, VLAN assignments)
- Changes to logging or monitoring configurations

In ICS/OT environments, pay particular attention to changes that affect process control, safety instrumented systems, and historian data. Even read-only tools can sometimes alter device state through protocol interactions, so document anything that could affect system behavior.

## Change Activities Log

| Date/Time | System/Application Name | System/Application IP Address | Action Description | Change Description | Change Correction Description | Correction Date/Time | Confirmation Signature |
|-----------|------------------------|-------------------------------|--------------------|--------------------|-------------------------------|----------------------|------------------------|
| | | | | | | | |
| | | | | | | | |
| | | | | | | | |
| | | | | | | | |
| | | | | | | | |
| | | | | | | | |
| | | | | | | | |
| | | | | | | | |
| | | | | | | | |
| | | | | | | | |

## Field Descriptions

- **Date/Time** - When the change was made (use UTC and include timezone offset if local time is also recorded, e.g., 2025-03-15 14:30 UTC)
- **System/Application Name** - Hostname, application name, or device identifier of the affected system (e.g., HMI-01, DCS-ENG-WS, Historian Server)
- **System/Application IP Address** - IP address of the affected system (include port if relevant to the change)
- **Action Description** - The category of action taken (e.g., account creation, file upload, configuration change, software installation, database modification)
- **Change Description** - Specific details of what was changed, including original values where known (e.g., "Created local account 'pentest01' with administrator privileges" or "Uploaded nmap binary to C:\Temp\nmap.exe")
- **Change Correction Description** - Steps required to reverse the change (e.g., "Delete local account 'pentest01' via net user pentest01 /delete" or "Remove file C:\Temp\nmap.exe and verify deletion")
- **Correction Date/Time** - When the rollback was performed, or "Pending" if not yet completed
- **Confirmation Signature** - Initials or name of the person who verified the rollback was successful, or the administrator assigned to follow up

## Usage Notes

Maintain one log per engagement. If the assessment spans multiple sites or environments, use separate tables per site or clearly label entries with the site name.

At the end of each assessment day, review the log to confirm all changes that can be reversed have been. For changes that require administrator follow-up (e.g., removing accounts from Active Directory when the team lacks domain admin access), flag the Correction Date/Time as "Pending - Admin Required" and brief the client point of contact.

This log should be included in the final report deliverables and reviewed during the outbrief to confirm all items have been addressed or assigned for follow-up.