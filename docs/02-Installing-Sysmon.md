# Installing Sysmon on Windows 10

This guide documents how I installed **Sysmon** on the Windows 10 machine in my cybersecurity homelab.

Sysmon is part of Microsoft Sysinternals and provides detailed endpoint logging for security monitoring and threat detection.

---

## Why Sysmon?

Sysmon helps collect useful Windows security events such as:

- Process creation
- Network connections
- File creation
- Registry changes
- Driver loading
- DNS queries
- Process access

These logs are useful for SOC analysis, detection engineering, and incident response.

---

## Step 1 — Download Sysmon

Download Sysmon from Microsoft Sysinternals:

https://learn.microsoft.com/sysinternals/downloads/sysmon

---

## Step 2 — Extract Sysmon

After downloading Sysmon, extract the files on the Windows 10 virtual machine.

<p align="center">
<img src="screenshots/sysmon-installation.png" width="850">
</p>

**Figure 1:** Sysmon files extracted on the Windows 10 endpoint.

---

## Step 3 — Open PowerShell as Administrator

Open PowerShell as Administrator.

Navigate to the Sysmon folder:

```powershell
cd Downloads\Sysmon
```

---

## Step 4 — Install Sysmon

Install Sysmon using the configuration file:

```powershell
.\Sysmon64.exe -accepteula -i sysmonconfig.xml
```

If no custom config is available, Sysmon can also be installed with:

```powershell
.\Sysmon64.exe -accepteula -i
```

---

## Step 5 — Verify Sysmon Service

Check that Sysmon is running:

```powershell
Get-Service Sysmon64
```

---

## Step 6 — View Sysmon Logs

Open Event Viewer and navigate to:

```text
Applications and Services Logs
Microsoft
Windows
Sysmon
Operational
```

---

## Next Step

Sysmon logs can later be forwarded to tools such as:

- Wazuh
- Elastic Stack
- Splunk
- TheHive
- Shuffle SOAR
