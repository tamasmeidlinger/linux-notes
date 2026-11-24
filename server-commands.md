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
