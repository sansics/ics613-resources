# Physical Site Analysis Checklist

[← Back to README](../README.md)

---

Physical security protections are trusted in most process environments to prevent, limit, and detect unauthorized access. Unfortunately, over time, these protections naturally degrade due to environmental conditions or alert fatigue. Keeping threat actors away from physical devices and network infrastructure is critical to protecting the assets, their communications, and the safety of all personnel, including the threat actors themselves.

This checklist supports periodic review of physical security mechanisms at an operational site. It is referenced in [SANS ICS613: ICS/OT Penetration Testing & Assessments](https://www.sans.org/cyber-security-courses/ics-ot-penetration-testing-assessments) as part of the bottom-up assessment methodology, where consultants evaluate the physical protections that the cybersecurity program depends on. Many of the technical findings identified during an ICS/OT penetration test are only exploitable because an attacker can physically reach the device, cabinet, or network port. This checklist helps assessors and asset owners identify those gaps before an adversary does.

An Obsidian template version of this checklist is available in the [config/templates](../config/templates/) directory for use during field walkthroughs with [Offensive Obsidian](https://github.com/zyenai/offensive_obsidian).

## Review Frequency

Physical security protections need to be periodically reviewed. The appropriate frequency depends on applicable regulations and the criticality of the assets being protected.

- **Monthly** reviews may be appropriate for high-criticality environments or those under active threat.
- **Biannual** reviews align with many regulatory frameworks and seasonal environmental changes.
- **Annual** reviews represent a minimum baseline for most operational environments.

## Stakeholders

The following teams should be involved in the physical site analysis. Coordination across all three is important because physical security events often have cybersecurity implications that go unreported when these teams operate independently.

- **Physical Security Team** owns the cameras, fences, locks, and access control systems. They conduct the walkthroughs and respond to unauthorized access events.
- **OT Operations Team** owns the control cabinets, network racks, and process equipment. They know what should and should not be present in the environment.
- **Cybersecurity Team** needs to be notified when physical security events occur because physical access to devices and network infrastructure can enable or indicate a cyberattack.

---

## Cameras

Camera systems provide both deterrence and forensic evidence. The connection type matters because an Ethernet-connected camera that can be unplugged gives an attacker a live network port, and a wireless camera on the process network introduces an unintended wireless bridge into the control system.

- Verify all cameras are working and cover each gate and building door.
- Identify the connection type for each camera: coaxial, Ethernet, or wireless.
- For Ethernet-connected cameras, check if the cable can be unplugged and the port used for unauthorized network access.
- For wireless cameras, confirm the wireless network is secured and is not on the process network.
- Confirm camera recordings can be exported and saved for forensic review.

## Perimeter

The perimeter is the first physical boundary an attacker must cross. Erosion, vegetation growth, and wear on gate hardware can quietly reduce its effectiveness over time.

- Walk the full fence line and verify all gates are locked.
- Check for low points where erosion makes it easy to move under the fence.
- Check for exterior objects (trees, walls, stacked equipment) that help individuals get over the fence.
- Check if gate exit mechanisms can be operated from outside the fence line to unlock the gate.

## External Cabinets

External control cabinets are high-value targets because they often contain PLCs, network switches, and field devices that are outside the building's access controls. These are also where environmental hazards like bee and wasp nests accumulate, so use caution when opening cabinets.

- Confirm each external control cabinet is locked.
- Review each cabinet for tamper tape or sensors that indicate the door has been opened.
- Review each cabinet for network diagrams, process documentation, or credential information. These should not be stored in external cabinets.
- Review the devices in each cabinet to ensure their physical security mechanisms are enabled (e.g., PLC key switches set to RUN, not PROGRAM).

## Building Access

Building entry points include doors, door frames, and roof access. Shifted foundations can expose door throws to simple bypass tools, and roof hatches are frequently overlooked during physical security reviews.

- Check each door into buildings to ensure it locks.
- Check each door frame to ensure the throw is protected and prevents under-the-door and over-the-door bypass tools. Shifted foundations can expose throws to tampering.
- Check doors for tamper tape or sensors that indicate the door has been opened.
- Check buildings for ladders that lead to roof access.
- Verify roof access points (doors, hatches) are locked and determine if tamper tape or sensors are used to indicate they have been opened.

## Internal Cabinets and Network Racks

Control cabinets, network racks, and server racks inside buildings are where the most critical assets live. Physical walkthroughs to check for unauthorized devices are one of the most effective controls against persistent threats, particularly those that use rogue network implants.

- Check control cabinets, network racks, and server racks to ensure doors are installed, locks are available and in use, and determine if tamper tape or sensors indicate when doors have been opened.
- For critical cabinets or closets, determine if cameras are used to monitor and record access.
- Determine if physical walkthroughs are regularly conducted to review control cabinets, network racks, and server racks for unauthorized devices and network connections.

## General

These items address site-wide practices that span multiple physical areas.

- Review buildings for unexpected wireless antennas. Cellular modems on generators and compressors are common, often installed by vendors or integrators for remote maintenance without going through the site's change management process.
- Review site personnel and physical security team unauthorized access checklists to determine if the cybersecurity team is notified when these events occur. Physical access events should trigger a review of nearby network and control assets for signs of tampering.