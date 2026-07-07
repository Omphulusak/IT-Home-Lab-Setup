# Building My Cybersecurity Homelab

This guide documents the process of building my cybersecurity homelab using Oracle VirtualBox.

---

# Lab Overview

The lab consists of three virtual machines.

| Machine | Purpose |
|----------|----------|
| Windows 10 | Target Workstation |
| Kali Linux | Attacker Machine |
| Windows Server 2026 | Active Directory (Future Expansion) |

<p align="center">
<img src="../screenshots/virtualbox-overview.png" width="900">
</p>

*VirtualBox Manager displaying the homelab.*

---

# Tutorials Followed

Special thanks to the creators of the tutorials that helped me build this lab.

- Jon Good – *How to Setup an Isolated Network Using VirtualBox*
- *(Add your remaining tutorial links here.)*

---

# Downloads

| Software | Download |
|----------|----------|
| Oracle VirtualBox | https://www.virtualbox.org/wiki/Downloads |
| Windows 10 ISO | https://www.microsoft.com/software-download/windows10 |
| Kali Virtual Machine | https://www.kali.org/get-kali/#kali-virtual-machines |
| VC++ Redistributable (x64) | https://aka.ms/vs/17/release/vc_redist.x64.exe |
| VC++ Redistributable (x86) | https://aka.ms/vs/17/release/vc_redist.x86.exe |

---

# Step 1

Install Oracle VirtualBox.

Launch the application and verify it opens successfully.

---

# Step 2

Download:

- Windows 10
- Kali Linux VirtualBox Image

---

# Step 3

Create the Windows virtual machine.

- Click **New**
- Select the Windows ISO
- Allocate RAM
- Create a virtual hard disk
- Install Windows

---

# Step 4

Import Kali Linux.

- Download VM image
- Extract archive
- Import appliance
- Boot Kali

---

# Step 5

Configure Virtual Networking.

Use an **Internal Network** named:

```

intnet

```

---

# Step 6

Assign Static IP Addresses.

| Machine | IP |
|----------|-----|
| Kali Linux | 192.168.56.10 |
| Windows 10 | 192.168.56.20 |

---

# Step 7

Verify communication.

Kali:

```bash
ping 192.168.56.20
```

Windows:

```powershell
ping 192.168.56.10
```

---

The virtual machines can now communicate while remaining isolated from external networks.
