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

## 3. Key File System Paths for an SRE

| Path | Purpose |
|------|---------|
| `/etc/` | Core system configuration files |
| `/var/log/` | The most common location for log files |
| `/proc/` | A virtual filesystem with real-time kernel and process info |
| `/home/` | Home directories for users |
| `/tmp/` | Temporary files that are often cleared on reboot |

## 4. System Services Management (systemd)

systemd is the standard for managing services on modern Linux.

| Command | What it does |
|---------|--------------|
| `systemctl status nginx` | Check the detailed status of a service |
| `systemctl restart nginx` | Stop and then start the service |
| `systemctl start nginx` | Start a stopped service |
| `systemctl stop nginx` | Stop a running service |
| `systemctl enable nginx` | Make the service start automatically on boot |
| `journalctl -u nginx -f` | Follow the logs for a specific service |
