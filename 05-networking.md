# Networking Commands

## Network Configuration

### IP Address Information

#### ip command (modern)
```bash
ip addr show                     # Show all IP addresses
ip a                             # Short form
ip addr show eth0                # Show specific interface
ip link show                     # Show network interfaces
```

#### ifconfig (legacy, but still common)
```bash
ifconfig                         # Show all interfaces
ifconfig eth0                    # Show specific interface
ifconfig eth0 up                 # Enable interface
ifconfig eth0 down               # Disable interface
```

### Hostname

```bash
hostname                         # Display hostname
hostname -I                      # Display all IP addresses
hostnamectl                      # Detailed hostname info (systemd)
hostnamectl set-hostname newname # Set new hostname (requires root)
```

## Network Testing

### Ping - Test Connectivity
```bash
ping example.com                 # Continuous ping (Ctrl+C to stop)
ping -c 4 example.com            # Ping 4 times
ping -i 2 example.com            # Ping every 2 seconds
ping -s 1000 example.com         # Ping with packet size 1000 bytes
ping 192.168.1.1                 # Ping IP address
```

### Traceroute - Trace Network Path
```bash
traceroute example.com           # Show route to destination
traceroute -n example.com        # No DNS resolution (faster)
tracepath example.com            # Alternative to traceroute
```

### MTR - Network Diagnostic Tool
Combines ping and traceroute functionality.
```bash
mtr example.com                  # Interactive network diagnostic
mtr -n example.com               # No DNS resolution
mtr -r example.com               # Report mode
```

## DNS Tools

### nslookup - Query DNS
```bash
nslookup example.com             # Basic DNS lookup
nslookup example.com 8.8.8.8     # Query specific DNS server
```

### dig - DNS Lookup Tool
```bash
dig example.com                  # Detailed DNS query
dig example.com +short           # Brief output (just IP)
dig example.com MX               # Query MX records
dig example.com ANY              # Query all records
dig @8.8.8.8 example.com         # Query specific DNS server
dig -x 8.8.8.8                   # Reverse DNS lookup
```

### host - Simple DNS Lookup
```bash
host example.com                 # Simple DNS lookup
host -t MX example.com           # Query specific record type
```

## Port and Connection Testing

### netstat - Network Statistics (legacy)
```bash
netstat -tuln                    # Show listening ports
netstat -tun                     # Show established connections
netstat -r                       # Show routing table
netstat -i                       # Show interface statistics
netstat -s                       # Show protocol statistics
```

Flags explained:
- **-t**: TCP
- **-u**: UDP
- **-l**: Listening
- **-n**: Numeric (no name resolution)
- **-p**: Show program/PID

### ss - Socket Statistics (modern replacement for netstat)
```bash
ss -tuln                         # Show listening ports
ss -tun                          # Show established connections
ss -s                            # Show statistics summary
ss -tp                           # Show processes
ss -o state established          # Show established connections
ss -o state listening            # Show listening sockets
```

### lsof - List Open Files (includes network connections)
```bash
lsof -i                          # Show all network connections
lsof -i :80                      # Show what's using port 80
lsof -i TCP                      # Show TCP connections
lsof -i UDP                      # Show UDP connections
lsof -i @192.168.1.1             # Show connections to specific IP
```

### telnet - Test Port Connection
```bash
telnet example.com 80            # Test if port 80 is open
```

### nc (netcat) - Network Swiss Army Knife
```bash
nc -zv example.com 80            # Test if port is open
nc -zv example.com 20-100        # Scan port range
nc -l 8080                       # Listen on port 8080
echo "test" | nc example.com 80  # Send data to port
```

### nmap - Network Mapper (Security Tool)
```bash
nmap example.com                 # Scan common ports
nmap -p 1-65535 example.com      # Scan all ports
nmap -p 80,443 example.com       # Scan specific ports
nmap -sV example.com             # Detect service versions
nmap -O example.com              # Detect OS
nmap 192.168.1.0/24              # Scan entire subnet
```

**Note**: Requires installation and should only be used on networks you own or have permission to scan.

## Downloading Files

### wget - Download Files
```bash
wget https://example.com/file.zip          # Download file
wget -O newname.zip https://example.com/file.zip  # Save with new name
wget -c https://example.com/file.zip       # Continue interrupted download
wget -b https://example.com/file.zip       # Download in background
wget --limit-rate=200k https://example.com/file.zip  # Limit speed
wget -r https://example.com/                # Recursive download
```

### curl - Transfer Data
```bash
curl https://example.com                    # Display page content
curl -o file.html https://example.com       # Save to file
curl -O https://example.com/file.zip        # Save with original name
curl -L https://example.com                 # Follow redirects
curl -I https://example.com                 # Show headers only
curl -X POST https://api.example.com        # POST request
curl -d "param=value" https://api.example.com  # POST with data
curl -H "Header: value" https://example.com # Custom header
```

## Routing

### View Routing Table
```bash
ip route show                    # Display routing table
route -n                         # Display routing table (legacy)
netstat -rn                      # Display routing table (legacy)
```

### Add/Delete Routes (requires root)
```bash
ip route add 192.168.2.0/24 via 192.168.1.1  # Add route
ip route del 192.168.2.0/24                  # Delete route
ip route add default via 192.168.1.1         # Set default gateway
```

## Network Configuration Files

### Debian/Ubuntu
```bash
cat /etc/network/interfaces      # Network configuration
cat /etc/resolv.conf             # DNS configuration
cat /etc/hosts                   # Static host mappings
```

### Red Hat/CentOS
```bash
cat /etc/sysconfig/network-scripts/ifcfg-eth0  # Interface config
cat /etc/resolv.conf             # DNS configuration
cat /etc/hosts                   # Static host mappings
```

### Modern Systems (systemd-networkd)
```bash
cat /etc/netplan/*.yaml          # Ubuntu 18.04+ network config
```

## Firewall Management

### UFW (Uncomplicated Firewall - Ubuntu/Debian)
```bash
sudo ufw status                  # Check status
sudo ufw enable                  # Enable firewall
sudo ufw disable                 # Disable firewall
sudo ufw allow 22                # Allow port 22
sudo ufw allow 80/tcp            # Allow port 80 TCP
sudo ufw deny 25                 # Block port 25
sudo ufw delete allow 80         # Remove rule
sudo ufw allow from 192.168.1.0/24  # Allow from subnet
```

### firewalld (Red Hat/CentOS)
```bash
sudo firewall-cmd --state        # Check status
sudo firewall-cmd --list-all     # List all rules
sudo firewall-cmd --add-port=80/tcp --permanent  # Add port
sudo firewall-cmd --remove-port=80/tcp --permanent  # Remove port
sudo firewall-cmd --reload       # Reload firewall
```

### iptables (Low-level firewall)
```bash
sudo iptables -L                 # List rules
sudo iptables -A INPUT -p tcp --dport 80 -j ACCEPT  # Allow port 80
sudo iptables -D INPUT -p tcp --dport 80 -j ACCEPT  # Remove rule
sudo iptables-save > /tmp/rules  # Save rules
sudo iptables-restore < /tmp/rules  # Restore rules
```

## Network Performance

### iperf - Network Bandwidth Testing
```bash
# On server
iperf -s                         # Start server

# On client
iperf -c server_ip               # Test bandwidth to server
iperf -c server_ip -t 30         # Test for 30 seconds
iperf -c server_ip -u            # UDP test
```

### Bandwidth Monitoring
```bash
iftop                            # Interactive bandwidth monitor
nethogs                          # Per-process bandwidth monitor
bmon                             # Bandwidth monitor with graphs
vnstat                           # Network traffic monitor
```

Install these tools:
```bash
sudo apt install iftop nethogs bmon vnstat  # Ubuntu/Debian
sudo yum install iftop nethogs bmon vnstat  # CentOS/RHEL
```

## Network Interface Management

### Bring Interface Up/Down
```bash
sudo ip link set eth0 up         # Enable interface
sudo ip link set eth0 down       # Disable interface
sudo ifup eth0                   # Enable (legacy)
sudo ifdown eth0                 # Disable (legacy)
```

### Assign IP Address
```bash
sudo ip addr add 192.168.1.100/24 dev eth0  # Assign IP
sudo ip addr del 192.168.1.100/24 dev eth0  # Remove IP
```

### Change MAC Address
```bash
sudo ip link set eth0 down
sudo ip link set eth0 address aa:bb:cc:dd:ee:ff
sudo ip link set eth0 up
```

## Network Services

### SSH - Secure Shell
```bash
ssh user@hostname                # Connect to remote host
ssh -p 2222 user@hostname        # Connect on specific port
ssh user@hostname command        # Run command on remote host
ssh -i keyfile user@hostname     # Use specific key file
ssh-keygen                       # Generate SSH key pair
ssh-copy-id user@hostname        # Copy SSH key to server
```

### SCP - Secure Copy
```bash
scp file.txt user@host:/path/    # Copy to remote
scp user@host:/path/file.txt .   # Copy from remote
scp -r directory user@host:/path/  # Copy directory
scp -P 2222 file.txt user@host:/path/  # Use specific port
```

### rsync - Remote Sync
```bash
rsync -avz source/ user@host:/dest/  # Sync directories
rsync -avz --delete source/ dest/    # Sync and delete extras
rsync -avz -e ssh source/ user@host:/dest/  # Over SSH
```

Flags:
- **-a**: Archive mode (preserves permissions, timestamps)
- **-v**: Verbose
- **-z**: Compress during transfer
- **--delete**: Delete files in dest not in source

## Network Troubleshooting

### Common Connectivity Issues

#### Can't reach internet
```bash
ping 8.8.8.8                     # Test if network works (DNS-independent)
ping google.com                  # Test if DNS works
cat /etc/resolv.conf             # Check DNS servers
ip route show                    # Check default gateway
```

#### Slow connection
```bash
ping -c 10 google.com            # Check latency
mtr google.com                   # Check for packet loss
traceroute google.com            # Find where delay occurs
```

#### Port not accessible
```bash
nc -zv hostname port             # Test port connectivity
telnet hostname port             # Alternative test
sudo ss -tuln | grep port        # Check if locally listening
sudo ufw status                  # Check firewall rules
```

#### DNS issues
```bash
nslookup example.com             # Test DNS resolution
dig example.com                  # Detailed DNS query
cat /etc/resolv.conf             # Check DNS configuration
```

### View Network Statistics
```bash
ip -s link                       # Interface statistics
cat /proc/net/dev                # Network device stats
netstat -i                       # Interface statistics
```

## Advanced Topics

### Network Namespaces
```bash
ip netns list                    # List network namespaces
ip netns add ns1                 # Create namespace
ip netns exec ns1 command        # Run command in namespace
```

### VLAN Configuration
```bash
ip link add link eth0 name eth0.10 type vlan id 10  # Create VLAN
ip addr add 192.168.10.1/24 dev eth0.10  # Assign IP to VLAN
```

### Bridge Configuration
```bash
ip link add br0 type bridge      # Create bridge
ip link set eth0 master br0      # Add interface to bridge
```

## Quick Reference

```bash
# Configuration
ip addr show                     # Show IP addresses
hostname -I                      # Show IPs

# Testing
ping example.com                 # Test connectivity
traceroute example.com           # Trace route
mtr example.com                  # Combined ping/traceroute

# DNS
dig example.com                  # DNS lookup
nslookup example.com             # DNS query

# Ports and Connections
ss -tuln                         # Listening ports
lsof -i :80                      # What's using port 80
nc -zv host port                 # Test port

# Downloading
wget URL                         # Download file
curl -O URL                      # Download with curl

# Remote Access
ssh user@host                    # SSH connection
scp file user@host:/path/        # Secure copy
rsync -avz source/ dest/         # Sync directories

# Monitoring
iftop                            # Bandwidth monitor
nethogs                          # Per-process bandwidth
```

## Next Steps

Learn about package management to understand how to install, update, and remove software on your Linux system.
