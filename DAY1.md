<div align="center">

# 🐧 Linux Learning Journey — Day 1

### *From "What is Linux" to Ethical Hacking with Kali — the visual way*

![Linux](https://img.shields.io/badge/OS-Linux-FCC624?style=for-the-badge&logo=linux&logoColor=black)
![VirtualBox](https://img.shields.io/badge/Virtualization-Oracle_VM-183A61?style=for-the-badge&logo=virtualbox&logoColor=white)
![Kali](https://img.shields.io/badge/Security-Kali_Linux-557C94?style=for-the-badge&logo=kalilinux&logoColor=white)
![Status](https://img.shields.io/badge/Progress-Day%201-brightgreen?style=for-the-badge)

*A beginner's notes — visual, quick, and practical. No boring theory walls. 🚀*

</div>

---

## 🗺️ Quick Navigation

| 🧩 Topic | 🔑 One-liner |
|---|---|
| [🐧 What is Linux](#-1-what-is-linux) | The manager running everything behind the scenes |
| [🖥️ TTY Sessions](#️-2-terminal-sessions-behind-the-gui) | Secret rooms behind your desktop |
| [🐚 Shell](#-3-what-is-a-shell) | Your translator to talk to the kernel |
| [📜 Linux History](#-4-history-of-linux) | One student, 1991, and the internet today |
| [📦 Distributions](#-5-linux-distributions) | Same engine, different cars |
| [☁️ Virtualization](#️-6-virtualization) | One building, many flats |
| [🧠 Hypervisors](#-7-hypervisors--types) | The landlord of virtual machines |
| [📦 Oracle VirtualBox](#-8-oracle-virtualbox) | TV inside a TV |
| [🌐 Network Adapters](#-10-network-adapter-types) | Who can talk to whom |
| [📁 File Transfer](#-11-file-transfer-winscp--scp) | Moving files safely between servers |
| [🕵️ Kali Linux](#️-12-why-kali-linux) | The hacker's ready-made toolbox |

---

## 🐧 1. What is Linux

<img src="https://img.shields.io/badge/Role-Operating%20System-blue?style=flat-square" />

Linux = the **manager** between your hardware and your apps.

```mermaid
flowchart LR
    A[👤 You / Apps] -->|Requests| B((🐧 Linux OS))
    B -->|Controls| C[💾 Hardware: CPU, RAM, Disk]
    B -->|Controls| D[🖨️ Devices]
```

> 💡 **Real scenario:** A company skips paying for 50 Windows licenses → installs **Ubuntu Server** free → saves money, gets rock-solid uptime.

---

## 🖥️ 2. Terminal Sessions Behind the GUI

Even with a graphical desktop open, Linux keeps **6 virtual terminals** running quietly in the background.

```mermaid
flowchart TB
    subgraph Linux Machine
    T1[TTY1 - GUI 🖥️]
    T2[TTY2 - Text]
    T3[TTY3 - Text]
    T4[TTY4 - Text]
    T5[TTY5 - Text]
    T6[TTY6 - Text]
    end
    Key1["Ctrl+Alt+F1"] --> T1
    Key2["Ctrl+Alt+F2"] --> T2
    Key3["Ctrl+Alt+F3"] --> T3
```

> 💡 **Scenario:** Your desktop **freezes** 🥶 → press `Ctrl+Alt+F3` → kill the frozen app from a clean text session → desktop is saved without a restart.

---

## 🐚 3. What is a Shell?

<details>
<summary>🔽 Click to see how a Shell works (Waiter Analogy)</summary>

```mermaid
sequenceDiagram
    participant You
    participant Shell as 🐚 Shell (bash)
    participant Kernel as 🐧 Kernel
    You->>Shell: "list my files" (ls)
    Shell->>Kernel: system call
    Kernel-->>Shell: file data
    Shell-->>You: 📄 output shown
```

</details>

**Popular shells:** `bash` 🅱️ `zsh` ⚡ `sh` 🔹 `ksh` 🔸

> 💡 **Scenario:** Admin manages **100 remote servers** → writes ONE bash script → updates all servers in minutes instead of clicking through each one manually.

---

## 📜 4. History of Linux

```mermaid
timeline
    title Linux Timeline
    1969 : UNIX created at Bell Labs
    1991 : Linus Torvalds builds Linux kernel (Finland 🇫🇮)
    1992 : Linux combined with GNU tools → full OS
    Today : Powers Android, Tesla, NASA, most web servers
```

> 🧠 Linux ≠ copied UNIX. It was **inspired by** UNIX but written completely from scratch, then made open-source.

---

## 📦 5. Linux Distributions

| 🚗 Distro | 🎯 Best For | Vibe |
|---|---|---|
| 🟠 **Ubuntu** | Beginners, daily use | Family car |
| 🔵 **Debian** | Stability | Reliable van |
| 🔴 **Fedora** | Latest features | Sports car |
| ⚫ **Kali Linux** | Hacking/security | Race car |
| 🟣 **CentOS/RHEL** | Enterprise servers | Heavy truck |

Same **engine (kernel)** 🔧 → different **cars (distros)** built on top.

---

## ☁️ 6. Virtualization

```mermaid
flowchart TB
    H[🏢 Physical Server / Host Machine] --> V1[🏠 VM 1 - Ubuntu]
    H --> V2[🏠 VM 2 - Kali Linux]
    H --> V3[🏠 VM 3 - Windows]
```

One building 🏢 → many independent flats 🏠 → each isolated, sharing the same physical resources.

> 💡 **Scenario:** One laptop, but you want Windows Server skills **AND** Kali hacking skills → run both as VMs on the same machine, zero extra hardware cost.

---

## 🧠 7. Hypervisors & Types

```mermaid
flowchart LR
    subgraph Type1["🏗️ Type 1 - Bare Metal"]
        HW1[Hardware] --> HV1[Hypervisor] --> VM1[VMs]
    end
    subgraph Type2["🖥️ Type 2 - Hosted"]
        HW2[Hardware] --> OS2[Host OS] --> HV2[Hypervisor] --> VM2[VMs]
    end
```

| Type | Runs On | Example | Used By |
|---|---|---|---|
| 🏗️ **Type 1** | Directly on hardware | VMware ESXi, Hyper-V | Data centers |
| 🖥️ **Type 2** | On top of an OS | **VirtualBox**, VMware Workstation | Students & learners |

---

## 📦 8. Oracle VirtualBox

<div align="center">

**"A TV playing inside your TV" 📺➡️📺**

</div>

```mermaid
flowchart TB
    Laptop[💻 Your Windows/Mac Laptop] --> VBox[📦 Oracle VirtualBox]
    VBox --> Ubuntu[🐧 Ubuntu VM]
    VBox --> Kali[🕵️ Kali Linux VM]
```

## 🎯 9. Why Oracle VM?

✅ Free & open-source &nbsp;|&nbsp; ✅ Safe sandbox to break things &nbsp;|&nbsp; ✅ Snapshots (undo button 🔁) &nbsp;|&nbsp; ✅ Practice networking safely

---

## 🌐 10. Network Adapter Types

```mermaid
flowchart LR
    Internet((🌍 Internet))
    Host[💻 Host Machine]
    VM1[🖥️ VM A]
    VM2[🖥️ VM B]

    Internet -.NAT.-> VM1
    Host <-.Host-Only.-> VM1
    VM1 <-.Internal/NAT Network.-> VM2
    Internet <-.Bridged.-> Host
```

| 🌐 Network Type | VM→Host | VM→VM | VM→Internet | Internet→VM |
|---|:---:|:---:|:---:|:---:|
| 🟡 **NAT** | ❌ | ❌ | ✅ | ❌ |
| 🟢 **NAT Network** | ❌ | ✅ | ✅ | ❌ |
| 🔵 **Bridged** | ✅ | ✅ | ✅ | ✅ |
| 🟣 **Host-Only** | ✅ | ✅ | ❌ | ❌ |
| ⚫ **Internal Network** | ❌ | ✅ | ❌ | ❌ |

> 💡 **Scenario:** Hacking lab setup → Kali + victim VM on **Internal Network** → they attack each other, but total isolation from real internet/host. 🔒

---

## 📁 11. File Transfer: WinSCP & scp

```mermaid
flowchart LR
    Win[🪟 Windows PC] <-->|SFTP - WinSCP GUI| Linux1[🐧 Linux Server]
    Linux1 <-->|scp / rsync CLI| Linux2[🐧 Linux Server 2]
```

**Windows ➡ Linux (WinSCP):** connect via SFTP → drag & drop files between two panels.

**Linux ➡ Linux (Terminal):**
```bash
scp report.txt admin@192.168.1.50:/home/admin/documents/
```

> 💡 **Scenario:** Nightly backup script uses `scp` + `cron` to auto-copy database backups from **Server A ➜ Server B** every night. 🌙

---

## 🕵️ 12. Why Kali Linux?

<div align="center">

**"A fully-stocked electrician's toolbox 🧰 — 600+ tools, ready on day one"**

</div>

```mermaid
mindmap
  root((🕵️ Kali Linux))
    Nmap 🔍
      Network scanning
    Wireshark 🌊
      Traffic analysis
    Metasploit 💥
      Exploit testing
    Burp Suite 🕸️
      Web app testing
```

**How much should a beginner know?** Just 4 tools to start: `Nmap` `Wireshark` `Metasploit` `Burp Suite` + basic networking. Go deeper later. 🎓

> 💡 **Scenario:** Ethical hacker scans a company's servers with **Nmap** before launch day → finds an open vulnerable port → devs patch it before real attackers find it. 🛡️

---

<div align="center">
*More notes coming as the Linux journey continues...* 🐧🚀

</div>
