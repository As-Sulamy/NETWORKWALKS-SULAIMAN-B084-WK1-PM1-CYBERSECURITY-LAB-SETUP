# NETWORKWALKS-SULAIMAN-B084-WK1-PM1-CYBERSECURITY-LAB-SETUP
Cybersecurity lab setup: Kali Linux on VirtualBox with a private NAT Network, static IP, and snapshot baseline
# 🔐 Cybersecurity Lab Environment Setup

**An isolated virtual lab for ethical hacking and penetration-testing practice, built with VirtualBox and Kali Linux.**

![VirtualBox](https://img.shields.io/badge/VirtualBox-7.2.22-0070C0?style=flat-square)
![Kali](https://img.shields.io/badge/Kali%20Linux-2026.3-E87500?style=flat-square&logo=kalilinux&logoColor=white)
![Network](https://img.shields.io/badge/Network-NAT%20Network-238F89?style=flat-square)
![Ethical Hacking](https://img.shields.io/badge/Ethical%20Hacking-C00000?style=flat-square)

---

## 📌 Project Overview

This project documents how I built a **virtual cybersecurity lab** on my own computer. The lab runs Kali Linux inside VirtualBox on a private virtual network, so I can practise scanning, reconnaissance, and vulnerability assessment without touching any real network or system.

The network is designed to grow: more machines (such as intentionally vulnerable targets) can be added later and will be able to talk to Kali on the same private network.

---

## 🎯 Objectives

- Install and configure VirtualBox as the hypervisor.
- Import Kali Linux as a virtual machine.
- Create a dedicated **NAT Network** for the lab.
- Give the Kali VM a fixed IP address.
- Verify connectivity, DNS, and tool availability.
- Take a clean snapshot as a recovery baseline.
- Document the whole process, including problems and fixes.

---

## 🛡️ Purpose of the Lab

The lab is a safe, repeatable environment for activities such as:

- Network reconnaissance and port scanning
- Vulnerability assessment
- Packet capture and analysis
- Web application security testing
- Testing security tools before using them elsewhere

> ⚠️ **Ethical use only.** This lab must only be used against systems I own or have explicit written permission to test.

---

## 🏗️ Lab Architecture

```
                 ┌──────────────────────────────────┐
                 │   Host: [WINDOWS]                 │
                 │   VirtualBox [7.2.22]            │
                 │                                  │
                 │   ┌──────────────────────────┐   │
                 │   │ NAT Network:             │   │
                 │   │ 10.0.0.0/24              │   │
                 │   │                          │   │
                 │   │  ┌────────────────────┐  │   │
                 │   │  │ Kali Linux         │  │   │
                 │   │  │ 10.0.0.2           │  │   │
                 │   │  └────────────────────┘  │   │
                 │   │  (future targets here)   │   │
                 │   └────────────┬─────────────┘   │
                 └────────────────┼─────────────────┘
                                  │ NAT (outbound only)
                              Internet
```



---

## ⚙️ Lab Configuration

| Component | Configuration |
|---|---|
| Host OS | Windows 11 |
| Host RAM | 12 GB |
| Processor | Intel Core i5 |
| Hypervisor | VirtualBox 7.2.22 |
| Security OS | Kali Linux 2026.3 |
| Kali RAM | 4048 MB |
| Kali CPUs | 2 |
| Virtual Network | NAT Network |
| Network Address | 10.0.0.0/24 |
| Kali IP Address | 10.0.0.2/24 |
| Default Gateway | 10.0.0.1 |
| DNS Server | 8.8.8.8 |
| Reserved for future VMs | 10.0.0.3 – 10.0.0.99 |

---

## 🪜 Setup Procedure

### Step 1. Check that virtualization is enabled
Before installing anything, I confirmed that hardware virtualization (Intel VT-x/AMD-V) is enabled. On Windows, this can be checked in **Task Manager → Performance → CPU → Virtualization: Enabled**.

### Step 2. Install 7-Zip
Kali's pre-built VM is often distributed as a `.7z` archive, so I installed 7-Zip to extract it.

### Step 3. Install VirtualBox
I downloaded VirtualBox from the official site and installed it with the default options.

![VirtualBox installed](https://github.com/As-Sulamy/NETWORKWALKS-SULAIMAN-B084-WK1-PM1-CYBERSECURITY-LAB-SETUP/blob/main/Kali%20Machine.png)

### Step 4. Create the NAT Network
In VirtualBox: **Tools → Network → NAT Networks → Create**.

```
Name:          [NatNetwork]
IPv4 Prefix:   10.0.0.0/24
DHCP:          Enabled
IPv6:          Disabled
```

I chose a **NAT Network** rather than plain NAT because VMs on the same NAT Network can communicate with each other *and* still reach the internet for updates. Plain NAT isolates each VM from the others.

![NAT Network settings](https://github.com/As-Sulamy/NETWORKWALKS-SULAIMAN-B084-WK1-PM1-CYBERSECURITY-LAB-SETUP/blob/main/Nat%20setup.png)

### Step 5. Import Kali Linux
I downloaded the pre-built Kali VirtualBox image from the official Kali website, extracted it, and imported it (**File → Import Appliance**, or **Machine → Add** for a `.vbox` file).

VM settings:

```
RAM:            [2048 MB]
CPUs:           [2]
Adapter 1:      Attached to NAT Network
Network name:   [NatNetwork]
```

![Kali VM settings](https://github.com/As-Sulamy/NETWORKWALKS-SULAIMAN-B084-WK1-PM1-CYBERSECURITY-LAB-SETUP/blob/main/Kali%20Machine.png)

### Step 6. Boot Kali and update it
I started the VM, logged in with the default credentials from the Kali documentation, and changed the default password right away.

```bash
sudo apt update && sudo apt full-upgrade -y
```

![Kali desktop](https://github.com/As-Sulamy/NETWORKWALKS-SULAIMAN-B084-WK1-PM1-CYBERSECURITY-LAB-SETUP/blob/main/Kali%20Testig.png)

### Step 7. Configure a static IP address
I assigned Kali a fixed address so it is easy to reference in future labs.

```
IP Address:   10.0.0.2
Subnet Mask:  255.255.255.0
Gateway:      10.0.0.1
DNS:          8.8.8.8
```

![Kali network settings](https://github.com/As-Sulamy/NETWORKWALKS-SULAIMAN-B084-WK1-PM1-CYBERSECURITY-LAB-SETUP/blob/main/Kali%20Machine.png)

### Step 8. Take a clean snapshot
Once everything worked, I created a VirtualBox snapshot named:

```
Clean Kali - Network Setup
```

If a later experiment breaks the VM, I can restore this known-good baseline.


---

## 🔎 Lab Verification

| Test | Command | Expected Result | Result |
|---|---|---|---|
| Check IP address | `ip a` | Correct Kali IP shown | ✅ |
| Test gateway | `ping -c 4 10.0.0.1` | Replies received | ✅ |
| Test internet | `ping -c 4 8.8.8.8` | Replies received | ✅ |
| Test DNS | `nslookup google.com` | Domain resolves | ✅ |
| Verify Nmap | `nmap --version` | Version displayed | ✅ |
| Verify snapshot | Restore snapshot, run `ip a` | Baseline config restored | ✅ |



---

## 🐞 Problems Encountered & Solutions

> Keep only the problems you actually hit. Delete the rest.

### Problem 1. VM would not start (VT-x / virtualization error)
**Cause:** Hardware virtualization was disabled in BIOS/UEFI.
**Fix:** Restarted the PC, entered BIOS/UEFI, enabled Intel VT-x (or AMD-V / SVM Mode), saved, and rebooted.

### Problem 2. No internet after setting a static IP
**Cause:** NetworkManager settings conflicted with the manual configuration.
**Fix:** Found the real connection name with `nmcli connection show`, then reapplied the address, gateway, and DNS to that connection and restarted it:

```bash
sudo nmcli connection modify "<connection-name>" ipv4.dad-timeout 0
sudo nmcli connection down "<connection-name>" && sudo nmcli connection up "<connection-name>"
```

### Problem 3. Kali has no internet after setting a static IP
**Cause:** Wrong gateway or DNS, or the NetworkManager connection name differs from the one in your command
**Fix:** Check the connection name with nmcli connection show, confirm the gateway and DNS, then restart the connection

---

## 💡 What I Learned

1. **NAT vs NAT Network.** NAT isolates each VM; a NAT Network lets VMs on the same network talk to each other while still having outbound internet.
2. **VM networking.** How virtual adapters connect a VM to a network and how that affects what it can reach.
3. **Static IP configuration.** Setting and verifying IP, subnet mask, gateway, and DNS in Kali.
4. **Snapshots.** Take a clean snapshot *before* risky experiments, not after.
5. **Documentation.** Good notes, screenshots, and recorded problems are part of professional security work.

---

## 🔐 Security & Ethical Use

This lab is for education and authorized testing only. I will not use these tools against any system without explicit permission.

---

## 🔗 Tools & Resources

- **7-Zip:** https://7-zip.org/download.html
- **VirtualBox:** https://www.virtualbox.org/wiki/Downloads
- **Kali Linux:** https://www.kali.org/get-kali/

---

## 👤 Author

**Sulaiman Ismail Bello**
Cybersecurity Analyst Intern, NETWORKWALKS 

LinkedIn: https://lnkd.in/p/d_xcwcfC

---

## 📌 Project Information

**Program:** Cyversecurity | **Week:** 01 | **Project:** Cybersecurity Lab Setup | **Repository:** GitHub
