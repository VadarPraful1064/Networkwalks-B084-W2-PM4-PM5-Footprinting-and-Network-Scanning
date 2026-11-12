# Footprinting & Network Scanning Lab (Kali Linux & Windows 11)

An evidence-based cybersecurity lab exercise documenting passive reconnaissance (OSINT) and active local network discovery using **Kali Linux** and **Windows 11** virtual machines on **Oracle VirtualBox**.

> **Disclaimer & Scope:** Only activities visible in the supplied evidence screenshots are documented below. No unshown tests, port scans, or vulnerabilities are claimed. All scanning was restricted to an authorized local lab subnet (`10.0.0.0/24`).

---

## 📋 Lab Metadata

| Field | Details |
| :--- | :--- |
| **Prepared By** | Praful Vadar |
| **Assessment Date** | 9 November 2026 |
| **Report Date** | 12 November 2026 |
| **Modules Covered** | **PM1 / PM4:** Footprinting & Reconnaissance with `theHarvester`<br>**PM5:** Network Scanning & Host Discovery with Zenmap / Nmap |
| **Lab Environment** | Kali Linux + Windows 11 on Oracle VirtualBox |
| **Network Scope** | `10.0.0.0/24` |
| **Assessment Type** | Authorized educational lab exercise |

---

## 🎯 Project Overview

This lab demonstrates foundational reconnaissance and host enumeration workflows across two modules:

| Module | Activity | Target |
| :--- | :--- | :--- |
| **PM1 / PM4** | Passive Footprinting / OSINT tool practice | `microsoft.com` (`theHarvester` CLI test only) |
| **PM5** | Local network scanning & active host discovery | `10.0.0.0/24` |

---

## 🛠️ Tools & Technologies

| Tool | Purpose |
| :--- | :--- |
| **Kali Linux** | Primary security testing and OSINT environment |
| **theHarvester** | OSINT tool for gathering emails, subdomains, hosts, and IPs |
| **Windows CMD / PowerShell** | Verifying local virtual machine network configuration |
| **ipconfig / ipconfig /all** | Inspecting IPv4 address, subnet mask, gateway, DHCP, and DNS settings |
| **Zenmap / Nmap** | Active host discovery and visual network topology mapping |
| **Nmap `-sn`** | Ping scan for live host discovery without performing a full port scan |

---

## 🔍 Module Execution & Findings

### 1. Footprinting & Reconnaissance (`theHarvester`)
* **Platform:** Kali Linux
* **Target Tested:** `microsoft.com`
* **Initial Invocation:** Launching `theHarvester` without required flags displayed standard CLI usage output, confirming tool availability.
* **Baidu Source Query:**
  ```bash
  theHarvester -d microsoft.com -l 50 -b baidu
  ```
* **Result:** The run completed and returned **0 IPs, 0 emails, 0 people, and 0 hosts**. This reflects a limited single-source query result rather than a full external footprint assessment.

---

### 2. Local Network Configuration (Windows 11 VM)
Network parameters were verified on the Windows 11 guest (`window11vm`) via Command Prompt (`ipconfig`) and PowerShell (`ipconfig /all`):

| Parameter | Value |
| :--- | :--- |
| **Host Name** | `window11vm` |
| **Adapter** | `Intel(R) PRO/1000 MT Desktop Adapter` |
| **Physical (MAC) Address** | `08-00-27-84-08-FD` |
| **IPv4 Address** | `10.0.0.4` |
| **Subnet Mask** | `255.255.255.0` |
| **Network CIDR** | `10.0.0.0/24` |
| **Default Gateway** | `10.0.0.1` |
| **DHCP Server** | `10.0.0.2` |
| **DNS Servers** | `8.8.8.8`, `8.8.4.4` |

---

### 3. Active Host Discovery & Topology (Zenmap / Nmap)
* **Target Subnet:** `10.0.0.0/24`
* **Scan Profile:** Ping scan
* **Command Executed:**
  ```bash
  nmap -sn 10.0.0.0/24
  ```
* **Summary:** Scanned **256 IP addresses** in `3.13 seconds`; **3 hosts up**.

| Live Host | MAC Address Shown | Evidence-Based Note |
| :--- | :--- | :--- |
| `10.0.0.1` | `52:54:00:12:35:00` | QEMU virtual NIC; acts as the default gateway per Windows config |
| `10.0.0.2` | `08:00:27:B0:EF:2A` | Oracle VirtualBox virtual NIC; acts as the DHCP server per Windows config |
| `10.0.0.4` | *Not shown in Nmap* | Matches the local Windows 11 VM IPv4 address |

* **Network Topology:** Zenmap's topology graph visualizes the three responding nodes (`10.0.0.1`, `10.0.0.2`, and `10.0.0.4`) connected to `localhost`.

---

## 📊 Observations & Recommendations

| Observation | Recommendation |
| :--- | :--- |
| Three live hosts responded on the `10.0.0.0/24` lab subnet. | Maintain a baseline asset inventory and investigate any unexpected MAC/IP pairs. |
| Windows VM uses `10.0.0.4/24` with gateway `10.0.0.1` and DHCP `10.0.0.2`. | Document VirtualBox network adapter modes (e.g., NAT Network / Host-Only) to ensure proper lab isolation. |
| External DNS resolvers (`8.8.8.8`, `8.8.4.4`) are configured. | Confirm external DNS forwarding is intentional for the lab environment. |
| `theHarvester` query against Baidu returned no records. | Correlate across multiple OSINT sources when performing authorized reconnaissance. |

### Assessment Limitations
* No port scanning (`-sS`/`-sT`), service/version detection (`-sV`), vulnerability scanning, or exploitation was performed.
* Findings are informational baseline observations; no CVSS scores are assigned.

---

## 📁 Repository Structure & Evidence

All original lab screenshots are stored in the `evidence/` directory:

```text
.
├── README.md
├── Footprinting-and-Network-Scanning-Lab-Report.pdf
└── evidence/
    ├── 01-kali-theharvester-initial-run.png
    ├── 02-windows-ipconfig.png
    ├── 03-windows11-virtualbox-console.png
    ├── 04-zenmap-ping-scan.png
    ├── 05-zenmap-topology.png
    ├── 06-zenmap-host-discovery-details.png
    ├── 07-windows-ipconfig-all.png
    └── 08-kali-theharvester-final-run.png
```

| Evidence ID | File Name | Description |
| :--- | :--- | :--- |
| **Evidence 01** | `01-kali-theharvester-initial-run.png` | Initial `theHarvester` CLI launch and usage output in Kali Linux |
| **Evidence 02** | `02-windows-ipconfig.png` | Windows Command Prompt showing basic `ipconfig` output (`10.0.0.4`) |
| **Evidence 03** | `03-windows11-virtualbox-console.png` | Full VirtualBox console view of the Windows 11 VM network check |
| **Evidence 04** | `04-zenmap-ping-scan.png` | Zenmap GUI running `nmap -sn 10.0.0.0/24` |
| **Evidence 05** | `05-zenmap-topology.png` | Zenmap Topology tab and legend displaying the 3 active hosts |
| **Evidence 06** | `06-zenmap-host-discovery-details.png` | Detailed Nmap host discovery output showing MAC addresses and latencies |
| **Evidence 07** | `07-windows-ipconfig-all.png` | Windows PowerShell `ipconfig /all` output showing DHCP, DNS, and MAC details |
| **Evidence 08** | `08-kali-theharvester-final-run.png` | Completed `theHarvester -d microsoft.com -l 50 -b baidu` execution |