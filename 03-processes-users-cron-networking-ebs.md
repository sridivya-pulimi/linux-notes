# Linux Fundamentals Part 3: Processes, Users, Cron, Networking & AWS EBS Storage

Wrapping up my three-part Linux notes series. [Part 1](#) covered the filesystem and file operations, [Part 2](#) covered permissions and package management. This final part covers process management, user administration, scheduled jobs, basic networking, and — since most of us run Linux on the cloud these days — AWS EBS storage fundamentals.

## Process Management

```bash
top              # real-time view of processes, CPU, and memory usage
ps               # snapshot of current processes
ps aux           # a = all users, u = owner info, x = processes with no terminal
ps aux | grep sleep

pidof nginx      # get the process ID of a running service
pgrep httpd      # grep specifically for process names
pkill nginx      # kill all matching processes by name

kill <pid>       # terminate a specific process
kill -9 <pid>    # SIGKILL - force kill

pstree           # view the process tree
pstree | less

dnf install htop -y
htop             # interactive, color version of top
```

## Memory Usage

```bash
free
free -h    # human-readable
free -g    # in gigabytes
```

## User & Group Management

```bash
useradd <username>          # create a user
id <username>                # show user's UID/GID info
su <username>                 # switch user (as root)
su - <username>                # switch user with full login environment

usermod -aG wheel <username>  # add user to the wheel group (sudo access)
groups <username>              # list groups a user belongs to

groupadd <groupname>           # create a new group
gpasswd -a <username> <group>  # add user to a group
deluser <username>              # remove a user

cat /etc/group                # list all groups and their members
```

For direct password-based SSH login, you'll need to edit `/etc/ssh/sshd_config` and set `PasswordAuthentication yes`, then restart the `sshd` service.

## Cron: Scheduling Tasks

Cron runs jobs on a time-based schedule. Format:

```
minute hour day month day_of_week   command
```

```bash
sudo dnf install cronie cronie-anacron -y

# Run every minute
* * * * * sh /home/ec2-user/hello.sh

# Run daily
sudo -u avinash crontab -e
@daily echo "$(date) - Uptime: $(uptime)" >> home/avinash/uptime.log

# Cleanup: delete files older than 7 days
find /tmp -type f -atime +7 -delete
```

- User-specific crontabs live in `/var/spool/cron`
- System-wide schedule: `/etc/crontab`
- [crontab.guru](https://crontab.guru) is a handy tool for building cron expressions

## Basic Networking

```bash
scp myfile.txt ec2-user@192.168.100.1:/home/ec2-user/           # secure copy
scp -r project-data/ ec2-user@192.168.100.1:/home/ec2-user/     # copy a directory
scp -i keypair.pem myfile.txt ec2-user@192.168.100.1:/home/ec2-user/  # with a key pair

ip addr        # view network interfaces (modern)
ifconfig       # view network interfaces (legacy)

ss -tulnp              # list listening ports and associated processes
sudo lsof -i :80        # what's using port 80?

sudo dnf install bind-utils
nslookup google.com
dig google.com

telnet <ip> <port>                              # test connectivity to a port
openssl s_client -connect <ip>:<port>           # modern telnet alternative, esp. for TLS
```

## AWS EBS Storage Fundamentals

A quick primer since most Linux boxes today are EC2 instances.

**IOPS** = Input/Output Operations Per Second. Rough formula:

```
IOPS = (Throughput × 1024) / Block Size
```

**Volume types:**

| Type | Category | Notes |
|---|---|---|
| gp2 | General Purpose SSD | 3 IOPS/GB, min 100 IOPS, min size 1 GB, max 16 TB |
| gp3 | General Purpose SSD | IOPS configurable independently of size |
| io1 / io2 | Provisioned IOPS SSD | Best performance, for workloads needing >16,000 IOPS, min 4 GB |
| st1 | Throughput Optimized HDD | Big data, log processing, min 125 GB |
| sc1 | Cold HDD | Same use case as st1, lower cost/performance |
| standard | Magnetic | Legacy, low-cost, max 1 TB |

**Working with volumes:**

```bash
lsblk           # list all block storage devices attached to the instance
df -Th          # show mounted storage devices, including temp volumes

mkdir newvol/
mkfs -t xfs /dev/nvme1n1        # format the volume with a filesystem
mount /dev/nvme1n1 newvol/       # mount it
```

After mounting, check `/etc/mtab` for the mount entry and copy it into `/etc/fstab` to make the mount persistent across reboots.

**Resizing a volume:**

```bash
# 1. Increase size in the AWS console first
xfs_growfs -d /home/ec2-user/newvol    # then grow the filesystem
growpart /dev/nvme0n1 1                # or grow a specific partition
```

**Resizing `/tmp` on the fly:**

```bash
mount -o remount,size=4G /tmp
```
And to persist it, add to `/etc/fstab`:
```
tmpfs /tmp tmpfs defaults,size=4G 0 0
```

---

That wraps up the three-part series — from filesystem basics all the way to EBS storage on AWS. Hopefully useful whether you're prepping for a sysadmin/DevOps role or just building muscle memory with the command line.

*Thanks for following along — let me know in the comments what you'd want covered next (shell scripting? networking deep dive? Docker?).*

#Linux #DevOps #AWS #CloudComputing #SysAdmin #LearningInPublic
