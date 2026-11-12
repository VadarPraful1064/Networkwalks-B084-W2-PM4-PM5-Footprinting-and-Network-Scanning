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

## 🔍 Module Execution & Evidence

### 1. Footprinting & Reconnaissance (`theHarvester`)
* **Platform:** Kali Linux (`theHarvester 4.11.1`)
* **Target Tested:** `microsoft.com`
* **Initial Invocation:** Launching `theHarvester` without required flags displayed standard CLI usage output, followed by an `-b all` run showing missing API key notices for third-party sources.

![Initial theHarvester run in Kali Linux](evidence/01-kali-theharvester-initial-run.png)

* **Baidu Source Query:**
  ```bash
  theHarvester -d microsoft.com -l 1000 -b baidu
  ```
* **Result:** Returned **0 IPs, 0 emails, 0 people**, and **3 hosts** (`c.urs.microsoft.com`, `copilot.microsoft.com`, `hxd.research.microsoft.com`).

![Completed theHarvester Baidu run in Kali Linux](evidence/08-kali-theharvester-final-run.png)

---

### 2. Local Network Configuration (Windows 11 VM)
Network parameters were verified on the Windows 11 guest (`window11vm`) via Command Prompt (`ipconfig`) and PowerShell (`ipconfig /all`):

| Parameter | Value |
| :--- | :--- |
| **Host Name** | `window11vm` |
| **Adapter** | `Intel(R) PRO/1000 MT Desktop Adapter` |
| **Physical (MAC) Address** | `08-00-27-B4-08-FD` |
| **IPv4 Address** | `10.0.0.4` |
| **Subnet Mask** | `255.255.255.0` |
| **Network CIDR** | `10.0.0.0/24` |
| **Default Gateway** | `10.0.0.1` |
| **DHCP Server** | `10.0.0.2` |
| **DNS Servers** | `8.8.8.8`, `8.8.4.4` |

![Windows Command Prompt ipconfig output](evidence/02-windows-ipconfig.png)

![Windows 11 VirtualBox console view](evidence/03-windows11-virtualbox-console.png)

![Windows PowerShell ipconfig /all output](evidence/07-windows-ipconfig-all.png)

---

### 3. Active Host Discovery & Topology (Zenmap / Nmap)
* **Target Subnet:** `10.0.0.0/24`
* **Scan Profile:** Ping scan
* **Command Executed:**
  ```bash
  nmap -sn 10.0.0.0/24
  ```
* **Summary:** Scanned **256 IP addresses** in `3.13 seconds` (`4.39 seconds` on initial run); **3 hosts up**.

| Live Host | MAC Address Shown | Evidence-Based Note |
| :--- | :--- | :--- |
| `10.0.0.1` | `52:54:00:12:35:00` | QEMU virtual NIC (`0.0070s` latency); default gateway per Windows config |
| `10.0.0.2` | `08:00:27:B0:EF:2A` | Oracle VirtualBox virtual NIC (`0.0051s` latency); DHCP server per Windows config |
| `10.0.0.4` | *Not shown in Nmap* | Matches the local Windows 11 VM IPv4 address |

![Zenmap ping scan inside VirtualBox console](evidence/04-zenmap-ping-scan.png)

![Zenmap detailed host discovery output](evidence/06-zenmap-host-discovery-details.png)

* **Network Topology:** Zenmap's topology graph visualizes the three responding nodes (`10.0.0.1`, `10.0.0.2`, and `10.0.0.4`) connected to `localhost`.

![Zenmap network topology view and legend](evidence/05-zenmap-topology.png)

---

## 📊 Observations & Recommendations

| Observation | Recommendation |
| :--- | :--- |
| Three live hosts responded on the `10.0.0.0/24` lab subnet. | Maintain a baseline asset inventory and investigate any unexpected MAC/IP pairs. |
| Windows VM uses `10.0.0.4/24` with gateway `10.0.0.1` and DHCP `10.0.0.2`. | Document VirtualBox network adapter modes (e.g., NAT Network / Host-Only) to ensure proper lab isolation. |
| External DNS resolvers (`8.8.8.8`, `8.8.4.4`) are configured. | Confirm external DNS forwarding is intentional for the lab environment. |
| `theHarvester` query against Baidu returned 3 subdomains (`c.urs.microsoft.com`, `copilot.microsoft.com`, `hxd.research.microsoft.com`). | Correlate across multiple OSINT sources and configure API keys when performing authorized reconnaissance. |

### Assessment Limitations
* No port scanning (`-sS`/`-sT`), service/version detection (`-sV`), vulnerability scanning, or exploitation was performed.
* Findings are informational baseline observations; no CVSS scores are assigned.

---

## 📁 Repository Structure & Evidence Mapping

Place your uploaded screenshots into the `evidence/` folder using the filenames below so all Markdown image links render automatically on GitHub:

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

| Evidence ID | Repository Path | Original Screenshot File | Description |
| :--- | :--- | :--- | :--- |
| **Evidence 01** | `evidence/01-kali-theharvester-initial-run.png` | `Screenshot 2026-11-12 014008.jpg` | Initial `theHarvester` CLI launch and `-b all` API key notices in Kali Linux |
| **Evidence 02** | `evidence/02-windows-ipconfig.png` | `Screenshot 2026-11-12 020807.png` | Windows Command Prompt showing basic `ipconfig` output (`10.0.0.4`) |
| **Evidence 03** | `evidence/03-windows11-virtualbox-console.png` | `Screenshot 2026-11-12 021104.png` | Full VirtualBox console view of the Windows 11 VM network check |
| **Evidence 04** | `evidence/04-zenmap-ping-scan.png` | `Screenshot 2026-11-12 021829.png` | Zenmap GUI running `nmap -sn 10.0.0.0/24` inside VirtualBox |
| **Evidence 05** | `evidence/05-zenmap-topology.png` | `Screenshot 2026-11-12 022233.png` | Zenmap Topology tab and legend displaying the 3 active hosts |
| **Evidence 06** | `evidence/06-zenmap-host-discovery-details.png` | `Screenshot 2026-11-12 022454.png` | Detailed Nmap host discovery output showing MAC addresses and latencies |
| **Evidence 07** | `evidence/07-windows-ipconfig-all.png` | `Screenshot 2026-11-12 023442.png` | Windows PowerShell `ipconfig /all` output showing DHCP, DNS, and MAC details |
| **Evidence 08** | `evidence/08-kali-theharvester-final-run.png` | `image_19598f.jpg` | Completed `theHarvester -d microsoft.com -l 1000 -b baidu` output (3 hosts found) |