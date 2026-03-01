
## Use Case name

PowerShell Attack Simulation (Download + Encoded Command + Hidden Execution)
## Objective

Simulate common PowerShell attacker behaviour and confirm Wazuh can detect it using **Sysmon (EID 1)** and **PowerShell Script Block Logging (EID 4104)**.

## Environment
- **SIEM:** Wazuh (Ubuntu VM)
- **Endpoint:** Windows 11 (Wazuh agent)
- **Telemetry:** Sysmon + PowerShell Operational logs

---

## What I simulated

### 1) Download / staging (Ingress Tool Transfer)

**Command**

```powershell
Invoke-WebRequest -Uri https://example.com/ -OutFile $env:TEMP\stage6.txt
```

**What I saw in Wazuh**

- Logged as **PowerShell Event ID 4104**
- The command appeared in: `data.win.eventdata.scriptBlockText`
- My custom rule **100200** fired successfully (keyword match on download tooling like `Invoke-WebRequest`)

**MITRE**

- **T1105 – Ingress Tool Transfer**

---

### 2) Encoded PowerShell command (obfuscation pattern)

**What I did**

- Built a simple command (`whoami; hostname; Get-Date`)
- Encoded it in **UTF-16LE Base64** (CyberChef)
- Ran it using `-EncodedCommand`

**What I saw in Wazuh**

- Captured in process telemetry (Sysmon EID 1 / command line)
- `-EncodedCommand` was visible in the command line
- I decoded it in CyberChef to confirm the content was benign

**MITRE**

- **T1059.001 – PowerShell**
- **T1027 – Obfuscated/Compressed Information (EncodedCommand pattern)**

---

### 3) Suspicious execution from TEMP + hidden window

**What I did**
- Executed PowerShell from a user-writable TEMP path with a hidden window style (typical staging/execution pattern)

**What I saw in Wazuh**
- Logged as **Sysmon Event ID 1 (Process Create)**
- Built-in Wazuh rule **92029** fired:
    - **“Powershell executed script from suspicious location”**
- Key indicators in the event:
    - `commandLine` contained: `-WindowStyle Hidden` and a `...\AppData\Local\Temp\...` path
    - Parent context showed Task Scheduler service style execution (`svchost.exe ... -s Schedule`)

**MITRE**
- **T1059.001 – PowerShell**
- **T1053.005 – Scheduled Task/Job (context observed via parent)**

---

## Main takeaway

- **Telemetry is working**: I got solid visibility from both **4104** (what was typed) and **Sysmon EID 1** (what executed).
- **Custom rule worked** for download/staging (`100200`).
- **Built-in rule worked** for suspicious PowerShell execution from TEMP (`92029`).
- The hardest part wasn’t PowerShell — it was **writing Wazuh local rules correctly**:
    - XML structure matters (only `<group>` at root)
    - Rule chaining depends on the **actual rule.id firing in my environment**, not the one I assume

---

## Conclusion

I successfully simulated and detected:

- **PowerShell download behaviour**
- **EncodedCommand usage**
- **Hidden execution from TEMP**

Wazuh produced alerts that matched real-world attacker patterns, and I validated them by checking the exact command line/script block content and decoding encoded payloads where needed.
