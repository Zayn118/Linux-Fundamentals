# Getting Started with Linux

## What is Linux?

Linux is a free and open-source operating system kernel that powers millions of servers, desktops, and embedded devices worldwide. It's essential for computer scientists, especially those pursuing careers in DevOps, system administration, and software development.

## Why Learn Linux?

- **Industry Standard**: Most servers and cloud infrastructure run on Linux
- **DevOps Essential**: Critical for CI/CD pipelines, containerization (Docker), and orchestration (Kubernetes)
- **Free & Open Source**: No licensing costs, full control over the system
- **Security**: Robust permission system and active security community
- **Customizable**: Tailor the system to your exact needs

## Popular Linux Distributions

### For Beginners:
- **Ubuntu**: User-friendly, great community support, excellent for desktop and server
- **Linux Mint**: Based on Ubuntu, great for those transitioning from Windows
- **Pop!_OS**: Optimized for developers and content creators

### For Servers/DevOps:
- **CentOS/Rocky Linux**: Enterprise-grade, stable, used in many production environments
- **Debian**: Rock-solid stability, extensive package repository
- **Ubuntu Server**: Popular for cloud deployments and containers

### For Advanced Users:
- **Arch Linux**: Minimalist, rolling release, complete customization
- **Fedora**: Cutting-edge features, sponsored by Red Hat

## Basic Linux Concepts

### The Shell
The shell is a command-line interface that allows you to interact with the Linux system. The most common shell is **Bash** (Bourne Again Shell). Alternatives: `zsh`, `fish`, `ksh`.

My favourite shell was zsh, due to its powerful features, such as tab completion via a menu, and how easily customizable it is, with plug-ins such as Oh-My-Zsh! 
I installed this with the following command:
 ```bash
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```
I recommend going for a custom theme such as Powerlevel10k, which had a really clean look, and you could choose how verbose you wanted the information the shell provides you to be.

### Terminal
A terminal (or terminal emulator) is an application that provides access to the shell.

### Root User
The root user (also called superuser) has complete control over the system. For security, most tasks should be performed as a regular user, using `sudo` when elevated privileges are needed.

## Getting Help

### Man Pages (Manual Pages)
```bash
man command_name
```
Example: `man ls` - Shows the manual for the ls command

### Info Pages
```bash
info command_name
```
More detailed than man pages for some commands

### Help Option
```bash
command_name --help
```
Quick reference for command options

### Online Resources
- Official documentation for your distribution
- StackOverflow and Unix & Linux Stack Exchange
- Linux man pages online
- Community forums and IRC channels

## Your First Commands

### Check Your Username
```bash
whoami
```

### Check Your Current Directory
```bash
pwd
```
(Print Working Directory)

### List Files
```bash
ls
```

### List Command History
```bash
history
```
Use `!!` to grab the last command or `!` with its number from the command history

### Clear the Screen
```bash
clear
```
Or press `Ctrl + L`

### Exit the Terminal
```bash
exit
```

## Tips for Beginners

1. **Tab Completion**: Press Tab to auto-complete commands and file names
2. **Command History**: Use Up/Down arrows to navigate through previous commands
3. **Ctrl+C**: Stop a running command
4. **Ctrl+D**: End of input/exit shell
5. **Ctrl+R**: Search command history
6. **Don't Panic**: Linux is forgiving if you're not running as root

## Next Steps

Once you're comfortable with these basics, proceed to learning about the Linux file system structure and navigation commands.
