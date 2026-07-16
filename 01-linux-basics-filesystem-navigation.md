# Linux Fundamentals Part 1: Filesystem Structure, Navigation & File Operations

If you're starting out with Linux (or brushing up before an interview), the first thing to get comfortable with is the filesystem — everything in Linux lives under a single root, and once that clicks, the rest of the commands start to make a lot more sense.

Here's a practical rundown of what I've been working through.

## Everything Starts at `/`

In Linux, there's one top-level directory: `/` (root). Every other directory — `/home`, `/etc`, `/var` — branches off from it. It's the equivalent of `C:\` in Windows, except there's no concept of separate drive letters; everything is one unified tree.

Key directories worth knowing:

- **`/root`** – the root user's private home directory (like `C:\Users\Administrator`)
- **`/home`** – home directories for all non-root users
- **`/bin`** – essential binaries used system-wide (`ls`, `cp`, `mv`, etc.)
- **`/boot`** – kernel and boot loader files
- **`/dev`** – device files representing hardware (similar to Windows Device Manager)
- **`/etc`** – system configuration files (passwords, DNS, OS info)
- **`/var`** – frequently changing data: logs, databases, web content
- **`/usr`** – user-level programs and libraries
- **`/sbin`** – admin-only commands (`ifconfig`, `iptables`, firewall tools)
- **`/mnt`** – temporary mount points
- **`/media`** – removable media (USB, CD)
- **`/proc`** – live process information
- **`/lib`, `/lib64`** – shared libraries (like Windows `.dll` files)
- **`/opt`** – optional third-party software

## Basic Navigation

```bash
pwd          # print working directory
cd           # change directory
cd ..        # go one level back
ls           # list contents of current directory
ls -a        # list all, including hidden files
whoami       # shows current logged-in user
hostname     # displays the system/instance name
```

Pro tip: when navigating with `cd`, press **Tab twice** to see available directories at that path.

## Creating Files and Directories

```bash
touch filename.txt        # create an empty file
touch song{1..10}.txt     # create song1.txt through song10.txt in one shot
mkdir new_directory       # create a directory
```

## Copying, Moving & Renaming

```bash
cp source target                     # copy
cp song1.mp3 song2.mp3 songs/        # copy multiple files into a folder
cp s* test/                          # copy all files starting with "s"

mv video1.mp4 video2.mp4 videos/     # move (cut & paste)
mv oldname.txt newname.txt           # mv also handles renaming
```

## Deleting

```bash
rmdir directory     # deletes only EMPTY directories
rm -rf directory    # force-deletes a directory and its contents (use with care!)
```

## Viewing File Contents

```bash
cat filename          # dump entire file to screen (good for short files)
more filename         # page through a long file
head passwd           # first 10 lines by default
head -n 3 passwd      # first 3 lines
tail passwd           # last 10 lines
tail -n 3 passwd      # last 3 lines
sort passwd           # sort lines alphabetically
sort -r passwd        # sort in reverse
```

## Searching Text with grep

`grep` = **g**lobal **r**egular **e**xpression **p**rint — your go-to for finding text inside files.

```bash
grep "word" filename.txt              # case-sensitive search
grep -i "word" filename.txt           # case-insensitive search
grep "word" file1.txt file2.txt       # search across multiple files
grep -r "word" /path/to/directory     # recursive search through a directory
```

## Editing Files with Vim

`vi` is the original editor; `vim` (Vi IMproved) builds on it and is what you'll use day-to-day.

```bash
vim new.txt      # creates and opens a new file
vim first.txt    # opens an existing file
```

Vim has two core modes:
- **Command mode** – for navigation and deletion
- **Insert mode** – for actually typing content

Useful command-mode shortcuts:

| Key | Action |
|---|---|
| `k` / `j` | move cursor up / down |
| `o` | open a new line below and enter insert mode |
| `x` | delete character before cursor |
| `db` | delete the word before the cursor |
| `dd` | delete the current line |
| `5dd` | delete the next 5 lines |
| `:set number` | show line numbers |
| `:set nonumber` | hide line numbers |
| `7G` | jump to line 7 |
| `u` | undo |
| `Ctrl + r` | redo |
| `:wq` | save and quit |
| `:q!` | quit without saving |

---

That covers the foundation — the filesystem layout, moving around, and basic file handling. In **Part 2**, I'll go through file permissions, compression (zip/tar), and package management with rpm/yum/dnf.

*Following along and building your own Linux notes? Drop a comment — always happy to compare notes with others learning this stuff.*

#Linux #DevOps #SysAdmin #CloudComputing #LearningInPublic
