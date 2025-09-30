# File Permissions and Ownership

## Understanding Linux Permissions

Linux is a multi-user system with a robust permission system. Every file and directory has permissions that control who can read, write, or execute them.

## Permission Types

There are three types of permissions:

- **Read (r)** - Can view file contents or list directory contents
- **Write (w)** - Can modify file contents or create/delete files in directory
- **Execute (x)** - Can run file as program or enter directory

## Permission Categories

Permissions are assigned to three categories of users:

- **User (u)** - The file owner
- **Group (g)** - Users in the file's group
- **Others (o)** - Everyone else
- **All (a)** - All of the above

## Viewing Permissions

```bash
ls -l filename
```

Output format:
```
-rwxr-xr-- 1 user group 1234 Jan 15 10:30 filename
│└┬┘└┬┘└┬┘   └──┬─┘ └─┬──┘ └──┬─┘ └────┬────┘ └──┬───┘
│ │  │  │       │     │       │         │         │
│ │  │  │       │     │       │         │         filename
│ │  │  │       │     │       │         date/time
│ │  │  │       │     │       file size
│ │  │  │       │     group
│ │  │  │       owner
│ │  │  other permissions (r--)
│ │  group permissions (r-x)
│ user permissions (rwx)
file type (-=file, d=directory, l=link)
```

### File Type Indicators
- **-** Regular file
- **d** Directory
- **l** Symbolic link
- **c** Character device
- **b** Block device
- **s** Socket
- **p** Named pipe

## Permission Notation

### Symbolic Notation
- **r** = read (4)
- **w** = write (2)
- **x** = execute (1)
- **-** = no permission (0)

### Numeric (Octal) Notation
Each permission set is represented by a number (sum of values):
- **7** = rwx (4+2+1)
- **6** = rw- (4+2)
- **5** = r-x (4+1)
- **4** = r-- (4)
- **3** = -wx (2+1)
- **2** = -w- (2)
- **1** = --x (1)
- **0** = --- (0)

Example: **755** = rwxr-xr-x
- Owner: 7 (rwx)
- Group: 5 (r-x)
- Others: 5 (r-x)

## Changing Permissions (chmod)

### Using Symbolic Mode
```bash
chmod u+x filename               # Add execute for user
chmod g-w filename               # Remove write for group
chmod o+r filename               # Add read for others
chmod a+x filename               # Add execute for all
chmod u+x,g-w filename           # Multiple changes
chmod u=rwx,g=rx,o=r filename    # Set exact permissions
```

### Using Numeric Mode
```bash
chmod 755 filename               # rwxr-xr-x
chmod 644 filename               # rw-r--r--
chmod 700 filename               # rwx------
chmod 666 filename               # rw-rw-rw-
chmod 600 filename               # rw-------
chmod 777 filename               # rwxrwxrwx (not recommended!)
```

### Recursive Changes
```bash
chmod -R 755 directory           # Apply to directory and all contents
```

## Common Permission Patterns

### Files
- **644** (rw-r--r--) - Standard file (owner can edit, others read)
- **600** (rw-------) - Private file (only owner can read/write)
- **755** (rwxr-xr-x) - Executable file (owner can modify, all can execute)
- **700** (rwx------) - Private executable

### Directories
- **755** (rwxr-xr-x) - Standard directory (owner full access, others can enter/list)
- **700** (rwx------) - Private directory (only owner can access)
- **775** (rwxrwxr-x) - Collaborative directory (group has full access)

### Special Cases
- **666** (rw-rw-rw-) - Shared writable file (rarely needed)
- **777** (rwxrwxrwx) - Everything for everyone (security risk!)

## File Ownership

### Viewing Ownership
```bash
ls -l filename
```

### Changing Owner (chown)
```bash
chown newuser filename           # Change owner
chown newuser:newgroup filename  # Change owner and group
chown -R newuser directory       # Recursive
```

**Note**: Usually requires root privileges (`sudo`)

### Changing Group (chgrp)
```bash
chgrp newgroup filename          # Change group
chgrp -R newgroup directory      # Recursive
```

## Special Permissions

### Setuid (Set User ID)
When set on executable files, the program runs with the permissions of the file owner.
```bash
chmod u+s filename               # Add setuid
chmod 4755 filename              # Numeric notation (4 = setuid)
```
Display: `rws` in owner execute position

### Setgid (Set Group ID)
When set on executables, runs with group permissions. On directories, new files inherit the directory's group.
```bash
chmod g+s filename               # Add setgid
chmod 2755 filename              # Numeric notation (2 = setgid)
```
Display: `rws` or `rwS` in group execute position

### Sticky Bit
On directories, only file owners can delete their own files (useful for shared directories like `/tmp`).
```bash
chmod +t directory               # Add sticky bit
chmod 1755 directory             # Numeric notation (1 = sticky bit)
```
Display: `t` or `T` in others execute position

### Examples
```bash
ls -ld /tmp                      # drwxrwxrwt (sticky bit)
ls -l /usr/bin/passwd            # -rwsr-xr-x (setuid)
```

## Default Permissions (umask)

The `umask` command sets default permissions for new files and directories.

### View Current umask
```bash
umask                            # Display current umask (e.g., 0022)
umask -S                         # Symbolic notation
```

### How umask Works
- Default permissions: 666 for files, 777 for directories
- Actual permissions: default - umask

Example with umask 0022:
- Files: 666 - 022 = 644 (rw-r--r--)
- Directories: 777 - 022 = 755 (rwxr-xr-x)

### Set umask
```bash
umask 0027                       # Files: 640, Directories: 750
umask 0077                       # Files: 600, Directories: 700 (private)
```

Add to `.bashrc` or `.profile` to make permanent.

## Access Control Lists (ACLs)

ACLs provide more fine-grained permissions beyond the standard user/group/other model.

### View ACLs
```bash
getfacl filename
```

### Set ACLs
```bash
setfacl -m u:username:rwx filename    # Give user specific permissions
setfacl -m g:groupname:rx filename    # Give group specific permissions
setfacl -m d:u:username:rwx dir       # Set default ACL for directory
setfacl -x u:username filename        # Remove ACL entry
setfacl -b filename                   # Remove all ACLs
```

## Practical Examples

### Secure SSH Keys
```bash
chmod 700 ~/.ssh                 # Secure SSH directory
chmod 600 ~/.ssh/id_rsa          # Private key (read/write owner only)
chmod 644 ~/.ssh/id_rsa.pub      # Public key (readable by all)
chmod 600 ~/.ssh/authorized_keys # Authorized keys file
```

### Web Server Files
```bash
chmod 755 /var/www/html          # Directory accessible
chmod 644 /var/www/html/*.html   # Files readable
chmod 755 /var/www/cgi-bin/*.cgi # CGI scripts executable
```

### Shared Project Directory
```bash
mkdir /shared/project
chgrp developers /shared/project
chmod 2775 /shared/project       # Group can write, setgid for inheritance
```

### Make Script Executable
```bash
chmod +x script.sh               # Make script executable
./script.sh                      # Run the script
```

## Troubleshooting Permission Issues

### Permission Denied Errors
```bash
ls -l filename                   # Check permissions
whoami                           # Check your username
groups                           # Check your groups
```

### Can't Execute Script
```bash
chmod +x script.sh               # Add execute permission
```

### Can't Access Directory
```bash
chmod +x directory               # Need execute permission to enter
```

### Can't Create Files in Directory
```bash
ls -ld directory                 # Check directory permissions
chmod u+w directory              # Add write permission
```

## Security Best Practices

1. **Principle of Least Privilege**: Give only necessary permissions
2. **Avoid 777**: Almost never needed and creates security holes
3. **Protect Private Files**: Use 600 or 700 for sensitive data
4. **Regular Audits**: Check permissions on critical files
5. **Use Groups**: Organize permissions with groups rather than world-readable files
6. **Secure Home Directory**: Keep `~` at 755 or 700
7. **Be Careful with Setuid**: Potential security risk if misused

## Quick Reference

```bash
# Viewing
ls -l                            # View permissions
ls -ld directory                 # View directory permissions

# Changing permissions
chmod 755 file                   # Numeric
chmod u+x file                   # Symbolic

# Changing ownership
sudo chown user:group file       # Change owner and group

# Special permissions
chmod u+s file                   # Setuid
chmod g+s directory              # Setgid
chmod +t directory               # Sticky bit

# Default permissions
umask                            # View umask
umask 0022                       # Set umask
```

## Next Steps

Learn about process management to understand how to monitor and control running programs.
