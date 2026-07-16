# Linux Fundamentals Part 2: Permissions, Compression & Package Management

Continuing my Linux notes series — in [Part 1](#) I covered the filesystem and basic file operations. This time: who gets to do what to a file, how to zip things up, and how to actually install software.

## File Permissions: The Octal Notation

Linux permissions boil down to three numbers, one each for **user (owner)**, **group**, and **others**:

- **4** = Read
- **2** = Write
- **1** = Execute

You add them together to get the permission level for each group:

| Value | Meaning |
|---|---|
| 7 (4+2+1) | Read, Write, Execute |
| 6 (4+2) | Read, Write |
| 5 (4+1) | Read, Execute |
| 4 | Read only |
| 3 (2+1) | Write, Execute |
| 2 | Write only |
| 1 | Execute only |
| 0 | No access |

So `chmod 777 file` gives everyone full access, while `chmod 400 file` restricts a file to owner-read-only.

```bash
chmod 400 filename
ls -l          # or "ll" - view permissions in rwx format
stat filename  # detailed metadata view
getfacl file   # view the access control list
```

## Changing Ownership

```bash
chown avinash a.txt          # change file owner
chgrp developers a.txt       # change group
chown -R avinash /data       # change ownership recursively across a directory
```

## Sticky Bit

A sticky bit means anyone can create files in a directory, but only the owner of a file can delete it — commonly used for shared temp directories:

```bash
chmod 1777 team_temp
```

## Compression: zip, unzip, tar

```bash
dnf install zip -y
dnf install unzip -y

zip files.zip file1.txt file2.txt file3.txt   # zip specific files
zip allfiles.zip *                            # zip everything in current dir
zip -r myfolder.zip myfolder/                 # zip a directory recursively

unzip myzip.zip           # extract
unzip -l test.zip         # list contents without extracting
```

For `.tar.gz` files:

```bash
tar -czvf files.tar.gz file1 file2 file3   # create + compress
tar -xzvf songs.tar.gz                     # extract
tar -ztvf songs.tar.gz                     # list contents without extracting
```

Flag breakdown: **c**reate, co**m**press (**z**), **v**erbose, **f**ile name.

## Redirecting Output

```bash
echo "hi"

echo "hello from avinash" >> aa.txt   # append a new line
echo "hello from avinash" > aa.txt    # overwrite the file entirely
```

## Package Management: RPM vs YUM vs DNF

**RPM** (Red Hat Package Manager) — the original. Installs `.rpm` packages directly but won't resolve dependencies for you.

```bash
rpm -ivh package.rpm
```

**YUM** (Yellowdog Updater Modified) — adds automatic dependency resolution, pulling from local/remote repos.

```bash
yum install httpd
yum search httpd
yum remove httpd
```

**DNF** (Dandified YUM) — the modern successor, used by newer distros. Faster dependency resolution, plugin-based, more secure.

```bash
dnf install httpd
dnf search tree           # search by name/description
dnf list installed        # view everything installed
dnf update                # update all packages
```

Repo configs live in `/etc/yum.repos.d/`.

## Managing Services with systemctl

Once you've installed something like Apache, you manage it through `systemctl`:

```bash
systemctl status httpd     # check status
systemctl start httpd      # start
systemctl restart httpd    # restart
systemctl enable httpd     # auto-start on boot
```

(Older equivalents: `service httpd start` and `chkconfig httpd on`.)

**Config file locations worth remembering:**
- Apache config: `/etc/httpd/conf/httpd.conf`
- Apache document root: `/var/www/html/`
- nginx config: `/etc/nginx/nginx.conf`
- nginx document root: `/usr/share/nginx/html/`

---

That's permissions, compression, and package management covered. In **Part 3**, I'll wrap up with process management, user administration, cron jobs, basic networking, and AWS EBS storage.

*If you're working through the same fundamentals, would love to hear what tripped you up — always learning from the comments.*

#Linux #DevOps #SysAdmin #CloudComputing #AWS
