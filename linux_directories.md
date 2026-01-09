# Linux Root Directory Structure

## Core Directories

- **`/`** - Root directory, top of the filesystem hierarchy
- **`/bin`** - Essential user command binaries (ls, cp, cat, bash)
- **`/boot`** - Boot loader files, kernel, initrd
- **`/dev`** - Device files (hard drives, terminals, USB devices)
- **`/etc`** - System-wide configuration files
- **`/home`** - User home directories (/home/username)
- **`/lib`** - Essential shared libraries and kernel modules
- **`/media`** - Mount points for removable media (USB drives, CD-ROMs)
- **`/mnt`** - Temporary mount points for filesystems
- **`/opt`** - Optional/third-party software packages
- **`/proc`** - Virtual filesystem with process and kernel information
- **`/root`** - Root user's home directory
- **`/run`** - Runtime data (process IDs, sockets) - cleared on reboot
- **`/sbin`** - System binaries for administration (fsck, reboot, iptables)
- **`/srv`** - Service data (web server files, FTP data)
- **`/sys`** - Virtual filesystem for kernel and device information
- **`/tmp`** - Temporary files - cleared on reboot
- **`/usr`** - User programs and data (read-only)
  - **`/usr/bin`** - User commands and applications
  - **`/usr/lib`** - Libraries for /usr/bin programs
  - **`/usr/local`** - Locally installed software
  - **`/usr/share`** - Architecture-independent data (docs, icons)
- **`/var`** - Variable data that changes during operation
  - **`/var/log`** - Log files
  - **`/var/cache`** - Application cache
  - **`/var/lib`** - State information (databases, package data)
  - **`/var/tmp`** - Temporary files - preserved across reboots

## Quick Reference

| Directory | Purpose | Example |
|-----------|---------|---------|
| `/etc` | Configuration | `/etc/environment`, `/etc/passwd` |
| `/opt` | Third-party apps | `/opt/google/chrome` |
| `/usr` | Programs & libraries | `/usr/lib/jvm/java-17` |
| `/var` | Changing data | `/var/log/syslog` |
| `/home` | User files | `/home/midserver` |
| `/tmp` | Temp files (cleared) | `/tmp/session-123` |
