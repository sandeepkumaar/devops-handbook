## User & Groups
- Every Server will have Users and Groups
- Softwares create groups on installation with the necessary roles to run commands, other only root user can run commands
- Users can be added to the group so they dont need root privileges

> Add a User to Software Group is a common thing in CICD for non-user to execute commands for security(least privileges)

## Commands 
### List All Users
```
#All users (from /etc/passwd)
cat /etc/passwd

# Just usernames
cut -d: -f1 /etc/passwd

# Or
getent passwd
```
### List All Groups
```
# All groups (from /etc/group)
cat /etc/group

# Just group names
cut -d: -f1 /etc/group

# Or
getent group
```
### List groups for a specific user
```
# Groups for sandeep user
groups sandeep

# Or more detailed
id sandeep
```
### List members of a specific group
```
# Members of docker group
getent group docker

# Or
grep docker /etc/group
```
### Switch User
```
# switch to root user
su - 

# switch to node user
su - node # switch to node user with user's login session.
su node # switch to node user but retains the current shell env session.
```
Note: '-' flag is used load the user's environment. ie. $HOME=/node etc. 

### Create a User & password
```
useradd -m -s /bin/bash sandeep
-m : creates a home directory 'sandeep'
-s : default shell

passwd some-passoword
```
### Add a user to group
```
usermod -aG <group> <username>
-aG: add to Group
```

### User permission to file/directory
```
chown -R <user>:<group> <file|directory>
-R : Recursively
```

### Check File|Directory permission

## Basic Commands

```bash
# Long listing format (shows permissions)
ls -l /path/to/file

# For directories
ls -ld /path/to/directory

# Detailed view with human-readable sizes
ls -lh /path/to/file

# Show all files including hidden
ls -la /path/to/directory
```

## Reading the Output

```bash
-rwxr-xr-x 1 username groupname 4096 Jan 10 15:30 script.sh
```

Breaking it down:
- **`-rwxr-xr-x`** - Permissions (explained below)
- **`1`** - Number of hard links
- **`username`** - Owner (user)
- **`groupname`** - Group
- **`4096`** - Size in bytes
- **`Jan 10 15:30`** - Last modified date/time
- **`script.sh`** - Filename

## Permission Breakdown

```
-rwxr-xr-x
│└┬┘└┬┘└┬┘
│ │  │  └─ Others permissions (r-x = read, execute)
│ │  └──── Group permissions (r-x = read, execute)
│ └─────── Owner permissions (rwx = read, write, execute)
└───────── File type (- = file, d = directory, l = link)
```

## Permission Meanings

- **`r`** - Read (4)
- **`w`** - Write (2)
- **`x`** - Execute (1)
- **`-`** - No permission

## File Type Indicators

- **`-`** - Regular file
- **`d`** - Directory
- **`l`** - Symbolic link
- **`b`** - Block device
- **`c`** - Character device
- **`s`** - Socket
- **`p`** - Named pipe

## Common Permission Patterns

| Permissions | Numeric | Meaning |
|-------------|---------|---------|
| `-rw-------` | 600 | Owner can read/write, no one else |
| `-rw-r--r--` | 644 | Owner can read/write, others can read |
| `-rwxr-xr-x` | 755 | Owner can read/write/execute, others can read/execute |
| `-rwx------` | 700 | Owner can read/write/execute, no one else |
| `drwxr-xr-x` | 755 | Directory: Owner full access, others can read/list |

## Examples

```bash
# Check script permissions
ls -la /home/user/app/bin/script.sh
# -rwxr-xr-x 1 user user 12345 Jan 10 15:30 script.sh
# Owner can read/write/execute, group and others can read/execute

# Check directory permissions
ls -ld /home/user/app
# drwxr-xr-x 5 user user 4096 Jan 10 15:30 /home/user/app
# Directory: Owner full access, group and others can read/list

# Check config file permissions
ls -l /home/user/app/config.xml
# -rw-r--r-- 1 user user 2048 Jan 10 15:30 config.xml
# Owner can read/write, group and others can only read
```

## Additional Commands

```bash
# Check ownership and permissions recursively
ls -lR /path/to/directory

# Show numeric permissions
stat -c '%a %n' /path/to/file
# Output: 755 /path/to/file

# Detailed file information
stat /path/to/file

# Check who you are (current user)
whoami

# Check your groups
groups

# Check if you can access a file
test -r /path/to/file && echo "Readable" || echo "Not readable"
test -w /path/to/file && echo "Writable" || echo "Not writable"
test -x /path/to/file && echo "Executable" || echo "Not executable"
```
