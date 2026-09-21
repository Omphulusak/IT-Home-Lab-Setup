# Splunk SOC Dashboards: SSH Brute-Force Monitoring & Windows Event Analysis

## Overview

This phase of the homelab focuses on building and interpreting Splunk dashboards for two common SOC monitoring use cases: SSH brute-force detection on an internet-facing honeypot, and Windows domain event monitoring for privilege and account-change activity.

---

## Dashboards

![Splunk Dashboards Overview](../screenshots/splunk-dashboards-overview.png)

Three dashboards were built in this environment: **SSH activity**, **ssh2**, and **Windows events**.

---

## SSH Activity Dashboard

![SSH Activity Dashboard](../screenshots/splunk-ssh-activity.png)

**Purpose:** Monitor SSH authentication attempts against an exposed host to identify brute-force activity and successful compromises.

**Key findings from this dataset:**

- **Top failed account:** `root` — 240 failed attempts, far exceeding any other account (`andry`, `maven`, `redis`, `test`, `admin` all under 5 attempts each). This is a classic automated brute-force signature: attackers overwhelmingly target `root` first since it grants full system access if successful.
- **Top source IP:** `178.128.225.222` — a single IP responsible for the majority of successful root logins observed in the dataset, appearing repeatedly within the same short time window (19:36–19:41).
- **Successful logins by user:** Despite `root` receiving the most failed attempts, the successful login table shows multiple **successful root logins** from this same source IP in rapid succession — a strong indicator that this specific brute-force attempt succeeded, not just that it was attempted.
- **Network activity map:** Global distribution of source traffic, with concentrated activity visible across North America and parts of Europe/Asia, consistent with widely distributed scanning infrastructure rather than a single geographic origin.

**Analysis / What this would mean in a real SOC:**
A successful root login immediately following a high-volume failed-attempt pattern from the same source IP is a high-confidence indicator of a successful brute-force compromise (MITRE ATT&CK T1110 – Brute Force). In a live environment, this would warrant immediate isolation of the host, forced credential rotation, and a review of any activity performed under the `root` account following the successful login timestamp.

---

## Windows Events Dashboard

![Windows Events Dashboard](../screenshots/splunk-windows-events.png)

**Purpose:** Monitor a Windows domain environment for successful logins, privileged account usage, new account creation, and firewall configuration changes.

**Key findings from this dataset:**

- **Successful logins by user:** The account `kporter` shows repeated successful logons across multiple hosts (`ADDC01.elnath.com`, `Desktop-IT.elnath.com`), including logon type 3 (network) and logon type 11 (cached credentials).
- **Admin privilege usage:** `kporter` also appears in the "users logon with admin privileges" panel with **22 recorded admin logons** — worth cross-referencing against whether this account should routinely hold administrative rights, or whether this volume of privileged access is unusual for its normal role.
- **New user account created:** A new account named `evil` was created by `kporter` on `Desktop-IT.elnath.com`. An administrative account creating a new account with an overtly suspicious name is a strong red flag — in a real investigation this would immediately escalate to a full incident, since it could indicate a compromised admin credential being used to establish persistence (MITRE ATT&CK T1136 – Create Account).
- **Firewall exceptions added:** Multiple firewall rule exceptions were added on the same host around the same time, associated with Microsoft Store app packages (MSPaint, Windows Photos). Firewall exceptions added in a tight time cluster alongside a suspicious account creation is worth correlating — either coincidental application updates, or an attacker creating outbound access paths for follow-on activity.

**Analysis / What this would mean in a real SOC:**
Taken together — a privileged account performing high-volume admin logons, followed by creation of a new account named `evil`, followed by firewall rule changes — this sequence maps to a plausible privilege escalation and persistence chain. The investigative next step would be to pull the full logon history for `kporter` prior to this event to determine whether the account itself was compromised, and to check whether the `evil` account was used for any subsequent authentication.

---

## Skills Demonstrated

- SIEM dashboard design and interpretation (Splunk)
- Brute-force detection and successful-compromise identification
- Windows Security Event analysis (logon types, privileged access, account creation)
- Correlating multiple log sources into a single incident narrative
- MITRE ATT&CK technique mapping (T1110, T1136)
