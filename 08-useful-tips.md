# Useful Tips for DevOps

## Text Processing and Analysis

### grep - Pattern Matching Master
```bash
# Find configuration errors
grep -i "error" /var/log/syslog

# Recursive search
grep -r "TODO" /path/to/code

# Show line numbers
grep -n "pattern" file.txt

# Context lines
grep -A 3 "pattern" file.txt    # 3 lines after
grep -B 3 "pattern" file.txt    # 3 lines before
grep -C 3 "pattern" file.txt    # 3 lines before and after

# Invert match (exclude)
grep -v "pattern" file.txt

# Count matches
grep -c "pattern" file.txt

# Multiple patterns
grep -E "pattern1|pattern2" file.txt
```

### awk - Text Processing Powerhouse
```bash
# Print specific columns
awk '{print $1, $3}' file.txt

# Column with custom delimiter
awk -F: '{print $1, $3}' /etc/passwd

# Filter and print
awk '$3 > 1000 {print $1}' file.txt

# Sum column values
awk '{sum += $1} END {print sum}' numbers.txt

# Format output
awk '{printf "%-10s %5s\n", $1, $2}' file.txt

# Use patterns
awk '/pattern/ {print $0}' file.txt
```

### sed - Stream Editor
```bash
# Replace text
sed 's/old/new/' file.txt           # First occurrence per line
sed 's/old/new/g' file.txt          # All occurrences
sed 's/old/new/gi' file.txt         # Case-insensitive

# In-place editing
sed -i 's/old/new/g' file.txt

# Delete lines
sed '/pattern/d' file.txt           # Delete matching lines
sed '5d' file.txt                   # Delete line 5
sed '1,3d' file.txt                 # Delete lines 1-3

# Print specific lines
sed -n '5p' file.txt                # Print line 5
sed -n '1,10p' file.txt             # Print lines 1-10

# Multiple commands
sed -e 's/old/new/g' -e 's/foo/bar/g' file.txt
```

### cut - Extract Columns
```bash
# By delimiter
cut -d: -f1 /etc/passwd             # First field
cut -d: -f1,3 /etc/passwd           # Fields 1 and 3
cut -d: -f1-3 /etc/passwd           # Fields 1 through 3

# By character position
cut -c1-5 file.txt                  # Characters 1-5
```

### sort - Sort Lines
```bash
sort file.txt                       # Alphabetical sort
sort -r file.txt                    # Reverse sort
sort -n file.txt                    # Numeric sort
sort -k2 file.txt                   # Sort by 2nd column
sort -u file.txt                    # Sort and remove duplicates
sort -t: -k3 -n /etc/passwd         # Sort by 3rd field numerically
```

### uniq - Remove Duplicates
```bash
uniq file.txt                       # Remove adjacent duplicates
uniq -c file.txt                    # Count occurrences
uniq -d file.txt                    # Show only duplicates
sort file.txt | uniq                # Remove all duplicates
```

### tr - Translate Characters
```bash
tr 'a-z' 'A-Z' < file.txt          # Lowercase to uppercase
tr -d '[:digit:]' < file.txt       # Delete digits
tr -s ' ' < file.txt               # Squeeze repeated spaces
```

### wc - Word Count
```bash
wc file.txt                         # Lines, words, characters
wc -l file.txt                      # Count lines
wc -w file.txt                      # Count words
wc -c file.txt                      # Count bytes
```

## Command Chaining and Pipes

### Operators
```bash
command1 ; command2                 # Run sequentially
command1 && command2                # Run command2 if command1 succeeds
command1 || command2                # Run command2 if command1 fails
command1 | command2                 # Pipe output
command1 & command2                 # Run in background
```

### Powerful Pipelines
```bash
# Find largest files
du -h | sort -h | tail -10

# Count unique IP addresses in log
cat access.log | awk '{print $1}' | sort | uniq -c | sort -rn

# Process monitoring
ps aux | grep "process" | awk '{print $2}' | xargs kill

# Find and replace in multiple files
find . -name "*.txt" -exec sed -i 's/old/new/g' {} \;

# Top 10 commands from history
history | awk '{print $2}' | sort | uniq -c | sort -rn | head -10
```

## Aliases and Functions

### Creating Aliases
```bash
# In ~/.bashrc or ~/.bash_aliases

alias ll='ls -lah'
alias ..='cd ..'
alias ...='cd ../..'
alias grep='grep --color=auto'
alias update='sudo apt update && sudo apt upgrade'
alias ports='netstat -tuln'
alias myip='curl ifconfig.me'
alias c='clear'
```

### Useful Functions
```bash
# In ~/.bashrc

# Extract archives automatically
extract() {
    if [ -f $1 ]; then
        case $1 in
            *.tar.bz2)   tar xjf $1     ;;
            *.tar.gz)    tar xzf $1     ;;
            *.bz2)       bunzip2 $1     ;;
            *.rar)       unrar x $1     ;;
            *.gz)        gunzip $1      ;;
            *.tar)       tar xf $1      ;;
            *.tbz2)      tar xjf $1     ;;
            *.tgz)       tar xzf $1     ;;
            *.zip)       unzip $1       ;;
            *.Z)         uncompress $1  ;;
            *.7z)        7z x $1        ;;
            *)           echo "'$1' cannot be extracted" ;;
        esac
    else
        echo "'$1' is not a valid file"
    fi
}

# Create directory and cd into it
mkcd() {
    mkdir -p "$1" && cd "$1"
}

# Find process by name
psgrep() {
    ps aux | grep -v grep | grep -i -e VSZ -e $1
}

# Backup file with timestamp
backup() {
    cp "$1" "$1.backup-$(date +%Y%m%d-%H%M%S)"
}
```

## Git Shortcuts (for DevOps)

```bash
# Aliases in ~/.gitconfig or command line

git config --global alias.st status
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.unstage 'reset HEAD --'
git config --global alias.last 'log -1 HEAD'
git config --global alias.lg "log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"
```

## Docker Helpers

### Docker Aliases
```bash
alias dps='docker ps'
alias dpsa='docker ps -a'
alias di='docker images'
alias dex='docker exec -it'
alias drm='docker rm'
alias drmi='docker rmi'
alias dstop='docker stop $(docker ps -a -q)'
alias dclean='docker system prune -af'
```

### Docker Functions
```bash
# Enter running container
denter() {
    docker exec -it $1 /bin/bash
}

# Remove all stopped containers
dclean-containers() {
    docker rm $(docker ps -a -q -f status=exited)
}

# Remove dangling images
dclean-images() {
    docker rmi $(docker images -f dangling=true -q)
}

# Container logs
dlog() {
    docker logs -f $1
}
```

## System Monitoring One-Liners

```bash
# Top 10 memory consuming processes
ps aux --sort=-%mem | head -11

# Top 10 CPU consuming processes
ps aux --sort=-%cpu | head -11

# Check disk usage by directory
du -h --max-depth=1 / | sort -h

# Monitor disk I/O
iostat -x 2

# Watch network connections
watch -n 1 'netstat -tuln'

# Monitor log file in real-time
tail -f /var/log/syslog

# Check open files by process
lsof -p <PID>

# Show all listening ports with process
sudo netstat -tulpn

# Memory usage by process
ps aux | awk '{print $6/1024 " MB\t" $11}' | sort -n

# Count files in directory
find . -type f | wc -l

# Find large files (over 100MB)
find / -type f -size +100M -exec ls -lh {} \; 2>/dev/null
```

## File and Directory Management

### Quick Navigation
```bash
cd -                                # Go to previous directory
pushd /path && popd                 # Save and restore directory

# In ~/.bashrc
export CDPATH=".:~:~/projects"      # Search paths for cd
```

### Batch Operations
```bash
# Rename multiple files
for file in *.txt; do
    mv "$file" "${file%.txt}.md"
done

# Mass permission change
find /path -type f -exec chmod 644 {} \;
find /path -type d -exec chmod 755 {} \;

# Find and delete files older than 30 days
find /path -type f -mtime +30 -delete

# Copy structure without files
find /source -type d -exec mkdir -p /dest/{} \;
```

## SSH Productivity

### SSH Config (~/.ssh/config)
```
Host myserver
    HostName 192.168.1.100
    User username
    Port 2222
    IdentityFile ~/.ssh/id_rsa

Host github
    HostName github.com
    User git
    IdentityFile ~/.ssh/github_key
```

Usage: `ssh myserver` (instead of `ssh username@192.168.1.100 -p 2222`)

### SSH Tunneling
```bash
# Local port forwarding
ssh -L 8080:localhost:80 user@remote

# Remote port forwarding
ssh -R 9090:localhost:3000 user@remote

# Dynamic SOCKS proxy
ssh -D 8080 user@remote

# Keep connection alive
ssh -o ServerAliveInterval=60 user@remote
```

### SSH Key Management
```bash
# Generate key
ssh-keygen -t ed25519 -C "email@example.com"

# Copy key to server
ssh-copy-id user@server

# Test connection
ssh -T git@github.com

# Use specific key
ssh -i ~/.ssh/specific_key user@server
```

## Environment Variables

### Useful Variables
```bash
export EDITOR=vim                   # Default editor
export VISUAL=vim                   # Visual editor
export HISTSIZE=10000               # Command history size
export HISTTIMEFORMAT="%F %T "      # Add timestamp to history
export HISTCONTROL=ignoredups       # Ignore duplicate commands
```

### PATH Management
```bash
# Add to PATH
export PATH="$HOME/bin:$PATH"
export PATH="$PATH:/usr/local/bin"

# Show PATH nicely
echo $PATH | tr ':' '\n'
```

## Performance and Benchmarking

```bash
# Time command execution
time command

# Measure disk write speed
dd if=/dev/zero of=/tmp/test bs=1M count=1024

# Measure disk read speed
dd if=/tmp/test of=/dev/null bs=1M

# HTTP request timing
curl -w "@curl-format.txt" -o /dev/null -s "http://example.com"

# Network throughput test (requires iperf3)
iperf3 -s                           # Server
iperf3 -c server_ip                 # Client
```

## JSON/YAML Processing

### jq - JSON Processor
```bash
# Pretty print JSON
cat data.json | jq '.'

# Extract field
cat data.json | jq '.field'

# Filter array
cat data.json | jq '.[] | select(.age > 30)'

# Map values
cat data.json | jq '.[] | .name'

# API response parsing
curl -s api.example.com/data | jq '.results[].name'
```

### yq - YAML Processor
```bash
# Read YAML value
yq eval '.key' file.yaml

# Update YAML
yq eval '.key = "new value"' -i file.yaml

# Convert YAML to JSON
yq eval -o json file.yaml
```

## Process Management Tricks

```bash
# Find process using port
lsof -i :8080
netstat -tulpn | grep :8080

# Kill all processes by name
pkill -9 process_name

# Monitor process
watch -n 1 'ps aux | grep process'

# Restart process when it dies
until myprocess; do
    echo "Process crashed. Restarting..." >&2
    sleep 1
done

# Run command with timeout
timeout 5s command

# Run command with nice priority
nice -n 10 command

# Limit memory usage
ulimit -v 1000000  # 1GB in KB
```

## Cron Job Management

### Crontab Syntax
```
# ┌───────────── minute (0-59)
# │ ┌───────────── hour (0-23)
# │ │ ┌───────────── day of month (1-31)
# │ │ │ ┌───────────── month (1-12)
# │ │ │ │ ┌───────────── day of week (0-6, Sunday = 0)
# │ │ │ │ │
# * * * * * command

# Edit crontab
crontab -e

# List crontab
crontab -l

# Remove crontab
crontab -r
```

### Common Schedules
```bash
0 * * * * command               # Every hour
*/15 * * * * command            # Every 15 minutes
0 0 * * * command               # Daily at midnight
0 2 * * 0 command               # Weekly on Sunday at 2am
0 0 1 * * command               # Monthly on 1st at midnight
@reboot command                 # At startup
@daily command                  # Daily
@hourly command                 # Hourly
```

## Systemd Timers (Modern Alternative to Cron)

```bash
# List timers
systemctl list-timers

# Create timer unit
sudo nano /etc/systemd/system/mytask.timer

# Create service unit
sudo nano /etc/systemd/system/mytask.service

# Enable and start timer
sudo systemctl enable --now mytask.timer
```

## Log Management

```bash
# View logs with journalctl (systemd)
journalctl -f                   # Follow logs
journalctl -u service           # Logs for service
journalctl --since "1 hour ago" # Recent logs
journalctl -p err               # Only errors
journalctl -b                   # Current boot logs

# Rotate logs manually
logrotate -f /etc/logrotate.conf

# Clear journal logs
journalctl --vacuum-time=7d     # Keep 7 days
journalctl --vacuum-size=1G     # Keep 1GB
```

## Quick Debugging

```bash
# Check why command failed
command; echo $?                # Check exit code

# Verbose mode
bash -x script.sh               # Debug script
curl -v https://example.com     # Verbose HTTP

# Trace system calls
strace command

# Trace library calls
ltrace command

# Check loaded libraries
ldd /path/to/binary

# Check file type
file filename
```

## Security Best Practices

```bash
# Check sudo permissions
sudo -l

# Find files with SUID bit
find / -perm -4000 -type f 2>/dev/null

# Check for rootkits
sudo rkhunter --check
sudo chkrootkit

# Scan for open ports
nmap localhost

# Check failed login attempts
sudo grep "Failed password" /var/log/auth.log

# Lock user account
sudo passwd -l username

# Generate random password
openssl rand -base64 32

# Hash password
echo -n "password" | sha256sum
```

## Productivity Shortcuts

### Bash Keyboard Shortcuts
```
Ctrl+A          # Move to beginning of line
Ctrl+E          # Move to end of line
Ctrl+U          # Delete from cursor to beginning
Ctrl+K          # Delete from cursor to end
Ctrl+W          # Delete word before cursor
Ctrl+R          # Search command history
Ctrl+L          # Clear screen
Ctrl+C          # Cancel current command
Ctrl+D          # Logout/exit
Ctrl+Z          # Suspend process
Alt+.           # Paste last argument of previous command
!!              # Repeat last command
!$              # Last argument of previous command
!^              # First argument of previous command
```

## Quick Reference

### Essential One-Liners
```bash
# Replace in files
find . -name "*.txt" -exec sed -i 's/old/new/g' {} \;

# Count code lines
find . -name "*.py" -exec wc -l {} \; | awk '{sum+=$1} END {print sum}'

# Compress directory
tar -czf archive.tar.gz directory/

# Monitor file changes
watch -d -n 2 'ls -l file.txt'

# Compare directories
diff -r dir1/ dir2/

# Show directory tree
tree -L 2

# Disk usage by directory
du -sh * | sort -h

# Follow symbolic links
readlink -f symlink
```

These tips and tricks will significantly improve your efficiency when working with Linux, especially in DevOps environments!
