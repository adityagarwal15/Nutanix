# Nutanix SRE Intern (On-Campus/Fresher) – Final Interview Prep Sheet

## 🚦 Role & Focus Areas

- **Non-coding SRE support role:** No DSA or typical coding rounds.
- Heavy focus on: **Linux commands, Networking, OS basics, Hardware, Cloud, Virtualization, Troubleshooting.**
- Both interviews are technical, scenario-driven, and assess your communication & thinking.

## 1. Online Test (OT) – What to Expect

- ~50 MCQs in 60-70 min.
- Covers: Linux, Networks, OS, Hardware, Virtualization, Cloud
- Mix of theory, command syntax, scenario, and true/false.

## 2. Core Technical Questions & Answers

### **Linux**

- `grep "error" file.txt` — Search for "error" lines in `file.txt`.
- `less filename` — View file (page-wise scrollable).
- `cat file.txt` — Show whole content.
- `pkill firefox` — Kill all processes named firefox.
- `vi file.txt` — Open file in VI editor. *(Know basics: `i` to insert, `:wq` to save quit, `:q!` to force quit)*
- **What is a shell?**  
  *A program that interprets your commands (bash, zsh, etc).*
- **Where are IP addresses stored in Linux?**  
  *Check with `ip addr`. Hosts resolved in `/etc/hosts`.*

**Extra Practical:**  
`df -h` (disk), `top` (CPU/mem), `ps` (processes), pipes (`|`), recursive chmod (`chmod -R`).

### **Operating System (OS) Essentials**

- **OS Boot Process:** BIOS/UEFI → Bootloader (GRUB) → Kernel → Init/Systemd → Login screen.
- **Deadlock:** 4 conditions (Mutual Exclusion, Hold+Wait, No Preemption, Circular Wait).
- **Kernel mode vs User mode:** Kernel is privileged; user mode limited.
- **Virtual memory/paging:** Allows more usable memory, uses disk when RAM full.
- **Inodes:** File metadata, run out of inodes → can't create new files (check with `df -i`).
- **What is a semaphore?** *Used for thread/process synchronization.*

### **Computer Networks**

- **OSI Model:** Physical, Data Link, Network, Transport, Session, Presentation, Application (7 layers).
- **TCP vs UDP:** TCP is reliable and connection-oriented, UDP is fast and connectionless.
- **Ports:** Know HTTP (80), HTTPS (443), SSH (22), DNS (53), FTP (21).
- **DNS Process:** Translates names to IPs. (Uses A, CNAME, MX records.)
- **Subnetting:** Breaks network into smaller segments for efficiency, security.
- **NAT:** Network Address Translation; types: Static, Dynamic, PAT.
- **IP vs MAC:** IP is logical (Layer 3); MAC physical (Layer 2).
- **Commands:** `ping`, `traceroute`, `ifconfig/ip addr`, `netstat`.

### **Hardware**

- **HDD vs SSD:** HDD = spinning disks/slower; SSD = no moving parts/faster and more durable.
- **ECC RAM:** Can detect/correct memory errors, used in servers.
- **RAID (0/1/5/10):** For redundancy/performance.
- **NIC:** Network Interface Card, connects to network.

### **Virtualization**

- **What is virtualization?**  
  *Running multiple OSes/VMs on same physical hardware via hypervisor.*
- **Type 1 (bare metal/ESXi/AHV) vs Type 2 (hosted/VirtualBox) hypervisors.**
- **Host v/s Guest:** Host = physical machine running hypervisor, Guest = VM.
- **Can guest have more memory than host?** *No in absolute terms, but overcommit is sometimes used.*

### **Cloud Computing**

- **Public vs Private vs Hybrid Cloud:**  
  - **Public:** AWS/Azure (shared infra).  
  - **Private:** On-premises, dedicated.  
  - **Hybrid:** Combination of both (portable workloads).
- **IaaS vs PaaS vs SaaS:**  
  - **IaaS:** Virtual hardware (VMs)  
  - **PaaS:** Platform for dev (App Engine, Heroku)  
  - **SaaS:** Ready-made apps (Gmail)
- Nutanix specializes in HCI and hybrid/multi-cloud management.

### **Nutanix-Specific**

- **Hyperconverged Infrastructure (HCI):**  
  Combines compute, storage, networking into a single platform managed by software.
- **Main Nutanix products:**  
  - Nutanix Cloud Infrastructure (NCI)  
  - AHV Hypervisor  
  - Prism dashboard
- **Company values:** **Hungry, Humble, Honest, with Heart**
- Know why you want SRE and why Nutanix.

## 3. Scenario & Practical Questions

- **"A website is not reachable from one computer, but is from another. Troubleshoot."**  
  *Check DNS, network, hosts files, proxy/firewall, connectivity.*
- **"System time out of sync?"**  
  *NTP (Network Time Protocol) issue.*
- **"Printer/coffee machine isn't working."**  
  *Check physical issues, power, network, drivers/logs, restart.*
- **"A production VM is down. What to do?"**  
  *Check logs/status, attempt safe restart, contact higher support if unresolved.*

## 4. Interview Rounds Detailed

- **Round 1:**  
  - MCQ test (Linux, CN, OS, cloud, hardware, virtualization)
- **Round 2:**  
  - Technical (commands, theory, practical troubleshooting, scenario Qs)
- **(Optional) Round 3:**  
  - More technical or scenario-based, could be skipped if you do well.
- **Round 4 (HR):**  
  - Motivations, background, "Why SRE?", communication, scenario/behavioral Qs.

## 5. Top Tips for Success

- **Practice commands hands-on** (not just theory).
- **Explain terms simply, with real-world examples.**
- **Be honest if you don't know an answer—show logical thinking.**
- **Confidence, clarity, and communication > memorized facts.**
- Revise port numbers, commands, process/boot flow, OSI, matrices/tables.
- Know your resume projects (they may ask for cloud/infrastructure drawings if relevant).

## 6. Rapid-Fire Last-Minute Q&A

- **Q:** How to see first 20 lines of a file?  
  **A:** `head -n 20 filename`
- **Q:** Difference between HTTP and HTTPS?  
  **A:** HTTPS is HTTP over TLS/SSL (encrypted, secure).
- **Q:** What is a shell?  
  **A:** Command interpreter between user and OS.
- **Q:** OSI Model layers?  
  **A:** P D N T S P A (Physical, Data Link, Network, Transport, Session, Presentation, Application).
- **Q:** How would you check disk space?  
  **A:** `df -h`
- **Q:** What will you do if you don't know the solution to a customer's problem?  
  **A:** Admit honestly, research/escalate, keep customer informed.
- **Q:** Difference: HDD vs SSD?  
  **A:** HDD is mechanical & slower, SSD is electronic & faster.

---

**If you can explain, troubleshoot, and execute all the above—you're well prepared for Nutanix SRE's on-campus process. Confidence + basics = success.**

**Good luck!**
