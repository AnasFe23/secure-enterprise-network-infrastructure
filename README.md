# 🏆 Secure Enterprise Network Infrastructure & Cyber Defense (Workshop II)

[![Award](https://img.shields.io/badge/Award-3rd%20Place%20Best%20Faculty%20Project-gold.svg)]()
[![Course](https://img.shields.io/badge/Course-BAXU%203923%20Workshop%20II-blue.svg)](https://www.utem.edu.my/)
[![University](https://img.shields.io/badge/UTeM-FAIX-red.svg)](https://www.utem.edu.my/)
[![Status](https://img.shields.io/badge/Status-Completed%20%26%20Defended-success.svg)]()

> 🥉 **Honors & Recognition:** Awarded **3rd Place for Best Project** across the Faculty of Artificial Intelligence and Cyber Security (FAIX), Universiti Teknikal Malaysia Melaka (UTeM).  
> **Course Module:** BAXU 3923 — Workshop II (Semester 1 2025/2026)  
> **Team:** Group 2 (White Team, Orange Team, Blue Team, Red Team)  
> **Supervised by:** Ts. Nurhashikin Binti Mohd Salleh  
> **Key Module Owner (Anas Faozi):** Secure Wireless Perimeter & Cross-Service 802.1X FreeRADIUS Integration (VLAN 70)

---

## 📄 Full Project Documentation
- 📥 **[Download Complete Workshop II Final Technical Report (PDF)](./GROUP%202_WorkshopII.pdf)**

---

## 📌 Project Overview
This project presents the architecture, configuration, offensive penetration testing, defensive hardening, and SIEM monitoring of an enterprise network designed for 1,000+ users across a segmented 7-VLAN topology. 

The engagement simulated a multi-team operational model:
- **White Team (Governance & SIEM):** Orchestrated centralized log correlation (Wazuh & Graylog), PKI/HTTPS security policies, and AP governance.
- **Orange Team (Core Infrastructure):** Configured Cisco Layer-3 routing, VLSM subnetting, PAT/Static NAT, Active Directory DS, DNS, DHCP, and IIS Virtual Webhosting.
- **Red Team (Offensive Operations):** Executed reconnaissance, SQLi, web shell exploitation, and full root privilege escalation.
- **Blue Team (Hardening & IDS):** Deployed Suricata IDS with ELK stack, FreeRADIUS 802.1X AAA, Single Sign-On (Kerberos/Samba AD Join), ModSecurity WAF, and OS hardening.

---

## 🏗️ Network Architecture & 7-VLAN Segmentation

```text
                                [ Metro Ethernet / ISP (200.1.40.0/24) ]
                                                   │
                                     [ Cisco Core Router (G2R1) ]
                                    (Sub-interfaces, PAT & Static NAT)
                                                   │
                                     [ Cisco 3650 Layer 3 Switch ]
                                        (802.1Q Inter-VLAN Trunk)
                                                   │
      ┌────────────────┬────────────────┬──────────┴─────┬────────────────┬────────────────┐
      │                │                │                │                │                │
 [VLAN 10]        [VLAN 20]        [VLAN 30]        [VLAN 40]        [VLAN 50]        [VLAN 60]        [VLAN 70]
  Finance            OPS             Admin          White Team       Orange Team       Blue Team       Wireless / AP
172.16.58.0/24   172.16.59.0/24   172.16.60.0/25  172.16.60.128/28 172.16.60.144/28 172.16.60.160/28   10.1.1.0/24
(DHCP Scope)     (DHCP Scope)     (126 Hosts)     (Wazuh/Graylog)  (Win Server/AD)  (Suricata/Radius) (802.1X EAP-PEAP)

```

## 🔑 Key Engineering Contribution: Enterprise 802.1X RADIUS Authentication

(Lead Author / Assigned Role: Anas Faozi Abdullah Al-Abi)

Implemented centralized identity governance and network access control for the wireless perimeter (VLAN 70) using FreeRADIUS 3.0
- Cross-VLAN AAA Integration: Configured FreeRADIUS on the Blue Team Linux Server (172.16.60.162) to authenticate wireless clients associating with the ASUS Wireless AP (10.1.1.12) over UDP port 1812.
- WPA2/WPA3-Enterprise Deployment: Replaced vulnerable pre-shared keys (PSK) with dynamic EAP-PEAP (MSCHAPv2) authentication, creating per-user credentials and granular session auditing.
- Switch & Router Fallback: Enforced aaa new-model authentication with fallback administrative users to prevent device lockouts during server outages
- Audit Logging: Monitored and validated radius.log telemetry to verify Access-Request, Access-Accept, and unauthorized Access-Reject events in real time[cite: 3].
