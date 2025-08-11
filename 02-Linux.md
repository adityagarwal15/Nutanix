# Linux Essentials for the SRE Interview

This guide covers the key Linux commands and concepts you need to know. The focus is on practical application: what the command does, how to use it, and why it's important for a Systems Reliability Engineer (SRE).

## 1. Essential Commands

### File System & Navigation

| Command | What it does | Example Usage |
|---------|--------------|---------------|
| `pwd` | Prints the working directory (shows you where you are) | `pwd` |
| `ls -la` | Lists all files/directories with detailed information | `ls -la /var/log` |
| `cd` | Changes the current directory | `cd /etc/nginx` |
| `cp` | Copies files or directories | `cp app.conf app.conf.bak` |
| `mv` | Moves or renames files or directories | `mv app.log /tmp/` |
| `rm` | Removes files. Use `rm -rf` for directories | `rm temp.txt` |
| `mkdir` | Creates a new directory | `mkdir -p /tmp/my_app` |
| `find` | Searches for files and directories | `find /var/log -name "*.log"` |
| `touch` | Creates an empty file or updates its timestamp | `touch /tmp/app.lock` |

### Text Manipulation & Viewing

| Command | What it does | Example Usage |
|---------|--------------|---------------|
| `cat` | Displays the content of a file | `cat /etc/hosts` |
| `less` | Displays file content one page at a time (q to quit) | `less /var/log/syslog` |
| `head` | Displays the first few lines of a file | `head -n 50 access.log` |
| `tail -f` | Follows a file in real-time, showing new lines | `tail -f /var/log/app.log` |
| `grep` | Searches for a pattern in text | `grep -i "error" /var/log/app.log` |
| `awk` | A powerful pattern scanning and processing language | `awk '{print $7}' access.log` |
| `sed` | A stream editor for filtering and transforming text | `sed 's/old/new/g' config.txt` |

### Process Management

| Command | What it does | Example Usage |
|---------|--------------|---------------|
| `ps aux` | Shows all currently running processes | `ps aux \| grep 'nginx'` |
| `top` / `htop` | Displays a real-time view of running processes | `htop` |
| `kill` | Sends a signal to a process to terminate it | `kill 12345` |
| `kill -9` | Forcefully kills a process | `kill -9 12345` |
| `pkill` | Kills processes based on their name | `pkill -f 'java -jar'` |

### Networking

| Command | What it does | Example Usage |
|---------|--------------|---------------|
| `ping` | Checks if a remote host is reachable | `ping -c 4 google.com` |
| `ss -tuln` | Shows all listening TCP and UDP ports (modern) | `ss -tuln` |
| `lsof -i` | Lists processes using a specific port | `sudo lsof -i :443` |
| `dig` | A tool for querying DNS servers | `dig nutanix.com` |
| `curl` | Transfers data from or to a server | `curl -I http://localhost/health` |
| `traceroute` | Traces the network path to a host | `traceroute google.com` |

### System & Disk Information

| Command | What it does | Example Usage |
|---------|--------------|---------------|
| `df -h` | Shows disk free space in human-readable format | `df -h` |
| `du -sh` | Shows disk usage of a directory in human-readable format | `du -sh /var/log/` |
| `free -h` | Shows free and used memory in human-readable format | `free -h` |
| `uname -a` | Prints all system information, including kernel version | `uname -a` |
| `uptime` | Shows system uptime and load averages | `uptime` |

## 2. Common SRE Troubleshooting Scenarios

### Scenario 1: "The system is running slow"

Your goal is to find the resource bottleneck: CPU, Memory, or I/O.

**Check System Load:**
- `uptime` - Look at the 1, 5, and 15-minute load averages. A load consistently higher than the number of CPU cores indicates a problem.

**Identify Problematic Processes:**
- `top` or `htop` - See which processes are consuming the most CPU (%CPU) and Memory (%MEM).

**Check Memory Usage:**
- `free -h` - Check if swap space is being heavily used. High swap usage indicates the system is out of physical RAM.

**Check Disk I/O:**
- `iostat -x 1` - Look for high %iowait (CPU waiting for disk) or high device utilization (%util).

### Scenario 2: "An application is not responding"

Your goal is to check connectivity from the outside-in.

**Check if the process is running:**
- `systemctl status myapp` - Use the modern service manager.
- `ps aux | grep myapp` - The classic way to see if the process exists.

**Check if it's listening on the correct port:**
- `ss -tuln | grep 8080` - Check if any process is listening on the application's port (e.g., 8080).

**Check the application logs in real-time:**
- `tail -f /var/log/myapp/error.log` - Look for errors as you try to connect.

**Test network connectivity locally:**
- `curl http://localhost:8080/health` - Can the server connect to the application itself?

### Scenario 3: "The disk is full"

Your goal is to find and clean up what's taking up space.

**Confirm which partition is full:**
- `df -h` - Identify the filesystem that is at or near 100% usage (e.g., /var).

**Find the largest directories:**
- `du -sh /var/* | sort -rh | head -n 10` - Find the top 10 largest directories within /var.

**Find the largest files:**
- `find /var/log -type f -size +1G` - Find all files larger than 1GB in /var/log.

**Clean up safely:**
- **NEVER** randomly delete files. First, try to truncate large log files (`> myapp.log`) or use tools like logrotate.

## 3. Interview Q&A: Troubleshooting Scenarios

### Q: How to troubleshoot a completely off laptop?

**Answer:**
Follow a systematic hardware troubleshooting approach:

1. **Check Power Supply:**
   - Verify power adapter is connected and LED indicator is on
   - Try a different power outlet
   - Check if battery is completely drained (leave charging for 30+ minutes)

2. **Perform Hard Reset:**
   - Remove battery (if removable) and power adapter
   - Hold power button for 30 seconds to drain residual power
   - Reconnect power and try to boot

3. **Check External Hardware:**
   - Disconnect all USB devices, external monitors, docks
   - Try booting with minimal hardware configuration

4. **Test Power Button:**
   - Ensure power button isn't stuck or damaged
   - Look for any LED indicators when pressing power button

5. **Hardware Failure Indicators:**
   - Listen for fan noise, hard drive spinning, or beep codes
   - Check for any lights on motherboard (if accessible)
   - If no signs of life, likely motherboard or power supply failure

### Q: What would you do to troubleshoot or fix the Blue Screen of Death (BSOD)?

**Answer:**
BSOD troubleshooting requires a systematic approach:

1. **Immediate Actions:**
   - Note the STOP code (e.g., 0x0000007E) and any error messages
   - Take a photo of the screen for reference
   - Restart the computer

2. **Boot into Safe Mode:**
   - Press F8 during boot or use Advanced Boot Options
   - Safe Mode loads minimal drivers to isolate the problem

3. **Check Recent Changes:**
   - Recently installed software or drivers
   - New hardware additions
   - Windows updates
   - Use System Restore to rollback recent changes

4. **Driver Issues:**
   - Update or rollback recently updated drivers
   - Use Device Manager to check for driver conflicts
   - Boot from Windows installation media and use driver rollback

5. **Hardware Testing:**
   - Run memory diagnostic (Windows Memory Diagnostic or MemTest86)
   - Check hard drive health with CHKDSK
   - Monitor temperatures to rule out overheating

6. **System File Corruption:**
   - Run `sfc /scannow` from Command Prompt
   - Use DISM tool: `DISM /Online /Cleanup-Image /RestoreHealth`

### Q: How data will be sent from one node to another through a hub?

**Answer:**
Hub-based networking (legacy Ethernet):

1. **Hub Characteristics:**
   - Operates at Physical Layer (Layer 1)
   - Creates single collision domain for all connected devices
   - Uses half-duplex communication
   - All ports share the total bandwidth

2. **Data Transmission Process:**
   - Source node sends data frame to hub
   - Hub receives the frame on one port
   - Hub repeats (amplifies) the frame to ALL other ports simultaneously
   - All nodes receive the frame, but only the intended recipient processes it
   - Other nodes ignore the frame (not their MAC address)

3. **Collision Detection:**
   - If two nodes transmit simultaneously, collision occurs
   - CSMA/CD (Carrier Sense Multiple Access/Collision Detection) protocol manages this
   - Nodes detect collision and wait random time before retransmitting

**Modern Alternative:** Switches create separate collision domains for each port and maintain MAC address tables for intelligent forwarding.

### Q: How to connect another network to the existing hub network and transfer data between networks?

**Answer:**
To connect two networks through hubs:

1. **Using a Router:**
   - Connect each hub to a router interface
   - Router operates at Network Layer (Layer 3)
   - Each network gets different IP subnet (e.g., 192.168.1.0/24 and 192.168.2.0/24)
   - Router maintains routing table for inter-network communication

2. **Data Transfer Process:**
   - Source node sends packet to router (default gateway)
   - Router examines destination IP address
   - Router forwards packet to appropriate network interface
   - Destination hub receives and broadcasts to all connected nodes
   - Target node processes the packet

3. **Using a Bridge:**
   - Bridge operates at Data Link Layer (Layer 2)
   - Connects two network segments
   - Maintains MAC address table for both networks
   - Filters traffic based on MAC addresses

### Q: How do you troubleshoot a desktop that doesn't boot up and you can't see anything on your screen?

**Answer:**
Systematic approach to no-display boot issues:

1. **Check Power and Connections:**
   - Ensure power cable is connected and PSU switch is on
   - Check monitor power and display cable connections
   - Try different display cable (HDMI, VGA, DVI)
   - Test with different monitor if available

2. **POST (Power-On Self Test) Indicators:**
   - Listen for beep codes (consult motherboard manual)
   - Check for LED indicators on motherboard
   - Look for fan spinning and hard drive activity

3. **Memory Issues:**
   - Reseat RAM modules
   - Try booting with one RAM stick at a time
   - Test with known good RAM

4. **Graphics Card Problems:**
   - Reseat graphics card
   - Try onboard graphics (if available)
   - Check power connectors to GPU

5. **BIOS/UEFI Issues:**
   - Clear CMOS (remove battery or use jumper)
   - Reset BIOS to defaults
   - Check for BIOS corruption

6. **Hardware Elimination:**
   - Disconnect non-essential peripherals
   - Try different PSU if available
   - Check motherboard for physical damage

### Q: Tell me everything that happens when you ping nutanix.com.

**Answer:**
Complete ping process breakdown:

1. **DNS Resolution:**
   - Check local DNS cache for nutanix.com IP
   - If not cached, query configured DNS servers
   - DNS server resolves nutanix.com to IP address (e.g., 52.84.86.123)

2. **Routing Decision:**
   - Check if destination IP is on local network
   - If remote, use default gateway (router)
   - Consult routing table for best path

3. **ARP Resolution:**
   - If destination is local: ARP to find MAC address
   - If remote: ARP to find gateway MAC address
   - Update ARP cache with MAC address mapping

4. **ICMP Echo Request Creation:**
   - Create ICMP packet with Type 8 (Echo Request)
   - Add sequence number and identifier
   - Calculate checksum

5. **IP Packet Formation:**
   - Add IP header with source and destination IPs
   - Set protocol field to 1 (ICMP)
   - Calculate IP header checksum

6. **Ethernet Frame Creation:**
   - Add Ethernet header with source and destination MACs
   - Add frame check sequence (FCS)

7. **Network Transmission:**
   - Frame sent to network interface
   - Travels through switches, routers to destination
   - Each router decrements TTL, forwards based on routing table

8. **Destination Processing:**
   - Destination receives frame, checks FCS
   - Processes IP packet, verifies checksum
   - Recognizes ICMP Echo Request
   - Creates ICMP Echo Reply with same data

9. **Return Path:**
   - Reply follows reverse path back to source
   - Source receives reply, measures round-trip time
   - Displays result with IP, bytes, time, TTL

### Q: Which command will tell you about the memory? (Linux)

**Answer:**
Multiple commands provide memory information:

1. **`free -h`** (Most Common):
   ```bash
   free -h
   # Shows total, used, free, shared, buff/cache, available memory
   # -h flag shows human-readable format (KB, MB, GB)
   ```

2. **`cat /proc/meminfo`** (Detailed):
   ```bash
   cat /proc/meminfo
   # Shows comprehensive memory statistics
   # MemTotal, MemFree, MemAvailable, Buffers, Cached, etc.
   ```

3. **`top` or `htop`** (Real-time):
   - Shows memory usage with process breakdown
   - Updates continuously

4. **`vmstat`** (Virtual Memory Statistics):
   ```bash
   vmstat 1 5
   # Shows memory, swap, and system activity
   ```

5. **`cat /proc/sys/vm/drop_caches`** (Cache Management):
   - For clearing memory caches when needed

### Q: What happens if a server goes down and it is the only server for 11 virtual machines?

**Answer:**
Single Point of Failure scenario:

**Immediate Impact:**
- All 11 VMs become unavailable simultaneously
- Complete service outage for applications running on those VMs
- Users lose access to all services hosted on that server
- Data in VM memory is lost

**Business Impact:**
- Service Level Agreement (SLA) violations
- Revenue loss due to downtime
- Customer dissatisfaction and potential churn
- Regulatory compliance issues (if applicable)

**Technical Consequences:**
- Applications may have inconsistent data states
- Database transactions might be incomplete
- Session data loss for active users
- Backup and monitoring systems may be affected

**Recovery Actions:**
1. **Immediate Assessment:**
   - Determine root cause (hardware failure, power, network)
   - Check if server can be restored quickly
   - Assess data integrity and backup status

2. **Recovery Options:**
   - Restore from recent VM backups on alternative hardware
   - Migrate VMs to other hosts (if using shared storage)
   - Rebuild VMs from infrastructure as code

3. **Communication:**
   - Notify stakeholders about outage and ETA
   - Update status pages and communication channels
   - Document incident for post-mortem

**Prevention Strategies:**
- Implement high availability clustering
- Use VM replication and failover
- Distribute VMs across multiple physical hosts
- Regular disaster recovery testing
- Implement proper monitoring and alerting

### Q: How would you try and repair your Wi-Fi before contacting support?

**Answer:**
Systematic Wi-Fi troubleshooting approach:

**Level 1 - Basic Troubleshooting:**
1. **Check Physical Connections:**
   - Ensure Wi-Fi is enabled on device
   - Check if airplane mode is off
   - Verify router/modem power and cables

2. **Network Adapter Reset:**
   - Disable and re-enable Wi-Fi adapter
   - Restart network services
   - Toggle Wi-Fi off/on in settings

3. **Restart Devices:**
   - Restart your device (computer/phone)
   - Power cycle router/modem (unplug 30 seconds)
   - Wait for full boot sequence

**Level 2 - Network Configuration:**
4. **Forget and Reconnect:**
   - Remove saved Wi-Fi network
   - Scan and reconnect with password
   - Check for typos in network name/password

5. **IP Configuration:**
   - Release and renew IP address
   - Try static IP configuration
   - Flush DNS cache (`ipconfig /flushdns` on Windows)

**Level 3 - Advanced Troubleshooting:**
6. **Driver Issues:**
   - Update Wi-Fi adapter drivers
   - Rollback recent driver updates
   - Reinstall network adapter

7. **Network Diagnostics:**
   - Run built-in network troubleshooter
   - Check signal strength and interference
   - Test with different devices

8. **Router Configuration:**
   - Access router admin panel
   - Check for firmware updates
   - Verify Wi-Fi settings and security type
   - Change Wi-Fi channel to avoid interference

9. **Alternative Testing:**
   - Test with mobile hotspot
   - Try ethernet connection
   - Connect to different Wi-Fi network

**Documentation:**
- Note specific error messages
- Record when problem started
- Document troubleshooting steps attempted
- Check if other devices are affected

## 4. Key File System Paths for an SRE

| Path | Purpose |
|------|---------|
| `/etc/` | Core system configuration files |
| `/var/log/` | The most common location for log files |
| `/proc/` | A virtual filesystem with real-time kernel and process info |
| `/home/` | Home directories for users |
| `/tmp/` | Temporary files that are often cleared on reboot |

## 5. System Services Management (systemd)

systemd is the standard for managing services on modern Linux.

| Command | What it does |
|---------|--------------|
| `systemctl status nginx` | Check the detailed status of a service |
| `systemctl restart nginx` | Stop and then start the service |
| `systemctl start nginx` | Start a stopped service |
| `systemctl stop nginx` | Stop a running service |
| `systemctl enable nginx` | Make the service start automatically on boot |
| `journalctl -u nginx -f` | Follow the logs for a specific service |
