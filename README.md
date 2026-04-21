# -Project-CyberTr0n-End-to-End-SIEM-Architecture-Threat-Emulation

# Executive SummaryProject 
CyberTr0n is a high-fidelity Security Operations Center (SOC) Proof of Concept (POC) designed to detect, analyze, and mitigate multi-stage cyber attacks within a hybrid-infrastructure environment. By integrating Splunk Enterprise with a distributed network of Windows and Ubuntu endpoints, the project demonstrates a robust defense-in-depth strategy. It specifically addresses the "Signal-to-Noise" crisis by correlating low-level system telemetry with high-level behavioral patterns to reduce analyst fatigue and improve Mean Time to Detect (MTTD).

# Technical Architecture
The project utilizes a hardened VirtualBox environment with isolated networking to simulate a corporate DMZ.
- SIEM Node: Ubuntu Server running Splunk Enterprise as the central log aggregator.
- Victim Node: Windows 10 Pro endpoint, initially configured with a "0 lockout threshold" for testing purposes.
- Attacker Node: Kali Linux positioned on the same subnet to simulate an internal "Assume Breach" scenario.
- Data Pipeline: Deployment of Universal Forwarders (UF) capturing Windows Event Logs (Security/System) and Performance Metrics (Perfmon) over secure TCP Port 9997.

#  Adversary Emulation & Detection
The environment was tested against a multi-stage attack mapped to the Cyber Kill Chain and MITRE ATT&CK frameworks:
Phase Tactic  Technique  Tooling
I  Credential AccessBrute Force (T1110.001)smbclient 
II ImpactNetwork Service DoS (T1499.002)hping3

The "Smokescreen" Attack
The project successfully emulated a SYN Flood (using hping3) intended to mask a Brute Force login attempt. Detection logic was validated through the CyberTr0n Command Center, a real-time Splunk dashboard that correlates concurrent system stress (85%+ CPU/Network saturation) with authentication failures (Event IDs 4625/4624).

# Incident Response (NIST SP 800-61 r2)
Following the identification of a "True Positive" breach, the following containment and hardening steps were executed:
1. Network Containment: Implemented a PowerShell host-based firewall rule to block the attacker's IP (10.55.23.231) and terminate the SYN flood.
2. Identity Recovery: Forced a password reset for the compromised CyberTr0n_Admin account to rotate credentials and eject the attacker.
3. System Hardening: Updated the Windows account lockout policy from 0 to 5 attempts to move the system from a vulnerable to a hardened state.

# Key Outcomes & Business Impact
- 75% Reduction in Investigation Time: Custom SPL queries transform raw logs into actionable "5W1H" incident tickets (Who, What, When, Where, Why, How).
- Operational Visibility: Achieved 100% visibility during the breach by mapping performance metrics against authentication events.
- Reduced Breach Cost: Early detection allows for containment before lateral movement or ransomware deployment can occur.

# Future Roadmap

SOAR Automation: Implementing automated playbooks to trigger firewall blocks instantly upon detecting 10+ failed attempts.

Cloud Integration: Extending log ingestion to Azure Arc to simulate hybrid-cloud monitoring.

MFA Implementation: Integrating Multi-Factor Authentication to further harden administrative access.

# Developed By: Orinea Mulaudzi Date: 2026-04-04
