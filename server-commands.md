# uptime

**Uptime** is a command that returns information about how long your system has been running together with the **current time**, **number of users with running sessions**, and **the system load averages** for the past **1**, **5**, and **15** minutes. It can also filter the information displayed at once depending on your specified options.

## System load averages

The average number of processes that are in a runnable or uninterruptable state.

1. A process is in a runnable state when it is using the CPU or waiting to use the CPU
2. A process is in an uninterruptable state when it is waiting for I/O access like waiting for a disk

### Load average

| Value  | Time period |
| ------ | ----------- |
| First  | 1 minute    |
| Second | 5 minutes   |
| Third  | 15 minutes  |

**Load average interpretation (4 CPU cores):**

- `1.00` – Roughly one CPU's worth of load.
- `4.00` – Roughly four CPUs' worth of load.
- `8.00` – More work waiting than the CPUs can handle.

#### Example

```bash
23:16:49 up  10:49,  5 user,  load average: 1.00, 0.40, 3.35
```

On a single core system this would mean:

- The CPU was fully (100%) utilized on average; 1 processes was running on the CPU (1.00) over the last 1 minute.
- The CPU was idle by 60% on average; no processes were waiting for CPU time (0.40) over the last 5 minutes.
- The CPU was overloaded by 235% on average; 2.35 processes were waiting for CPU time (3.35) over the last 15 minutes.

On a dual-core system this would mean:

- The one CPU was 100% idle on average, one CPU was being used; no processes were waiting for CPU time(1.00) over the last 1 minute.
- The CPUs were idle by 160% on average; no processes were waiting for CPU time. (0.40) over the last 5 minutes.
- The CPUs were overloaded by 135% on average; 1.35 processes were waiting for CPU time. (3.35) over the last 15 minutes.

## Flags

| Command  | Description                                            |
| -------- | ------------------------------------------------------ |
| `uptime` | Show system uptime, logged-in users, and load averages |
| `-p`     | Show uptime in a human-readable format                 |
| `-s`     | Show exact date and time the system booted             |
| `-V`     | Show version information                               |

# nproc

| `nproc` | Show the number of available CPU processing units |
| `lscpu` | Display information about the CPU architecture. |

# Understanding System Average Load in Relation Number of CPUs

## Multi-processor Vs Multi-core

- **Multi-processor** – is where two or more physical CPU’s are integrated into a single computer system.

- **Multi-core processor** – is a single physical CPU which has at least two or more separate cores (or what we can also refer to as processing units) that work in parallel. Meaning a dual-core has 2 two processing units, a quad-core has 4 processing units and so on.

## Hyper threading

Furthermore, there is also a processor technology which was first introduced by Intel to improve parallel computing, referred to as hyper threading.

Under hyper threading, a single physical CPU core appears as two logical CPUs core to an operating system (but in reality, there is one physical hardware component).

# Authentication logs

Authentication logs in Linux record all auth-related events like logins, password changes, and sudo commands. Located at `/var/log/auth.log` (Debian) or `/var/log/secure` (RHEL/CentOS), these logs help detect brute force attacks and unauthorized access attempts. Use `tail /var/log/auth.log` to view recent entries. Regular log analysis is essential for server security monitoring.

# Running Services

Linux servers run various services including web, database, DNS, and mail servers. System administrators use tools like `systemctl`, `service`, `netstat`, `ss`, and `lsof` to manage and monitor services. Use `systemctl --type=service` to list all active services with their status. Essential for server management, resource monitoring, and troubleshooting.

- https://www.howtogeek.com/839285/how-to-list-linux-services-with-systemctl/

## Check if system uses systemd

`systemd` is a system and service manager

In the systemd world, services are launched by systemd which is now the first process to be launched. The details of the services are stored in unit files located in the "/usr/lib/systemd" directory.

```bash
pstree | head
```

We can see that systemd is the first process that is run after boot, so we're definitely on a systemd-based installation of Linux.

```bash
systemd-+-ModemManager---3*[{ModemManager}]
        |-NetworkManager---3*[{NetworkManager}]
        |-accounts-daemon---3*[{accounts-daemon}]
        |-alsactl
        |-atd
......
```

# Using systemctl to List Services

The command to list services and daemons is `systemctl`. We can refine the `systemctl` command with the `type` and `state` options. We're asking `systemctl` to report on services that are in the running state.

```bash
systemctl --type=service --state=running
```

- **Unit**: The name of the service or daemon. The column is titled "Unit" because whatever is in this column was launched using information systemd found in a unit file.
- **Load**: The load state of the service or daemon. It can be loaded, not-found, bad-setting, error, or masked.
- **Active**: The overall state the service or daemon is in. It can be active, reloading, inactive, failed, activating, or deactivating.
- **SUB**: The sub-state of the service or daemon. It can be dead, exited, failed, inactive, or running.
- **Description**: A short description of the unit.

Look for failed services:

```bash
systemctl --type=service --state=failed
```

Combinations of sub-states can be used. Type them as a comma-separated list. Make sure you don't include any whitespace between the options. Note that this finds services that match either state.

```bash
systemctl --type=service --state=failed,exited
```

## Using systemctl to List Unit Files

A unit file is a configuration file that tells systemd how to manage something on your Linux system.

We can expand the scope of the systemctl command by including the `list-unit-files` option. This doesn't just report on services and daemons that have been launched, it also lists all the unit files installed on your computer.

```bash
systemctl list-unit-files --state=enabled
```

The **PRESET** column tells you what the distribution recommends the unit's enablement state to be.

| Column | Meaning                                                                 |
| ------ | ----------------------------------------------------------------------- |
| STATE  | Whether you have currently enabled or disabled the unit                 |
| PRESET | Whether the distribution recommends enabling or disabling it by default |

## Looking at One Service in Detail

If something about one service or daemon piques your interest and deserves a deeper dive, you can look at it in detail using the systemctl status option.

Let's have a look at the SSH daemon, sshd. All we need to do is use the status option and the name of the service or daemon.

```bash
systemctl status sshd
```

The compact display shows:

- The name of the service together with a short description. A color-coded dot shows whether it is running or not. Green means it is running, red means it isn't.
- What was loaded, including the path to the unit file.
- How long it has been running.
- Where the documentation is located in the `man` manual.
- The Process ID of the running instance.
- How many concurrent instances of this service are running. Usually this will be one.
- How much memory is being consumed.
- How much CPU time has been consumed.
- The control group the service belongs to.

Relevant entries from the system log are also shown. These are typically events such as the startup of the service. These can be informative if you're looking into a service or daemon that didn't launch correctly.

## Start/Stop/Restart a service

```bash
sudo systemctl stop nginx
sudo systemctl start nginx
sudo systemctl restart nginx
```

Assume in case if a machine reboots then you want start the service along with the Operating system then we have to enable the service to start at the time of reboot.

```bash
sudo systemctl enable nginx
sudo systemctl enable --now nginx # starts the sevice right away
# to disable
sudo systemctl disable nginx
```

## journalctl

| Command                           | Description                           |
| --------------------------------- | ------------------------------------- |
| `journalctl`                      | Show all available journal entries    |
| `journalctl -b`                   | Show logs from the current boot       |
| `journalctl -b -1`                | Show logs from the previous boot      |
| `journalctl -u nginx`             | Show logs for a specific systemd unit |
| `journalctl -u keyd.service`      | Show logs for keyd                    |
| `journalctl -f`                   | Follow logs in real time              |
| `journalctl -p err`               | Show errors and more severe messages  |
| `journalctl --since today`        | Show logs since today                 |
| `journalctl --since "1 hour ago"` | Show logs from the last hour          |
| `journalctl -k`                   | Show kernel messages                  |

### View a complete log for a service

`-u` flag means unit.

It filters journal entries to show only messages belonging to a specific systemd unit.

```bash
sudo journalctl -u nginx
```

### Follow the log for a service in real-time

```bash
sudo journalctl -u nginx -f
# press ctrl+c to come out.
```

## Where are journal files stored?

The actual journal files are generally stored in:

| Directory           | Description                                       |
| ------------------- | ------------------------------------------------- |
| `/var/log/journal/` | Persistent journal logs                           |
| `/run/log/journal/` | Volatile journal logs (usually lost after reboot) |

### Traditional log files (/var/log/)

```bash
ls -lah /var/log/
```

| File / Directory    | Description                                      |
| ------------------- | ------------------------------------------------ |
| `/var/log/messages` | General system messages, if configured           |
| `/var/log/secure`   | Authentication and security-related messages     |
| `/var/log/audit/`   | Audit subsystem logs                             |
| `/var/log/dnf.log`  | DNF package manager logs                         |
| `/var/log/boot.log` | Boot-related messages, if available              |
| `/var/log/`         | General location for application and system logs |

## Application-specific logs

Some applications maintain their own logs.

Common locations:

| Directory         | Purpose                                            |
| ----------------- | -------------------------------------------------- |
| `~/.local/state/` | User-specific application state and sometimes logs |
| `~/.cache/`       | Temporary application data and caches              |
| `~/.config/`      | Application configuration (not normally logs)      |
| `/var/log/`       | System-wide application logs                       |

# Writing a Service File

Let’s say we want to create a service that runs a simple script called `my_script.sh` located in the `/usr/local/bin` directory. The script simply prints “Hello World” to the console.

```bash
#!/bin/bash
echo "My service by Tamas"
sleep 100000
```

Important: `sleep` doesn't mean your script has stopped. The script is still running, but its process is sleeping while waiting for the timer to expire.

1. First, we create a new file in the /etc/systemd/system directory called my_service.service

```
[Unit]
Description=My Service
After=network.target

[Service]
User=tamasm
ExecStart=/bin/bash /usr/local/bin/my_script.sh
Restart=always

[Install]
WantedBy=multi-user.target
```

2. Finally, we save and close the file, then reload the systemd daemon

```bash
sudo systemctl daemon-reload
sudo systemctl start my_service
sudo systemctl status my_service
sudo journalctl -u my_service -f
```

# Available Memory

In Linux, the “**available**” memory refers to the memory that is not in use by the operating system or applications. The available memory is also referred to as “**unused**” memory. A user can schedule or decide to initiate a new process on the system by looking at the available memory.

## free

The “free” command is utilized with the “m” option to present the available, used, shared and total amount of physical memory.

```bash
free -m
```

| Metric       | What it tells you                            |
| ------------ | -------------------------------------------- |
| `total`      | Total usable RAM                             |
| `used`       | RAM currently counted as used                |
| `free`       | Completely unused RAM                        |
| `buff/cache` | RAM used for buffers and caches              |
| `available`  | Estimated RAM available for new applications |

## top

This command provides an overview of the running processes and their resource usage, including memory usage, available memory, and many more in the “Mem” field

Power users can hit `f` inside top for advanced options like:

- Change metric used for ordering processes dynamically
- Toggle colors for quicker identification
- Customize refresh speed for a balancing smoothness vs accuracy

## vmstat

The “vmstat” command with the “s” option provides detailed states about system memory usage, including the “total”, “used”, “active”, “inactive”, and “available”memory

```bash
vmstat -s
```

## cat /proc/meminfo

The “/proc/meminfo” file displays detailed information about system memory usage, including the amount of available memory. It shows the information in the “MemAvailable” field

```bash
cat /proc/meminfo
```

## Free vs. Available memory explained

**Free memory** is the amount of memory (RAM) that is currently not used for anything. For this reason, especially on servers, I like to consider free memory as wasted memory. Once your applications/processes have launched, and considerable uptime has passed, this number should almost always be small.

**Available memory** is the amount of memory that is available for allocation to new or existing processes. Available memory is then an estimation of how much memory is available for use without swapping.

The difference between free vs. available memory in Linux is that free memory is not in use and sits there doing nothing. In contrast, available memory is used memory that includes but is not limited to caches and buffers that can be freed without the performance penalty of using swap.

## Why are available and free different?

The key concept is Linux uses RAM for caching, even when applications aren't using it.

Imagine your computer has 16 GB of RAM.

You have:
- 5 GB used by applications.
- 2 GB completely unused.
- 9 GB used for caching.

Now imagine you open another application that needs 4 GB of RAM.

Linux doesn't need to immediately use swap. It can reclaim some of that 9 GB used for caching.

# Process Management

Linux treats every running program as a process. Process management commands help view, control, and manipulate these processes. Key commands: `ps aux` shows running processes, `top` provides live system view, `kill -SIGTERM pid` gracefully stops processes, `kill -SIGKILL pid` forcefully terminates processes. Essential for understanding and controlling Linux system operations effectively.

## Runaway process

A runaway process is a process that behaves unexpectedly and consumes excessive system resources, often because of a bug or programming error.

For example:
- A process gets stuck in an infinite loop.
- A process continuously consumes CPU.
- A process keeps allocating memory without releasing it.
- A process creates child processes uncontrollably.

## Ghost/zombie process

The term ghost process is less formally defined in Linux. It is sometimes used to describe a process that appears to exist but is no longer functioning normally.

A common related concept is a **zombie process**.

A zombie process:
- Has finished executing.
- No longer runs or consumes CPU.
- Still has an entry in the process table.
- Waits for its parent process to collect its exit status.

## Common Process Management Challanges

1. Runaway processes eating resources – For example, loggers filling disks or code loops crowding out RAM
2. Ghost processes clinging on after finishing – Defunct remains of once active processes
3. Failure to set appropriate process priorities – Critical customer-facing apps denied resources
4. Difficulty tracing issues to specific processes – Is it the web app or database process at fault?

# Commands for Managing Processes

| Purpose           |                      Commands |
| ----------------- | ----------------------------: |
| Monitoring        |          `top`, `ps`, `pidof` |
| Controlling       | `bg`, `fg`, `kill`, `killall` |
| Priority          |              `nice`, `renice` |
| Resource Tracking |            `df`, `du`, `free` |

## ps

While interactive top output serves most real-time analytical needs, ps provides static snapshots. Think of running ps periodically as taking pictures of processes state over time.

The simplest invocation displays processes tied to the current terminal:

```bash
ps
```

### ps aux

- All processes – not just terminal tied ones
- Additional metadata like environments variables

| Option | Meaning                 | Description                                                              |
| ------ | ----------------------- | ------------------------------------------------------------------------ |
| `a`    | All users' terminals    | Show processes associated with terminals from all users, not just yours. |
| `u`    | User-oriented format    | Display processes in a detailed, user-friendly format.                   |
| `x`    | No controlling terminal | Include processes that aren't attached to a terminal.                    |

```bash
ps aux
```

### Custom columns

```bash
# Show all processes and their priorities
ps -eo pid,comm,ni,pri

# Show a specific process
ps -p 1234 -o pid,comm,ni,pri

# Show processes sorted by nice value
ps -eo pid,comm,ni,pri --sort=ni
```

| Option | Meaning       | Description                                       |
| ------ | ------------- | ------------------------------------------------- |
| -e     | Every process | Select all processes running on the system.       |
| -o     | Output format | Specify which columns to display and their order. |

| Column    | Meaning                    |
| --------- | -------------------------- |
| `PID`     | Process ID                 |
| `COMMAND` | Process name               |
| `NI`      | Nice value                 |
| `PRI`     | Kernel scheduling priority |

The key advantage of ps over its real-time sibling top is collecting **historical process state data**:

```bash
ps aux --sort=%cpu > cpu_processes.log
```

The above grabs a CPU % ordered snapshot into a logged file. Comparing multiple such log files gathered over days or weeks gives trend insights.

For example, ps logs can clearly show that cron jobs start spiking site reliability after a certain package update.

In summary, use ps for both tactical snapshots and strategic historical process state analysis.

## kill

Despite best efforts to optimize Linux for stability, processes sometimes get unresponsive or resource hungry. kill gives administrators manual termination capabilities for such scenarios.

For example, to remove a ‘zombie‘ process clinging onto resources beyond its functioning:

```bash
kill 1234
```

This sends a polite termination request to process ID 1234. Most well written Linux programs handle such signals smoothly – saving state before shutting down handlers and threads gracefully.

However, some processes ignore this termination plea or take too long to wrap up. In such cases, `kill -9` can be used to force immediate stoppage:

### List signals

```bash
kill -L
# or -l if -L is not available
```

### killall

`killall` is an alternative way that instead of needing the PID, accepts process names:

```bash
killall vlc
```

This attempts terminating all instances of the VLC media application running. Add `-9` if needed for forced shutdown of misbehaving VLC processes.

## nice and renice

Influence Process Priority

By default, regular Linux processes have a priority value of 0 but administrators can influence relative weighting via `nice` and `renice`

### nice

Set starting priority when launching processes

```bash
nice -n 15 backup.sh
```

This starts the backup script with favorable priority. Think of nice as allowing higher or lower initial positioning in the process queue.

| Nice value | Meaning                     |
| ---------: | --------------------------- |
|      `-20` | Highest scheduling priority |
|        `0` | Default priority            |
|       `19` | Lowest scheduling priority  |

### renice

Alters priority of already running processes

```bash
renice +5 2304
```

Here we are reducing preference of an existing process ID 2304 to promote sharing of resources.

### NI vs PRI

| NI (Nice value)                           | PRI (Priority)                                             |
| ----------------------------------------- | ---------------------------------------------------------- |
| User-space scheduling preference          | Kernel scheduling priority                                 |
| Can be changed using `nice` or `renice`   | Calculated and managed by the kernel                       |
| Range: `-20` to `19` for normal processes | Depends on scheduling policy and kernel                    |
| Lower value = higher priority             | Lower value = higher priority (for normal Linux processes) |

### Processes

https://linuxconfig.org/understanding-foreground-and-background-linux-processes

# User Management

Linux user management allows multiple users to interact with the system in isolation. Includes creating, deleting, modifying users and groups, assigning permissions and ownership. Key commands: `adduser/useradd` creates users, `deluser/userdel` removes users, `passwd` manages passwords, `su` switches users. Essential for providing proper accessibility and maintaining Linux system security.

## User Management Commands (Linux)

| Command / Flag  | Meaning                                    | Example                                  | Result                                                |
| --------------- | ------------------------------------------ | ---------------------------------------- | ----------------------------------------------------- |
| `useradd`       | Create a new user                          | `sudo useradd tamas`                     | Creates user `tamas`                                  |
| `-m`            | Create the user's home directory           | `sudo useradd -m tamas`                  | Creates `/home/tamas`                                 |
| `-d DIR`        | Specify the home directory                 | `sudo useradd -m -d /home/custom tamas`  | Sets custom home directory                            |
| `-s SHELL`      | Specify the user's login shell             | `sudo useradd -s /bin/bash tamas`        | Sets Bash as login shell                              |
| `-G GROUPS`     | Add user to supplementary groups           | `sudo useradd -G wheel,docker tamas`     | Adds user to `wheel` and `docker`                     |
| `-u UID`        | Specify the user ID                        | `sudo useradd -u 1001 tamas`             | Assigns UID 1001                                      |
| `-c COMMENT`    | Add user information/comment               | `sudo useradd -c "Tamas" tamas`          | Sets user description                                 |
| `passwd`        | Set or change a user's password            | `sudo passwd tamas`                      | Changes password                                      |
| `passwd -l`     | Lock a user's password                     | `sudo passwd -l tamas`                   | Locks password authentication                         |
| `passwd -u`     | Unlock a user's password                   | `sudo passwd -u tamas`                   | Unlocks password authentication                       |
| `passwd -d`     | Delete a user's password                   | `sudo passwd -d tamas`                   | Removes password                                      |
| `usermod`       | Modify an existing user                    | `sudo usermod -s /bin/bash tamas`        | Changes user's shell                                  |
| `-aG`           | Append user to supplementary groups        | `sudo usermod -aG wheel tamas`           | Adds user to `wheel` without removing existing groups |
| `-G`            | Replace supplementary group membership     | `sudo usermod -G wheel tamas`            | Replaces existing supplementary groups                |
| `-l`            | Rename a user                              | `sudo usermod -l newname tamas`          | Renames user                                          |
| `-d`            | Change user's home directory               | `sudo usermod -d /home/newname tamas`    | Changes home directory path                           |
| `-m`            | Move contents to the new home directory    | `sudo usermod -d /home/newname -m tamas` | Moves home directory contents                         |
| `userdel`       | Delete a user                              | `sudo userdel tamas`                     | Deletes user account                                  |
| `-r`            | Delete user and home directory             | `sudo userdel -r tamas`                  | Deletes account and home directory                    |
| `id`            | Display user and group IDs                 | `id tamas`                               | Shows UID, GID and groups                             |
| `whoami`        | Display current username                   | `whoami`                                 | Prints current user                                   |
| `who`           | Display currently logged-in users          | `who`                                    | Lists logged-in sessions                              |
| `w`             | Display logged-in users and their activity | `w`                                      | Shows users and running processes                     |
| `groups`        | Display a user's groups                    | `groups tamas`                           | Lists group memberships                               |
| `getent passwd` | Display user account entries               | `getent passwd`                          | Lists users from configured account databases         |
| `chage`         | Manage password expiration and aging       | `sudo chage -l tamas`                    | Displays password aging information                   |
| `chage -M N`    | Set maximum password age in days           | `sudo chage -M 90 tamas`                 | Password expires after 90 days                        |
| `chage -E DATE` | Set account expiration date                | `sudo chage -E 2027-01-01 tamas`         | Account expires on specified date                     |

## Group Management Commands

| Command / Flag | Meaning                    | Example                                   | Result                      |
| -------------- | -------------------------- | ----------------------------------------- | --------------------------- |
| `groupadd`     | Create a group             | `sudo groupadd developers`                | Creates `developers` group  |
| `-g GID`       | Specify group ID           | `sudo groupadd -g 1500 developers`        | Creates group with GID 1500 |
| `groupdel`     | Delete a group             | `sudo groupdel developers`                | Deletes group               |
| `groupmod`     | Modify a group             | `sudo groupmod -n programmers developers` | Renames group               |
| `-n`           | Specify a new group name   | `sudo groupmod -n programmers developers` | Renames group               |
| `-g`           | Change group ID            | `sudo groupmod -g 1600 developers`        | Changes GID to 1600         |
| `gpasswd -a`   | Add a user to a group      | `sudo gpasswd -a tamas developers`        | Adds user to group          |
| `gpasswd -d`   | Remove a user from a group | `sudo gpasswd -d tamas developers`        | Removes user from group     |

## Important Configuration Files

| File              | Description                                                  | Example                 |
| ----------------- | ------------------------------------------------------------ | ----------------------- |
| `/etc/passwd`     | Stores user account information (not passwords)              | `cat /etc/passwd`       |
| `/etc/shadow`     | Stores password hashes and password aging information        | `sudo cat /etc/shadow`  |
| `/etc/group`      | Stores group information and memberships                     | `cat /etc/group`        |
| `/etc/gshadow`    | Stores secure group information                              | `sudo cat /etc/gshadow` |
| `/etc/skel/`      | Contains default files copied into new user home directories | `ls -la /etc/skel/`     |
| `/etc/login.defs` | Defines default settings for user account management         | `cat /etc/login.defs`   |

## Useful Examples

```bash
# Create a user with a home directory
sudo useradd -m tamas

# Set the user's password
sudo passwd tamas

# Add user to wheel (sudo privileges on Fedora)
sudo usermod -aG wheel tamas

# Check user information
id tamas

# List all groups
getent group

# List all users
getent passwd

# Switch to another user
su - tamas

# Execute a command as another user
sudo -u tamas whoami

# Delete a user and their home directory
sudo userdel -r tamas
```

# Disks and Filesystems

Linux uses various filesystems to organize, store, and retrieve data from storage devices. Popular filesystems include EXT4 (robust for Linux volumes), FAT32 (compatible with all OS for removable media), NTFS, and Btrfs. Each has specific advantages and use cases. Use `df -T` command to display filesystem types, disk space usage, and mounted device information.

## Filesystems, Layouts, and Mounts

Planning a Linux installation starts with deciding how storage should be divided. A partition marks a region on a disk, while its filesystem organizes data for access through a mount point in Linux’s directory tree.

- run `lsblk -f` to see device names, filesystem types, UUIDs and mount points.

### Linux partition

A Linux disk can use a partition table to divide a device into regions, and a filesystem organizes data within a region.

Linux exposes a filesystem’s directories through a mount point in its directory tree.

Think of a disk as a book. Its partition table is the index and its partitions are sections. Linux can also use filesystems on logical volumes, software RAID devices, image files or whole devices without conventional partitions.

LVM (Logical Volume Manager) builds logical volumes from physical storage through volume groups, adding a layer to inspect between disks and filesystems.

The partition table records region boundaries and types so firmware and operating systems can identify partitions during startup and normal operation.

Partition paths combine a device name and a partition number, with family-specific separators such as /dev/nvme0n1p1. These device-node paths can change with hardware or enumeration, while filesystem labels identify filesystems.

MBR (Master Boot Record) and GPT (GUID Partition Table) are disk partitioning schemes that define how partitions are structured and addressed on a storage drive. GPT is the modern standard for UEFI-based systems, drives over 2 TB, and Windows 11, while MBR is a legacy scheme tied to older BIOS systems and smaller drives.

BIOS (Basic Input/Output System) and UEFI (Unified Extensible Firmware Interface) are low-level motherboard firmware interfaces that initialize hardware and start your operating system when you power on a computer. UEFI is the modern replacement for legacy BIOS, offering faster startup times, larger drive support, graphical menus, and stronger security.

## Disk, partition table, partition and mount point

For example, `/dev/sda` and `/dev/nvme0n1` identify disks, while `/dev/sda1` and `/dev/nvme0n1p1` identify partitions.

GPT is common with modern UEFI layouts. The table above shows how a partition device differs from its filesystem and mount point.

| Layer           | What it describes                            | Example to recognize      |
| --------------- | -------------------------------------------- | ------------------------- |
| Device          | The storage unit presented by the kernel     | `/dev/sda`                |
| Partition table | Boundaries and types on a disk               | GPT or MBR                |
| Partition       | A bounded device region                      | `/dev/sda1`               |
| Filesystem      | Organization of files and metadata           | ext4, Btrfs, XFS, NTFS    |
| Mount point     | Directory through which mounted files appear | `/`, `/home`, `/mnt/data` |

The root filesystem holds the running Linux system, so changes to its storage can affect the environment performing the change. A live installer or recovery environment lets you inspect and manage a system partition while it is not serving as the active root.

## How Linux installation layouts fit together

UEFI uses an EFI System Partition (ESP) for firmware-readable boot files, which a dual-boot installer may reuse. A basic Linux installation assigns / to its root filesystem.

A separate /home partition can keep user files apart from the root filesystem, though it can complicate resizing.

The EFI system partition (ESP) is a dedicated storage partition used by Unified Extensible Firmware Interface (UEFI) computers to boot operating systems and run pre-boot utilities. It acts as the bridge between hardware firmware and the OS, replacing the legacy Master Boot Record (MBR) boot model.

Swap can use a file or a dedicated partition. A file can be adjusted without changing the partition table.

For hibernation, follow the distribution’s documented swap-capacity requirements.

**Hibernation vs sleep**
- Sleep (suspend): Your computer keeps its RAM powered, but most other components turn off. Your open programs remain in RAM.
- Hibernation: Linux saves the contents of RAM to disk and then completely powers off the computer.

| Filesystem | Useful detail                                 |
| ---------- | --------------------------------------------- |
| Ext4       | Common general-purpose choice with journaling |
| Btrfs      | Snapshots and checksums                       |
| XFS        | Journaling and online expansion               |

Filesystem repair tools expect a particular format and may require the filesystem to be unmounted. Identify the filesystem before selecting a checker, and do not run a repair operation against a mounted root system.

## Ext4

**General-purpose filesystem**

Ext4 (Fourth Extended Filesystem) is one of the most widely used Linux filesystems.

### Journaling

Before making certain changes to the filesystem, Ext4 records information about the intended changes in a special area called a *journal*.

Why?

Imagine your computer loses power while writing a file. The filesystem might be left in an inconsistent state.

With journaling, the filesystem can use its journal during recovery to help restore consistency.

Important: Journaling doesn't guarantee that every file's contents are protected from corruption or data loss.

**In simple terms:** Journaling helps the filesystem recover after unexpected crashes or power failures.

---

## Btrfs

**Advanced filesystem**

Btrfs (B-tree Filesystem) is a modern Linux filesystem with several advanced features.

### Snapshots

A snapshot captures the state of a filesystem or subvolume at a particular point in time.

Imagine:

1. Your Fedora system works perfectly.
2. You create a snapshot.
3. You install an update.
4. The update breaks something.
5. You can potentially roll back to the earlier snapshot.

Think of it like a restore point.

### Checksums

A checksum is a value calculated from data.

Btrfs calculates and stores checksums for data and metadata. When reading data, it calculates the checksum again and compares it with the stored one.

If they don't match, Btrfs can detect that the data has changed unexpectedly.

With redundant copies available, Btrfs can also repair certain corruption automatically.

**In simple terms:**
- Snapshots = capture and potentially restore filesystem states.
- Checksums = detect data corruption.

---

## XFS

**High-performance filesystem**

XFS is a mature Linux filesystem designed to handle large filesystems and heavy workloads.

### Journaling

Similar to Ext4, XFS uses journaling to help maintain filesystem consistency after crashes.

### Online expansion

This means you can increase the size of an XFS filesystem while it is mounted and actively being used.

For example:

1. You have a 100 GB partition.
2. You increase the underlying partition to 200 GB.
3. You expand the XFS filesystem to use the additional space.
4. You don't need to unmount the filesystem to expand it.

Important: XFS supports growing, but not shrinking.

---

## Quick Comparison

| Feature          | Ext4                     | Btrfs                 | XFS                      |
| ---------------- | ------------------------ | --------------------- | ------------------------ |
| Journaling       | Yes                      | Copy-on-Write instead | Yes                      |
| Snapshots        | No native support        | Yes                   | No native support        |
| Checksums        | Metadata (not file data) | Data and metadata     | Metadata (not file data) |
| Online expansion | Yes                      | Yes                   | Yes                      |
| Shrinking        | Yes, offline             | Yes, with limitations | No                       |

## What Should You Remember?

- **Ext4:** Reliable, straightforward, general-purpose filesystem.
- **Btrfs:** Snapshots, checksums and advanced storage management.
- **XFS:** Good for large filesystems and workloads, with online expansion.

### Important Concept: Copy-on-Write (CoW)

Ext4 and XFS use journaling, while Btrfs uses a different approach called Copy-on-Write (CoW).

CoW means that instead of overwriting existing data directly, the filesystem writes modified data to a new location and then updates its references.

This helps Btrfs create efficient snapshots and maintain data consistency.

# Mount

A **mount point** is a directory in Linux where a filesystem becomes accessible.

A **mount path** is simply the path to that directory.

```bash
lsblk -f
df -h
```

# Blocks

A **block** is a fixed-size unit of storage that a filesystem uses to organize and store data on a disk.

Think of a disk as a warehouse divided into small storage spaces. Each space represents a block.

### How do blocks work?

For example, if a filesystem uses 4 KB blocks and you save a 10 KB file:

| Block     | Storage used | Unused space |
| --------- | -----------: | -----------: |
| Block 1   |         4 KB |         0 KB |
| Block 2   |         4 KB |         0 KB |
| Block 3   |         2 KB |         2 KB |
| **Total** |    **10 KB** |     **2 KB** |

The file occupies 3 blocks (12 KB of allocated space).

The unused 2 KB in the last block is called **internal fragmentation**.

### Why do blocks exist?

Instead of managing every individual byte separately, filesystems manage storage in fixed-size chunks called blocks.

This makes allocating, tracking and organizing disk space easier.

### Remember

- A block is a small unit of storage.
- Files are stored across one or more blocks.
- Block size determines the minimum allocation unit.
- Common filesystem block size: 4 KB.
- The filesystem manages blocks automatically.

# Filesystem Hierarchy Standard

https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/4/html/reference_guide/s1-filesystem-fhs

# Blocks and other useful knowledge

https://www.linuxjournal.com/content/understanding-linux-filesystems-inodes-block-sizes-and-data-structures

# Computers fast as f

```bash
cat /home/tamas/notes.txt
```

Behind that one command:

1. Linux resolves the directory path.
2. The filesystem finds the file's metadata.
3. The filesystem identifies where its data lives.
4. Linux requests the corresponding storage data.
5. The SSD controller translates logical addresses into physical NAND locations.
6. The data travels back into RAM.
7. Linux gives the contents to cat.
8. Your terminal displays the text.
