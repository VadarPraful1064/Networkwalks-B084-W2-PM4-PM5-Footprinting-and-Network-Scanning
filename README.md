# Footprinting & Network Scanning Lab (Kali Linux & Windows 11)

An evidence-based cybersecurity lab exercise documenting passive reconnaissance (OSINT) and active local network discovery using **Kali Linux** and **Windows 11** virtual machines on **Oracle VirtualBox**.

> **Disclaimer & Scope:** Only activities visible in the supplied evidence screenshots are documented below. No unshown tests, port scans, or vulnerabilities are claimed. All scanning was restricted to an authorized local lab subnet (`10.0.0.0/24`).

---

## 📋 Lab Metadata

| Field | Details |
| :--- | :--- |
| **Prepared By** | Praful Vadar |
| **Assessment Date** | 9 November 2026 |
| **Report Date** | 9 November 2026 |
| **Modules Covered** | **PM4:** Footprinting & Reconnaissance with `theHarvester`<br>**PM5:** Network Scanning & Host Discovery with Zenmap / Nmap |
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

![Initial theHarvester run in Kali Linux]

* **Baidu Source Query:**
  ```bash
  theHarvester -d microsoft.com -l 1000 -b baidu
  ```
* **Result:** Returned **0 IPs, 0 emails, 0 people**, and **3 hosts** (`c.urs.microsoft.com`, `copilot.microsoft.com`, `hxd.research.microsoft.com`).

![Completed theHarvester Baidu run in Kali Linux]
<img width="1917" height="1020" alt="image" src="https://github.com/user-attachments/assets/8fe0cb8c-2f58-4e8f-b893-159964cbe01c" />
<img width="1917" height="1020" alt="Screenshot 2026-11-12 014008" src="https://github.com/user-attachments/assets/d287a0e6-53f3-4f0b-be4e-dfca08ccf793" />



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

![Windows Command Prompt ipconfig output]
<img width="1917" height="1020" alt="Screenshot 2026-11-12 021104" src="https://github.com/user-attachments/assets/74bd651e-8440-4a22-89ca-db356b29b2d5" />

![Windows PowerShell ipconfig /all output]
<img width="1031" height="772" alt="Screenshot 2026-11-12 023442" src="https://github.com/user-attachments/assets/0c695525-1910-4fcf-8fad-857d52746304" />


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

![Zenmap ping scan inside VirtualBox console]
<img width="1917" height="1022" alt="Screenshot 2026-11-12 021829" src="https://github.com/user-attachments/assets/b49a549d-caa2-4b98-8057-ab379fd19066" />


* **Network Topology:** Zenmap's topology graph visualizes the three responding nodes (`10.0.0.1`, `10.0.0.2`, and `10.0.0.4`) connected to `localhost`.

![Zenmap network topology view and legend]
<img width="1917" height="1020" alt="Screenshot 2026-11-12 022233" src="https://github.com/user-attachments/assets/5d893845-96ac-454a-9ded-81f221ed8548" />


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
