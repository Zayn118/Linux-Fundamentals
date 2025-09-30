# Shell Scripting Basics

## What is Shell Scripting?

Shell scripting allows you to automate tasks by writing sequences of commands in a file. Bash (Bourne Again Shell) is the most common shell for scripting on Linux.

## Your First Script

### Creating a Script

```bash
#!/bin/bash
# This is a comment
echo "Hello, World!"
```

### Making it Executable

```bash
chmod +x script.sh
./script.sh
```

### The Shebang Line

The first line `#!/bin/bash` tells the system which interpreter to use:
- `#!/bin/bash` - Bash shell
- `#!/bin/sh` - POSIX shell (more portable)
- `#!/usr/bin/env bash` - Find bash in PATH (more portable)
- `#!/usr/bin/python3` - Python script
- `#!/usr/bin/perl` - Perl script

## Variables

### Defining Variables
```bash
#!/bin/bash

# Variable assignment (no spaces around =)
NAME="John"
AGE=25
PATH_TO_FILE="/home/user/file.txt"

# Using variables (with $ prefix)
echo "My name is $NAME"
echo "I am $AGE years old"
echo "File: ${PATH_TO_FILE}"  # Curly braces for clarity
```

### Reading User Input
```bash
#!/bin/bash

echo "What is your name?"
read NAME
echo "Hello, $NAME!"

# Read with prompt
read -p "Enter your age: " AGE
echo "You are $AGE years old"

# Read password (hidden input)
read -sp "Enter password: " PASSWORD
echo  # New line after hidden input
```

### Command Substitution
```bash
#!/bin/bash

# Store command output in variable
CURRENT_DATE=$(date)
USER_COUNT=$(who | wc -l)

echo "Today is: $CURRENT_DATE"
echo "Logged in users: $USER_COUNT"

# Alternative syntax (backticks, older style)
FILES=`ls -l | wc -l`
echo "Files: $FILES"
```

### Special Variables
```bash
#!/bin/bash

echo "Script name: $0"
echo "First argument: $1"
echo "Second argument: $2"
echo "All arguments: $@"
echo "Number of arguments: $#"
echo "Exit status of last command: $?"
echo "Process ID: $$"
echo "User: $USER"
echo "Home directory: $HOME"
```

## Conditionals

### If Statement
```bash
#!/bin/bash

if [ condition ]; then
    # commands
fi
```

### If-Else
```bash
#!/bin/bash

if [ condition ]; then
    # commands if true
else
    # commands if false
fi
```

### If-Elif-Else
```bash
#!/bin/bash

if [ condition1 ]; then
    # commands
elif [ condition2 ]; then
    # commands
else
    # commands
fi
```

### Comparison Operators

#### Numeric Comparisons
```bash
-eq  # Equal to
-ne  # Not equal to
-gt  # Greater than
-ge  # Greater than or equal to
-lt  # Less than
-le  # Less than or equal to
```

Example:
```bash
#!/bin/bash

AGE=20

if [ $AGE -ge 18 ]; then
    echo "Adult"
else
    echo "Minor"
fi
```

#### String Comparisons
```bash
=    # Equal to
!=   # Not equal to
-z   # String is empty
-n   # String is not empty
```

Example:
```bash
#!/bin/bash

NAME="John"

if [ "$NAME" = "John" ]; then
    echo "Hello John!"
fi

if [ -z "$NAME" ]; then
    echo "Name is empty"
fi
```

#### File Tests
```bash
-e file  # File exists
-f file  # Regular file exists
-d file  # Directory exists
-r file  # File is readable
-w file  # File is writable
-x file  # File is executable
-s file  # File exists and is not empty
```

Example:
```bash
#!/bin/bash

FILE="/etc/passwd"

if [ -f "$FILE" ]; then
    echo "$FILE exists"
fi

if [ -r "$FILE" ]; then
    echo "$FILE is readable"
fi
```

### Logical Operators
```bash
&&   # AND
||   # OR
!    # NOT
```

Example:
```bash
#!/bin/bash

AGE=20
NAME="John"

if [ $AGE -ge 18 ] && [ "$NAME" = "John" ]; then
    echo "Adult named John"
fi

if [ $AGE -lt 18 ] || [ $AGE -gt 65 ]; then
    echo "Special age category"
fi
```

### Case Statement
```bash
#!/bin/bash

read -p "Enter a choice (1-3): " CHOICE

case $CHOICE in
    1)
        echo "You chose option 1"
        ;;
    2)
        echo "You chose option 2"
        ;;
    3)
        echo "You chose option 3"
        ;;
    *)
        echo "Invalid choice"
        ;;
esac
```

## Loops

### For Loop
```bash
#!/bin/bash

# Loop through list
for COLOR in red green blue; do
    echo "Color: $COLOR"
done

# Loop through files
for FILE in *.txt; do
    echo "Processing: $FILE"
done

# C-style for loop
for ((i=1; i<=5; i++)); do
    echo "Number: $i"
done

# Loop through command output
for USER in $(cat /etc/passwd | cut -d: -f1); do
    echo "User: $USER"
done
```

### While Loop
```bash
#!/bin/bash

COUNT=1

while [ $COUNT -le 5 ]; do
    echo "Count: $COUNT"
    COUNT=$((COUNT + 1))
done

# Read file line by line
while read LINE; do
    echo "Line: $LINE"
done < file.txt
```

### Until Loop
```bash
#!/bin/bash

COUNT=1

until [ $COUNT -gt 5 ]; do
    echo "Count: $COUNT"
    COUNT=$((COUNT + 1))
done
```

### Loop Control
```bash
#!/bin/bash

for i in {1..10}; do
    if [ $i -eq 5 ]; then
        continue  # Skip iteration
    fi
    
    if [ $i -eq 8 ]; then
        break     # Exit loop
    fi
    
    echo "Number: $i"
done
```

## Functions

### Defining Functions
```bash
#!/bin/bash

# Method 1
function greet() {
    echo "Hello, $1!"
}

# Method 2
say_goodbye() {
    echo "Goodbye, $1!"
}

# Call functions
greet "John"
say_goodbye "Jane"
```

### Function with Return Value
```bash
#!/bin/bash

add_numbers() {
    local NUM1=$1
    local NUM2=$2
    local SUM=$((NUM1 + NUM2))
    echo $SUM  # Return via echo
}

RESULT=$(add_numbers 5 3)
echo "Sum: $RESULT"
```

### Function with Exit Status
```bash
#!/bin/bash

check_file() {
    if [ -f "$1" ]; then
        return 0  # Success
    else
        return 1  # Failure
    fi
}

if check_file "/etc/passwd"; then
    echo "File exists"
else
    echo "File not found"
fi
```

## Arrays

### Defining Arrays
```bash
#!/bin/bash

# Method 1
COLORS=("red" "green" "blue")

# Method 2
NUMBERS[0]=10
NUMBERS[1]=20
NUMBERS[2]=30
```

### Accessing Array Elements
```bash
#!/bin/bash

COLORS=("red" "green" "blue")

echo "${COLORS[0]}"      # First element
echo "${COLORS[@]}"      # All elements
echo "${#COLORS[@]}"     # Array length
echo "${COLORS[@]:1:2}"  # Slice (from index 1, 2 elements)
```

### Iterating Over Arrays
```bash
#!/bin/bash

COLORS=("red" "green" "blue")

for COLOR in "${COLORS[@]}"; do
    echo "Color: $COLOR"
done

# With index
for i in "${!COLORS[@]}"; do
    echo "Index $i: ${COLORS[$i]}"
done
```

## Arithmetic Operations

```bash
#!/bin/bash

# Using $(( ))
NUM1=10
NUM2=5

SUM=$((NUM1 + NUM2))
DIFF=$((NUM1 - NUM2))
PRODUCT=$((NUM1 * NUM2))
QUOTIENT=$((NUM1 / NUM2))
REMAINDER=$((NUM1 % NUM2))

echo "Sum: $SUM"
echo "Difference: $DIFF"

# Increment/Decrement
COUNT=0
((COUNT++))
echo "Count: $COUNT"

# Using expr (older method)
SUM=$(expr $NUM1 + $NUM2)
echo "Sum: $SUM"

# Using let
let RESULT=NUM1+NUM2
echo "Result: $RESULT"
```

## String Operations

```bash
#!/bin/bash

STRING="Hello World"

# Length
echo "Length: ${#STRING}"

# Substring
echo "${STRING:0:5}"     # "Hello"
echo "${STRING:6}"       # "World"

# Replace
echo "${STRING/World/Universe}"  # Replace first occurrence
echo "${STRING//o/0}"            # Replace all occurrences

# Uppercase/Lowercase
echo "${STRING^^}"       # HELLO WORLD
echo "${STRING,,}"       # hello world

# Remove prefix/suffix
FILENAME="document.txt"
echo "${FILENAME%.txt}"  # document (remove .txt)
echo "${FILENAME#doc}"   # ument.txt (remove doc)
```

## Input/Output Redirection

```bash
#!/bin/bash

# Redirect output to file
echo "Hello" > file.txt          # Overwrite
echo "World" >> file.txt         # Append

# Redirect error
command 2> error.log             # Redirect stderr
command > output.log 2>&1        # Redirect both stdout and stderr

# Redirect input
while read LINE; do
    echo "$LINE"
done < input.txt

# Here document
cat << EOF > file.txt
Line 1
Line 2
Line 3
EOF

# Pipe
cat file.txt | grep "pattern" | sort | uniq
```

## Error Handling

### Exit Status
```bash
#!/bin/bash

command

if [ $? -eq 0 ]; then
    echo "Command succeeded"
else
    echo "Command failed"
fi
```

### Set Options
```bash
#!/bin/bash

set -e  # Exit on error
set -u  # Exit on undefined variable
set -o pipefail  # Exit on pipe failure

# Or combine
set -euo pipefail
```

### Trap
```bash
#!/bin/bash

cleanup() {
    echo "Cleaning up..."
    rm -f /tmp/tempfile
}

trap cleanup EXIT

# Script continues
echo "Running script..."
```

## Practical Examples

### Backup Script
```bash
#!/bin/bash

SOURCE="/home/user/documents"
DEST="/backup"
DATE=$(date +%Y%m%d)
BACKUP_FILE="backup_$DATE.tar.gz"

echo "Starting backup..."

if [ ! -d "$SOURCE" ]; then
    echo "Source directory not found!"
    exit 1
fi

tar -czf "$DEST/$BACKUP_FILE" "$SOURCE"

if [ $? -eq 0 ]; then
    echo "Backup successful: $BACKUP_FILE"
else
    echo "Backup failed!"
    exit 1
fi
```

### System Information Script
```bash
#!/bin/bash

echo "=== System Information ==="
echo "Hostname: $(hostname)"
echo "Uptime: $(uptime -p)"
echo "Current User: $USER"
echo "Current Date: $(date)"
echo ""
echo "=== CPU Info ==="
echo "CPU: $(lscpu | grep 'Model name' | cut -d: -f2 | xargs)"
echo ""
echo "=== Memory Info ==="
free -h
echo ""
echo "=== Disk Usage ==="
df -h /
```

### File Organizer
```bash
#!/bin/bash

SOURCE_DIR="$HOME/Downloads"
cd "$SOURCE_DIR" || exit

# Create directories
mkdir -p Images Documents Videos Archives

# Move files
for FILE in *; do
    if [ -f "$FILE" ]; then
        case "$FILE" in
            *.jpg|*.png|*.gif)
                mv "$FILE" Images/
                ;;
            *.pdf|*.doc|*.txt)
                mv "$FILE" Documents/
                ;;
            *.mp4|*.avi|*.mkv)
                mv "$FILE" Videos/
                ;;
            *.zip|*.tar|*.gz)
                mv "$FILE" Archives/
                ;;
        esac
    fi
done

echo "Files organized!"
```

### Menu Script
```bash
#!/bin/bash

while true; do
    clear
    echo "=== System Menu ==="
    echo "1. Show disk usage"
    echo "2. Show memory usage"
    echo "3. Show current users"
    echo "4. Exit"
    echo ""
    read -p "Enter choice: " CHOICE
    
    case $CHOICE in
        1)
            df -h
            read -p "Press Enter to continue..."
            ;;
        2)
            free -h
            read -p "Press Enter to continue..."
            ;;
        3)
            who
            read -p "Press Enter to continue..."
            ;;
        4)
            echo "Goodbye!"
            exit 0
            ;;
        *)
            echo "Invalid choice!"
            sleep 2
            ;;
    esac
done
```

## Best Practices

1. **Use meaningful variable names**: `USER_NAME` instead of `x`
2. **Quote variables**: `"$VAR"` to handle spaces
3. **Check for errors**: Use `set -e` or check exit codes
4. **Comment your code**: Explain what complex sections do
5. **Use functions**: Break complex scripts into functions
6. **Make scripts executable**: `chmod +x script.sh`
7. **Use shellcheck**: Lint your scripts with `shellcheck script.sh`
8. **Handle signals**: Use trap for cleanup
9. **Test thoroughly**: Test edge cases and errors

## Debugging

```bash
#!/bin/bash

# Enable debug mode
set -x  # Print commands before execution
set -v  # Print input lines as they are read

# Or run with
bash -x script.sh
```

## Quick Reference

```bash
# Variables
VAR="value"
echo "$VAR"

# Conditionals
if [ condition ]; then
    # code
fi

# Loops
for item in list; do
    # code
done

while [ condition ]; do
    # code
done

# Functions
function_name() {
    # code
}

# Arrays
ARRAY=("item1" "item2")
echo "${ARRAY[@]}"

# Arithmetic
RESULT=$((5 + 3))

# Command substitution
OUTPUT=$(command)
```

## Next Steps

Learn useful tips and tricks specific to DevOps workflows and advanced Linux usage.
