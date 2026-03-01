## Wazuh Detection Lab (Sysmon + PowerShell Attacks)

This project is a hands-on detection lab using Wazuh to simulate real-world attacker behaviour and validate detection capabilities.
The focus was on endpoint telemetry, detection engineering, and alert investigation using Sysmon and PowerShell logs.
Built as part of my cybersecurity portfolio to demonstrate blue team and SOC analyst skills.

<img width="2558" height="1269" alt="Screenshot 2026-02-24 142422" src="https://github.com/user-attachments/assets/4f2bb185-89d5-4aa3-b2fe-367b634efd5f" />

## Architecture
Windows 11 Endpoint (Sysmon + PowerShell Logs)
→ Wazuh Agent
→ Wazuh Manager (Ubuntu VM)
→ Wazuh Dashboard (Elastic Stack)

<img width="2559" height="1272" alt="Screenshot 2026-02-24 142344" src="https://github.com/user-attachments/assets/5b481d66-6299-4d6d-9eb1-21a4180e7e53" />

## What I Implemented
- Wazuh SIEM deployed on Ubuntu VM
- Windows 11 endpoint with Wazuh agent + Sysmon
- PowerShell Script Block Logging enabled (Event ID 4104)
- Custom Wazuh detection rules (e.g. rule 100200)
- Log analysis using Wazuh Discover dashboard
- Detection validation using simulated attacker techniques

## Attack Simulations
- PowerShell download (Invoke-WebRequest)
- EncodedCommand execution (Base64 obfuscation)
- Hidden PowerShell execution from TEMP directory
- Application Shimming (sdbinst.exe persistence technique)

<img width="2559" height="1270" alt="Screenshot 2026-02-24 144011" src="https://github.com/user-attachments/assets/3b4905ea-9dea-4ee4-a4b7-38f7061e4f38" />

## Skills Demonstrated
- SIEM: Wazuh
- Endpoint Logging: Sysmon, PowerShell (EID 4104)
- Detection Engineering (custom rules)
- Log Analysis & Investigation
- MITRE ATT&CK mapping
- Threat Simulation

## Key Takeaway
This lab showed that detection is not just about alerts, it's about validating telemetry, understanding attacker behaviour, and tuning rules to reduce false positives while maintaining visibility.

## Next Steps
- Expand detection rules (credential dumping, lateral movement)
- Improve rule tuning and reduce false positives
- Build alert dashboards and correlations
- Integrate with MITRE ATT&CK visualisation

## Use Cases
- [PowerShell Attack Simulation](./use-cases/powershell-attack.md)
- [Application Shimming Detection](./use-cases/application-shimming.md)
