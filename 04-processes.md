# Process Management

## What is a Process?

A process is an instance of a running program. Every time you execute a command or run an application, Linux creates a process for it.

## Process Attributes

- **PID (Process ID)**: Unique identifier for each process
- **PPID (Parent Process ID)**: PID of the process that started this process
- **User**: User who owns the process
- **CPU %**: Percentage of CPU usage
- **Memory %**: Percentage of memory usage
- **State**: Current state (running, sleeping, stopped, zombie)

## Viewing Processes

### ps - Process Status

#### Basic Usage
```bash
ps                               # Processes in current shell
ps -e                            # All processes
ps -ef                           # Full format listing
ps -eF                           # Extra full format
ps aux                           # BSD-style, detailed view
```

#### Filtering Processes
```bash
ps -u username                   # Processes for specific user
ps -C processname                # Processes by name
ps -p 1234                       # Specific PID
ps aux | grep processname        # Search for process
```

#### Output Columns Explained
- **USER**: Process owner
- **PID**: Process ID
- **%CPU**: CPU usage percentage
- **%MEM**: Memory usage percentage
- **VSZ**: Virtual memory size (KB)
- **RSS**: Resident set size (physical memory in KB)
- **TTY**: Controlling terminal
- **STAT**: Process state
- **START**: Start time
- **TIME**: CPU time consumed
- **COMMAND**: Command that started the process

### top - Real-time Process Monitor

```bash
top                              # Interactive process viewer
```

#### Interactive Commands (within top)
- **q**: Quit
- **k**: Kill a process
- **r**: Renice (change priority)
- **P**: Sort by CPU usage
- **M**: Sort by memory usage
- **h**: Help
- **1**: Show individual CPU cores
- **u**: Filter by user
- **c**: Show full command path

### htop - Enhanced Process Viewer

```bash
htop                             # Better interface than top (may need installation)
```

Features:
- Color-coded display
- Mouse support
- Tree view of processes
- Easy process killing

Install:
```bash
sudo apt install htop            # Debian/Ubuntu
sudo yum install htop            # CentOS/RHEL
```

## Process States

- **R (Running)**: Currently executing or ready to execute
- **S (Sleeping)**: Waiting for an event (interruptible)
- **D**: Sleeping (uninterruptible), usually waiting for I/O
- **T (Stopped)**: Stopped by job control signal
- **Z (Zombie)**: Terminated but not yet cleaned up by parent
- **< (High priority)**: Less nice process
- **N (Low priority)**: Nice process
- **s**: Session leader
- **l**: Multi-threaded
- **+**: Foreground process group

## Killing Processes

### kill - Send Signal to Process

```bash
kill PID                         # Terminate process (SIGTERM)
kill -9 PID                      # Force kill (SIGKILL)
kill -15 PID                     # Graceful termination (SIGTERM)
kill -STOP PID                   # Pause process
kill -CONT PID                   # Resume process
```

### killall - Kill by Name

```bash
killall processname              # Kill all processes with name
killall -9 processname           # Force kill all
killall -u username              # Kill all processes of user
```

### pkill - Advanced Process Killing

```bash
pkill processname                # Kill by name pattern
pkill -u username                # Kill all user processes
pkill -f pattern                 # Kill by full command line
pkill -9 processname             # Force kill
```

### Common Signals

- **SIGTERM (15)**: Graceful termination (default)
- **SIGKILL (9)**: Force kill (cannot be caught or ignored)
- **SIGHUP (1)**: Hangup (restart with config reload)
- **SIGINT (2)**: Interrupt (Ctrl+C)
- **SIGSTOP (19)**: Pause process
- **SIGCONT (18)**: Resume process

```bash
kill -l                          # List all signals
```

## Foreground vs Background Processes

### Running in Background
```bash
command &                        # Run command in background
```

Example:
```bash
sleep 100 &                      # Sleep for 100 seconds in background
```

### Jobs Management

```bash
jobs                             # List background jobs
jobs -l                          # List with PIDs
```

### Foreground/Background Control

```bash
fg                               # Bring last job to foreground
fg %1                            # Bring job 1 to foreground
bg                               # Resume last job in background
bg %1                            # Resume job 1 in background
```

### Suspend and Resume

- **Ctrl+Z**: Suspend foreground process
- **Ctrl+C**: Terminate foreground process

Example workflow:
```bash
vim document.txt                 # Start editing
# Press Ctrl+Z to suspend
jobs                             # See suspended job
fg                               # Resume editing
```

## Process Priority (Nice Values)

Nice values range from -20 (highest priority) to 19 (lowest priority). Default is 0.

### View Priority
```bash
ps -el                           # NI column shows nice value
top                              # NI column
```

### Start with Nice Value
```bash
nice -n 10 command               # Start with nice value 10
nice -n -10 command              # Start with nice value -10 (requires root)
```

### Change Priority of Running Process
```bash
renice 10 -p PID                 # Change nice value
renice 5 -u username             # Change for all user processes
```

**Note**: Only root can decrease nice values (increase priority).

## nohup - Run Process Immune to Hangups

`nohup` allows processes to continue running after you log out.

```bash
nohup command &                  # Run in background, immune to hangups
nohup command > output.log &     # Redirect output to file
```

Output is saved to `nohup.out` by default.

## Screen and tmux - Terminal Multiplexers

Keep sessions running after disconnect.

### Screen Basics
```bash
screen                           # Start new screen session
screen -S sessionname            # Start named session
screen -ls                       # List sessions
screen -r sessionname            # Reattach to session
```

Commands within screen:
- **Ctrl+A, D**: Detach from session
- **Ctrl+A, C**: Create new window
- **Ctrl+A, N**: Next window
- **Ctrl+A, P**: Previous window

### tmux Basics
```bash
tmux                             # Start new tmux session
tmux new -s sessionname          # Start named session
tmux ls                          # List sessions
tmux attach -t sessionname       # Attach to session
```

Commands within tmux:
- **Ctrl+B, D**: Detach
- **Ctrl+B, C**: New window
- **Ctrl+B, N**: Next window
- **Ctrl+B, %**: Split vertically
- **Ctrl+B, "**: Split horizontally

## Monitoring System Resources

### Free Memory
```bash
free -h                          # Human-readable memory usage
```

### CPU Information
```bash
lscpu                            # CPU information
cat /proc/cpuinfo                # Detailed CPU info
```

### System Uptime
```bash
uptime                           # System uptime and load average
```

### Load Average
Shows average system load over 1, 5, and 15 minutes.

```bash
uptime
# Output: 15:30:25 up 5 days, 3:45, 2 users, load average: 0.52, 0.58, 0.59
```

Rule of thumb: Load should be less than number of CPU cores.

### vmstat - Virtual Memory Statistics
```bash
vmstat 1                         # Update every second
vmstat 5 10                      # Update every 5 seconds, 10 times
```

### iostat - I/O Statistics
```bash
iostat                           # Disk I/O statistics
iostat -x 2                      # Extended stats, every 2 seconds
```

## Process Tree

### pstree - Show Process Hierarchy
```bash
pstree                           # Show process tree
pstree -p                        # Show PIDs
pstree -u                        # Show user changes
pstree username                  # Tree for specific user
```

### ps Tree Format
```bash
ps auxf                          # Forest view (process tree)
ps -ejH                          # Tree format
```

## System Logs for Process Issues

```bash
dmesg                            # Kernel ring buffer (system messages)
dmesg | tail                     # Recent kernel messages
dmesg | grep -i error            # Check for errors

journalctl -xe                   # System logs (systemd systems)
journalctl -u servicename        # Logs for specific service
journalctl -f                    # Follow logs in real-time
```

## Systemd Service Management

Systemd manages system services (daemons).

### Service Commands
```bash
systemctl status servicename     # Check service status
systemctl start servicename      # Start service
systemctl stop servicename       # Stop service
systemctl restart servicename    # Restart service
systemctl reload servicename     # Reload config without restart
systemctl enable servicename     # Enable at boot
systemctl disable servicename    # Disable at boot
systemctl list-units --type=service  # List all services
```

### Examples
```bash
systemctl status nginx           # Check nginx status
systemctl restart apache2        # Restart Apache
systemctl enable docker          # Enable Docker at boot
```

## Practical Scenarios

### Find Resource-Heavy Processes
```bash
ps aux --sort=-%cpu | head -10   # Top CPU consumers
ps aux --sort=-%mem | head -10   # Top memory consumers
```

### Kill Unresponsive Application
```bash
ps aux | grep appname            # Find PID
kill PID                         # Try graceful kill
kill -9 PID                      # Force kill if needed
```

### Run Long Process After Logout
```bash
nohup ./long-running-script.sh > output.log 2>&1 &
```

### Monitor Specific Process
```bash
watch -n 1 "ps aux | grep processname"  # Update every second
```

### Find Parent Process
```bash
ps -ef | grep PID                # Show process with PPID
pstree -p PID                    # Show process tree
```

## Troubleshooting

### Process Won't Die
```bash
kill -9 PID                      # Force kill
```

### Zombie Processes
Zombies are harmless but indicate parent isn't cleaning up. Kill the parent process.
```bash
ps -elf | grep defunct           # Find zombies
kill PPID                        # Kill parent process
```

### Too Many Processes
```bash
ps aux | wc -l                   # Count processes
ulimit -u                        # Check process limit
ulimit -u 2048                   # Set new limit
```

## Quick Reference

```bash
# Viewing
ps aux                           # All processes
top                              # Real-time monitor
htop                             # Better real-time monitor
pstree                           # Process tree

# Killing
kill PID                         # Terminate gracefully
kill -9 PID                      # Force kill
killall processname              # Kill all by name
pkill pattern                    # Kill by pattern

# Background/Foreground
command &                        # Run in background
jobs                             # List jobs
fg                               # Bring to foreground
bg                               # Resume in background
Ctrl+Z                           # Suspend current process

# Priority
nice -n 10 command               # Start with priority
renice 10 -p PID                 # Change priority

# Persistence
nohup command &                  # Run after logout
screen                           # Terminal multiplexer
tmux                             # Modern multiplexer

# Services
systemctl status service         # Check service
systemctl start service          # Start service
systemctl stop service           # Stop service
```

## Next Steps

Learn about networking commands to understand how to manage network connections and troubleshoot network issues.
