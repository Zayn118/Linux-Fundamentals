# Package Management

## What is Package Management?

Package managers handle the installation, updating, and removal of software on Linux systems. They resolve dependencies automatically and maintain a database of installed software.

## Different Package Management Systems

### Debian-based Systems (Ubuntu, Debian, Mint)
- **Package Format**: .deb
- **Low-level Tool**: dpkg
- **High-level Tool**: apt, apt-get

### Red Hat-based Systems (RHEL, CentOS, Fedora)
- **Package Format**: .rpm
- **Low-level Tool**: rpm
- **High-level Tool**: yum, dnf

### Arch-based Systems
- **Package Manager**: pacman

### Universal Package Managers
- **Snap**: Works across distributions
- **Flatpak**: Works across distributions
- **AppImage**: Self-contained applications

## APT (Debian/Ubuntu)

### Update Package Lists
```bash
sudo apt update                  # Update package list from repositories
```

### Upgrade Packages
```bash
sudo apt upgrade                 # Upgrade all packages
sudo apt full-upgrade            # Upgrade with dependency resolution
sudo apt dist-upgrade            # Upgrade distribution (legacy)
```

### Install Packages
```bash
sudo apt install package_name    # Install package
sudo apt install package1 package2  # Install multiple packages
sudo apt install ./package.deb   # Install local .deb file
sudo apt install -y package      # Install without confirmation
```

### Remove Packages
```bash
sudo apt remove package_name     # Remove package (keep config files)
sudo apt purge package_name      # Remove package and config files
sudo apt autoremove              # Remove unnecessary dependencies
```

### Search Packages
```bash
apt search keyword               # Search for packages
apt-cache search keyword         # Alternative search
```

### Show Package Information
```bash
apt show package_name            # Show package details
apt-cache showpkg package_name   # Show package dependencies
```

### List Packages
```bash
apt list --installed             # List installed packages
apt list --upgradable            # List upgradable packages
apt list package_name            # Check if specific package is installed
```

### Clean Up
```bash
sudo apt clean                   # Remove downloaded package files
sudo apt autoclean               # Remove old downloaded packages
sudo apt autoremove              # Remove unused dependencies
```

## YUM/DNF (Red Hat/CentOS/Fedora)

DNF is the modern replacement for YUM (available on newer systems).

### Update Package Lists
```bash
sudo yum check-update            # Check for updates
sudo dnf check-update            # DNF version
```

### Upgrade Packages
```bash
sudo yum update                  # Update all packages
sudo dnf upgrade                 # DNF version (preferred)
```

### Install Packages
```bash
sudo yum install package_name    # Install package
sudo dnf install package_name    # DNF version
sudo yum install -y package      # Install without confirmation
sudo yum localinstall package.rpm  # Install local RPM
```

### Remove Packages
```bash
sudo yum remove package_name     # Remove package
sudo dnf remove package_name     # DNF version
sudo yum autoremove              # Remove unused dependencies
```

### Search Packages
```bash
yum search keyword               # Search for packages
dnf search keyword               # DNF version
```

### Show Package Information
```bash
yum info package_name            # Show package details
dnf info package_name            # DNF version
```

### List Packages
```bash
yum list installed               # List installed packages
dnf list installed               # DNF version
yum list available               # List available packages
yum list updates                 # List available updates
```

### Clean Up
```bash
sudo yum clean all               # Clean package cache
sudo dnf clean all               # DNF version
```

### Repository Management
```bash
yum repolist                     # List enabled repositories
yum-config-manager --add-repo URL  # Add repository
yum-config-manager --disable repo  # Disable repository
```

## Pacman (Arch Linux)

### Update System
```bash
sudo pacman -Syu                 # Update package list and upgrade all
sudo pacman -Sy                  # Update package list only
sudo pacman -Su                  # Upgrade packages only
```

### Install Packages
```bash
sudo pacman -S package_name      # Install package
sudo pacman -S package1 package2 # Install multiple packages
sudo pacman -U package.pkg.tar.zst  # Install local package
```

### Remove Packages
```bash
sudo pacman -R package_name      # Remove package
sudo pacman -Rs package_name     # Remove package and dependencies
sudo pacman -Rns package_name    # Remove package, deps, and config
```

### Search Packages
```bash
pacman -Ss keyword               # Search for packages
```

### Show Package Information
```bash
pacman -Si package_name          # Show package info
pacman -Qi package_name          # Show installed package info
```

### List Packages
```bash
pacman -Q                        # List installed packages
pacman -Qe                       # List explicitly installed
pacman -Qdt                      # List orphaned packages
```

### Clean Up
```bash
sudo pacman -Sc                  # Remove old packages from cache
sudo pacman -Scc                 # Remove all cached packages
```

## dpkg (Low-level Debian Package Manager)

```bash
dpkg -i package.deb              # Install .deb package
dpkg -r package_name             # Remove package
dpkg -l                          # List installed packages
dpkg -l | grep keyword           # Search installed packages
dpkg -L package_name             # List files in package
dpkg -s package_name             # Show package status
dpkg --contents package.deb      # Show contents of .deb file
```

## RPM (Low-level Red Hat Package Manager)

```bash
rpm -ivh package.rpm             # Install package
rpm -Uvh package.rpm             # Upgrade package
rpm -e package_name              # Remove package
rpm -qa                          # List all installed packages
rpm -qa | grep keyword           # Search installed packages
rpm -ql package_name             # List files in package
rpm -qi package_name             # Show package info
rpm -qf /path/to/file            # Find which package owns file
```

## Snap

Snap packages are self-contained and work across distributions.

### Install Snap
```bash
sudo apt install snapd           # Ubuntu/Debian
sudo yum install snapd           # CentOS/RHEL
```

### Snap Commands
```bash
snap find keyword                # Search for snaps
sudo snap install package_name   # Install snap
sudo snap remove package_name    # Remove snap
snap list                        # List installed snaps
sudo snap refresh                # Update all snaps
sudo snap refresh package_name   # Update specific snap
snap info package_name           # Show snap details
```

### Common Snaps
```bash
sudo snap install code --classic # VS Code
sudo snap install spotify        # Spotify
sudo snap install docker         # Docker
```

## Flatpak

Flatpak is another universal package format.

### Install Flatpak
```bash
sudo apt install flatpak         # Ubuntu/Debian
sudo yum install flatpak         # CentOS/RHEL
```

### Add Flathub Repository
```bash
flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
```

### Flatpak Commands
```bash
flatpak search keyword           # Search for apps
flatpak install flathub app_name # Install app from Flathub
flatpak uninstall app_name       # Uninstall app
flatpak list                     # List installed apps
flatpak update                   # Update all apps
flatpak run app_name             # Run app
```

## AppImage

AppImage files are self-contained executables.

### Using AppImage
```bash
chmod +x app.AppImage            # Make executable
./app.AppImage                   # Run application
```

No installation required, but you may want to integrate with system:
```bash
./app.AppImage --appimage-extract  # Extract AppImage contents
```

## Software Sources and Repositories

### Ubuntu/Debian - Adding PPAs
```bash
sudo add-apt-repository ppa:user/ppa-name  # Add PPA
sudo apt update                  # Update package list
sudo add-apt-repository --remove ppa:user/ppa-name  # Remove PPA
```

### Editing Sources List
```bash
sudo nano /etc/apt/sources.list  # Main sources file
ls /etc/apt/sources.list.d/      # Additional source files
```

### Red Hat/CentOS - Adding Repositories
```bash
sudo yum-config-manager --add-repo repository_url
```

Create repo file:
```bash
sudo nano /etc/yum.repos.d/repo-name.repo
```

## Common Tasks

### Install Development Tools

#### Ubuntu/Debian
```bash
sudo apt install build-essential # GCC, make, etc.
sudo apt install git             # Git version control
sudo apt install python3-pip     # Python package manager
sudo apt install nodejs npm      # Node.js and npm
```

#### CentOS/RHEL
```bash
sudo yum groupinstall "Development Tools"  # GCC, make, etc.
sudo yum install git             # Git version control
sudo yum install python3-pip     # Python package manager
sudo yum install nodejs npm      # Node.js and npm
```

### Check Package Version
```bash
apt-cache policy package_name    # Ubuntu/Debian
yum info package_name            # CentOS/RHEL
```

### Hold Package Version (Prevent Updates)
```bash
sudo apt-mark hold package_name  # Ubuntu/Debian
sudo apt-mark unhold package_name  # Allow updates again
sudo yum versionlock package_name  # CentOS/RHEL (requires plugin)
```

### Find Which Package Provides a File
```bash
dpkg -S /path/to/file            # Ubuntu/Debian
yum provides /path/to/file       # CentOS/RHEL
```

### Download Package Without Installing
```bash
apt download package_name        # Ubuntu/Debian
yumdownloader package_name       # CentOS/RHEL (requires yum-utils)
```

### Verify Package Integrity
```bash
debsums package_name             # Ubuntu/Debian (requires debsums)
rpm -V package_name              # CentOS/RHEL
```

## Troubleshooting

### Fix Broken Packages (Ubuntu/Debian)
```bash
sudo apt --fix-broken install    # Fix broken dependencies
sudo dpkg --configure -a         # Configure unconfigured packages
```

### Clear Package Manager Lock
```bash
sudo rm /var/lib/apt/lists/lock  # Ubuntu/Debian
sudo rm /var/cache/apt/archives/lock
sudo rm /var/lib/dpkg/lock*
```

### Refresh Package Database
```bash
sudo apt update                  # Ubuntu/Debian
sudo yum clean all && sudo yum makecache  # CentOS/RHEL
```

### Reinstall Package
```bash
sudo apt install --reinstall package_name  # Ubuntu/Debian
sudo yum reinstall package_name  # CentOS/RHEL
```

## Best Practices

1. **Always update before installing**: Run `apt update` or `yum check-update` first
2. **Be careful with PPAs**: Only use trusted sources
3. **Regular updates**: Keep your system updated for security
4. **Remove unused packages**: Use `apt autoremove` regularly
5. **Check dependencies**: Understand what's being installed
6. **Backup before major upgrades**: System upgrades can occasionally cause issues
7. **Use official repositories**: Avoid untrusted third-party repos when possible

## Quick Reference

### Ubuntu/Debian (apt)
```bash
sudo apt update                  # Update package list
sudo apt upgrade                 # Upgrade packages
sudo apt install package         # Install package
sudo apt remove package          # Remove package
sudo apt search keyword          # Search packages
apt show package                 # Show package info
sudo apt autoremove              # Remove unused dependencies
```

### CentOS/RHEL/Fedora (yum/dnf)
```bash
sudo yum update                  # Update all packages
sudo yum install package         # Install package
sudo yum remove package          # Remove package
yum search keyword               # Search packages
yum info package                 # Show package info
```

### Arch (pacman)
```bash
sudo pacman -Syu                 # Update system
sudo pacman -S package           # Install package
sudo pacman -R package           # Remove package
pacman -Ss keyword               # Search packages
```

### Universal
```bash
sudo snap install package        # Install snap
flatpak install package          # Install flatpak
```

## Next Steps

Learn about shell scripting to automate tasks and create powerful command combinations.
