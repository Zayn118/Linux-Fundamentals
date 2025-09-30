# Linux File System

## File System Hierarchy

Linux follows a hierarchical directory structure, starting from the root directory `/`. Everything in Linux is either a file or a directory.

### Important Directories

- **`/`** - Root directory, the top of the file system hierarchy
- **`/home`** - User home directories (e.g., `/home/username`)
- **`/root`** - Home directory for the root user
- **`/etc`** - System configuration files
- **`/var`** - Variable data (logs, temporary files, caches)
- **`/tmp`** - Temporary files (cleared on reboot)
- **`/usr`** - User programs and data
- **`/bin`** - Essential user command binaries
- **`/sbin`** - System binaries (usually require root)
- **`/opt`** - Optional/third-party software
- **`/dev`** - Device files (hardware representations)
- **`/proc`** - Process information (virtual filesystem)
- **`/mnt`** - Mount points for temporary filesystems
- **`/media`** - Mount points for removable media

## Navigation Commands

### Print Working Directory
```bash
pwd
```
Shows your current location in the file system

### Change Directory
```bash
cd /path/to/directory    # Absolute path
cd directory_name         # Relative path
cd ..                     # Go up one level
cd ~                      # Go to home directory
cd -                      # Go to previous directory
cd                        # Go to home directory (shortcut)
```

### List Files and Directories
```bash
ls                        # List files in current directory
ls -l                     # Long format (detailed info)
ls -a                     # Show hidden files (starting with .)
ls -lh                    # Human-readable file sizes
ls -la                    # Combine options
ls -lt                    # Sort by modification time
ls -lS                    # Sort by size
ls /path/to/directory     # List specific directory
```

## File Operations

### Create Files
```bash
touch filename.txt        # Create empty file
echo "text" > file.txt    # Create file with content
```

### Create Directories
```bash
mkdir directory_name      # Create single directory
mkdir -p path/to/nested   # Create nested directories
```

### Copy Files and Directories
```bash
cp source.txt dest.txt           # Copy file
cp -r source_dir dest_dir        # Copy directory recursively
cp -i source.txt dest.txt        # Interactive (prompt before overwrite)
cp -v source.txt dest.txt        # Verbose (show what's being done)
```

### Move/Rename Files and Directories
```bash
mv oldname.txt newname.txt       # Rename file
mv file.txt /path/to/directory/  # Move file
mv -i source dest                # Interactive mode
mv directory1 directory2         # Rename/move directory
```

### Delete Files and Directories
```bash
rm filename.txt                  # Delete file
rm -i filename.txt               # Interactive deletion
rm -f filename.txt               # Force deletion (no confirmation)
rm -r directory_name             # Delete directory recursively
rm -rf directory_name            # Force recursive deletion (DANGEROUS!)
rmdir empty_directory            # Remove empty directory
```

**⚠️ Warning**: Be extremely careful with `rm -rf` - it permanently deletes files without confirmation!

## Viewing File Contents

### Display Entire File
```bash
cat filename.txt                 # Display file contents
cat file1.txt file2.txt          # Concatenate multiple files
```

### View File with Pagination
```bash
less filename.txt                # View file (navigate with arrows, q to quit)
more filename.txt                # Similar to less, but less feature-rich
```

### View Beginning/End of File
```bash
head filename.txt                # First 10 lines
head -n 20 filename.txt          # First 20 lines
tail filename.txt                # Last 10 lines
tail -n 20 filename.txt          # Last 20 lines
tail -f logfile.log              # Follow file (watch for updates)
```

## Searching and Finding

### Find Files
```bash
find /path -name "filename"      # Find by name
find /path -name "*.txt"         # Find with wildcard
find /path -type f               # Find only files
find /path -type d               # Find only directories
find /path -size +100M           # Find files larger than 100MB
find /path -mtime -7             # Modified in last 7 days
```

### Locate Files (faster, uses database)
```bash
locate filename                  # Quick search
updatedb                         # Update locate database (requires root)
```

### Search Inside Files
```bash
grep "pattern" filename          # Search for pattern
grep -r "pattern" directory      # Recursive search
grep -i "pattern" filename       # Case-insensitive
grep -n "pattern" filename       # Show line numbers
grep -v "pattern" filename       # Invert match (lines NOT matching)
```

## Wildcards and Patterns

- **`*`** - Matches any characters (e.g., `*.txt` matches all .txt files)
- **`?`** - Matches single character (e.g., `file?.txt` matches file1.txt, file2.txt)
- **`[]`** - Matches any character in brackets (e.g., `file[12].txt`)
- **`{}`** - Brace expansion (e.g., `file{1,2,3}.txt`)

### Examples
```bash
ls *.txt                         # All .txt files
ls file?.txt                     # file1.txt, file2.txt, etc.
ls [abc]*                        # Files starting with a, b, or c
cp file{1,2,3}.txt /backup/      # Copy multiple specific files
```

## File Information

### File Stats
```bash
stat filename                    # Detailed file information
file filename                    # Determine file type
```

### Disk Usage
```bash
du -h filename                   # File size
du -sh directory                 # Directory size summary
du -h --max-depth=1              # Size of subdirectories
df -h                            # Disk space usage (all filesystems)
```

## Symbolic and Hard Links

### Symbolic Links (Symlinks)
```bash
ln -s /path/to/original linkname # Create symbolic link
ls -l linkname                   # View link target
```

### Hard Links
```bash
ln original_file hard_link       # Create hard link
```

**Difference**: Symlinks are pointers to files (like shortcuts), while hard links are direct references to the same file data.

## Hidden Files

In Linux, files starting with a dot (`.`) are hidden by default.

```bash
ls -a                            # Show hidden files
```

Common hidden files:
- `.bashrc` - Bash configuration
- `.bash_history` - Command history
- `.ssh/` - SSH configuration directory
- `.gitconfig` - Git configuration

## Compression and Archives

### Creating Archives
```bash
tar -cvf archive.tar directory   # Create tar archive
tar -czvf archive.tar.gz dir     # Create compressed tar.gz
tar -cjvf archive.tar.bz2 dir    # Create compressed tar.bz2
zip -r archive.zip directory     # Create zip archive
```

### Extracting Archives
```bash
tar -xvf archive.tar             # Extract tar
tar -xzvf archive.tar.gz         # Extract tar.gz
tar -xjvf archive.tar.bz2        # Extract tar.bz2
unzip archive.zip                # Extract zip
```

### Viewing Archive Contents
```bash
tar -tvf archive.tar             # List contents without extracting
```

## Tips

1. **Use Tab completion** to avoid typos and save time
2. **Be careful with rm -rf** - there's no trash/recycle bin
3. **Use relative paths** when working in the same area
4. **Check current directory** with `pwd` before destructive operations
5. **Use `-i` flag** for interactive confirmations with cp, mv, rm

## Next Steps

Learn about file permissions and ownership to control access to your files and directories.
