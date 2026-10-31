# Linux & DevOps Command Cheat Sheet

**Ubuntu / AWS EC2 \| Beginner-friendly \| GitHub-ready**

This reference combines commands practiced in your terminal sessions and
screenshots, plus closely related commands useful for Linux and DevOps
work.

> Most commands below are for a Linux shell (Ubuntu). `ssh` and `scp`
> can also be used from Windows PowerShell when OpenSSH is installed.
> Commands such as `awk`, `sed`, `grep`, `tar`, and `umask` should be
> run in Linux/WSL/Git Bash, not ordinary PowerShell.

## Contents

1.  Terminal and navigation
2.  Files and directories
3.  Reading and editing files
4.  Redirection and pipes
5.  Users and groups
6.  Ownership and permissions
7.  System information and administration
8.  Processes and monitoring
9.  Package management
10. Archives and compression
11. SSH, SCP and rsync
12. Networking and DNS
13. Routes, scanning and firewall
14. Text processing and log analysis
15. Shell operators
16. Practical workflows
17. Safety notes
18. Next commands to learn

------------------------------------------------------------------------

## 1. Terminal and navigation

  -----------------------------------------------------------------------
  Command                 Purpose                 Example
  ----------------------- ----------------------- -----------------------
  `pwd`                   Print current directory `pwd`

  `ls`                    List files and          `ls`
                          directories             

  `ls -l`                 Long listing with       `ls -l`
                          permissions, owner,     
                          group, size and date    

  `ls -a`                 Include hidden files    `ls -a`

  `ls -ltr`               Long listing sorted by  `ls -ltr`
                          modification time,      
                          oldest first            

  `cd DIR`                Change directory        `cd cloud`

  `cd ..`                 Move to parent          `cd ..`
                          directory               

  `cd ~`                  Move to home directory  `cd ~`

  `clear`                 Clear terminal display  `clear`

  `which CMD`             Show executable path    `which awk`
                          found in `PATH`         

  `man CMD`               Open command manual     `man chmod`

  `CMD --help`            Show command help (if   `ls --help`
                          supported)              
  -----------------------------------------------------------------------

Path shortcuts: `.` current directory; `..` parent; `~` home; `/`
filesystem root. Absolute paths start at `/`, such as `/home/ubuntu`;
relative paths start from the current directory.

## 2. Files and directories

  ----------------------------------------------------------------------------------
  Command                  Purpose                 Example
  ------------------------ ----------------------- ---------------------------------
  `mkdir DIR`              Create directory        `mkdir cloud`

  `mkdir -p a/b/c`         Create parent           `mkdir -p project/logs/archive`
                           directories as needed   

  `touch FILE`             Create empty file or    `touch abc.txt`
                           update timestamp        

  `cp SRC DEST`            Copy file               `cp abc.txt backup.txt`

  `cp -r SRC DEST`         Copy directory          `cp -r cloud backup-cloud`
                           recursively             

  `mv SRC DEST`            Move or rename          `mv old.txt new.txt`

  `rm FILE`                Remove file             `rm old.txt`

  `rm -r DIR`              Remove directory and    `rm -r old-folder`
                           contents recursively    

  `rmdir DIR`              Remove an empty         `rmdir empty-folder`
                           directory               

  `ln TARGET LINK`         Create hard link        `ln file.txt hardlink.txt`

  `ln -s TARGET LINK`      Create symbolic link    `ln -s file.txt softlink.txt`

  `find PATH -name NAME`   Find files/directories  `find . -name "*.log"`
                           by name                 
  ----------------------------------------------------------------------------------

**Hard link vs symbolic link:** A hard link is another directory entry
for the same file data (normally on the same filesystem). A symbolic
link stores a path to another file/directory and can become dangling if
its target is removed.

## 3. Reading and editing files

  ---------------------------------------------------------------------------
  Command                 Purpose                 Example
  ----------------------- ----------------------- ---------------------------
  `cat FILE`              Print file contents     `cat apt.log`

  `head FILE`             Show first 10 lines by  `head apt.log`
                          default                 

  `head -n 5 FILE`        Show first 5 lines      `head -n 5 apt.log`

  `tail FILE`             Show last 10 lines by   `tail apt.log`
                          default                 

  `tail -n 5 FILE`        Show last 5 lines       `tail -n 5 apt.log`

  `tail -f FILE`          Follow a growing log    `tail -f /var/log/syslog`
                          file                    

  `vi FILE`               Open file in vi         `vi notes.txt`

  `vim FILE`              Open file in Vim        `vim notes.txt`

  `less FILE`             View file page by page  `less apt.log`
  ---------------------------------------------------------------------------

Vim quick keys: `i` insert mode; `Esc` normal mode; `:w` save; `:q`
quit; `:wq` save and quit; `:q!` quit without saving.

## 4. Redirection and pipes

These are shell operators, not standalone commands.

  ------------------------------------------------------------------------------------
  Operator                Purpose                 Example
  ----------------------- ----------------------- ------------------------------------
  `>`                     Redirect output;        `echo "hello" > note.txt`
                          overwrite file          

  `>>`                    Append output           `echo "next line" >> note.txt`

  `\|`                    Pipe output into        `ps aux \| grep ubuntu`
                          another command         

  `&&`                    Run next command only   `mkdir demo && cd demo`
                          if previous succeeds    

  `\|\|`                  Run next command only   `cd missing \|\| echo "Not found"`
                          if previous fails       

  `;`                     Run commands            `date; pwd`
                          sequentially regardless 
                          of success              

  `&`                     Run command in          `sleep 60 &`
                          background              
  ------------------------------------------------------------------------------------

`>` can erase existing file contents. Use `>>` to append.

## 5. Users and groups

  ---------------------------------------------------------------------------------------
  Command                    Purpose                 Example
  -------------------------- ----------------------- ------------------------------------
  `whoami`                   Print current username  `whoami`

  `who`                      Show logged-in users    `who`

  `id`                       Show UID, primary and   `id ubuntu`
                             supplementary groups    

  `sudo CMD`                 Run command with        `sudo apt update`
                             elevated privileges     

  `su USER`                  Switch user             `su jethalal`

  `useradd USER`             Create user             `sudo useradd jethalal`

  `useradd -m USER`          Create user and home    `sudo useradd -m jethalal`
                             directory               

  `passwd USER`              Set/change user's       `sudo passwd jethalal`
                             password                

  `usermod -aG GROUP USER`   Add user to             `sudo usermod -aG devops jethalal`
                             supplementary group     

  `userdel USER`             Delete user             `sudo userdel jethalal`

  `userdel -r USER`          Delete user and         `sudo userdel -r jethalal`
                             home/mail files         

  `groupadd GROUP`           Create group            `sudo groupadd devops`

  `gpasswd -a USER GROUP`    Add user to group       `sudo gpasswd -a jethalal devops`

  `groupdel GROUP`           Delete group            `sudo groupdel devops`

  `groups USER`              Show user's group       `groups ubuntu`
                             memberships             
  ---------------------------------------------------------------------------------------

Notes: - User and group administration normally requires `sudo`. -
`useradd -m jethalal` creates `/home/jethalal` (subject to system
configuration). - `usermod -aG devops jethalal`: `-a` is important.
Without it, `-G` can replace the supplementary group list. - `su - USER`
starts a login shell and loads the target user's login environment.

## 6. Ownership and permissions

Example `ls -l` output:

``` text
-rw-rw-r-- 1 ubuntu devops 6 Oct 24 15:48 newfile.txt
drwxrwxrwx 3 ubuntu ubuntu 4096 Oct 23 03:04 cloud
```

The first character indicates type (`-` regular file, `d` directory, `l`
symbolic link). The next nine characters represent owner, group and
others permissions.

  -----------------------------------------------------------------------
  Permission              On a file               On a directory
  ----------------------- ----------------------- -----------------------
  `r`                     Read contents           List entries

  `w`                     Modify contents         Create/delete/rename
                                                  entries (subject to
                                                  other rules)

  `x`                     Execute file            Enter/traverse
                                                  directory
  -----------------------------------------------------------------------

Numeric values: `r=4`, `w=2`, `x=1`; therefore `7=rwx`, `6=rw-`,
`5=r-x`, `4=r--`.

  --------------------------------------------------------------------------
  Command                             Purpose
  ----------------------------------- --------------------------------------
  `chmod 777 DIR`                     Owner, group and others get `rwx`

  `chmod 755 DIR`                     Owner `rwx`; group/others `r-x`

  `chmod 644 FILE`                    Owner `rw-`; group/others `r--`

  `chmod u+x FILE`                    Add execute permission for owner

  `chmod g-w FILE`                    Remove group write permission

  `chmod -R 755 DIR`                  Apply permissions recursively; use
                                      carefully

  `chown USER FILE`                   Change owner,
                                      e.g. `sudo chown ubuntu file.txt`

  `chown USER:GROUP FILE`             Change owner and group

  `chgrp GROUP FILE`                  Change group,
                                      e.g. `sudo chgrp devops newfile.txt`

  `umask`                             Display current permission mask

  `umask 027`                         Set mask for current shell
  --------------------------------------------------------------------------

A common `umask` value is `0002`: regular files commonly become `664`
(from base `666`), and directories `775` (from base `777`). Applications
and default ACLs can affect actual results.

**Security:** Avoid `chmod 777` as a routine fix. It grants write
permission to every local user. Use the least permission required.

## 7. System information and administration

  Command        Purpose                                   Example
  -------------- ----------------------------------------- ---------------------
  `uname`        Kernel/system information                 `uname -a`
  `uptime`       Uptime and load averages                  `uptime`
  `date`         System date/time                          `date`
  `hostname`     Show hostname                             `hostname`
  `shutdown`     Shut down system                          `sudo shutdown now`
  `reboot`       Restart system                            `sudo reboot`
  `df -h`        Filesystem disk space in readable units   `df -h`
  `du -sh DIR`   Total directory disk usage                `du -sh cloud`
  `free -h`      RAM and swap usage                        `free -h`
  `ip address`   Network interfaces and IP addresses       `ip address`
  `ip route`     Kernel routing table                      `ip route`

## 8. Processes and monitoring

  -----------------------------------------------------------------------------------------
  Command                 Purpose                 Example
  ----------------------- ----------------------- -----------------------------------------
  `ps`                    Show processes          `ps`

  `ps aux`                Detailed process list   `ps aux`

  `top`                   Interactive live        `top`
                          process/resource        
                          monitor                 

  `kill PID`              Send default TERM       `kill 1234`
                          signal                  

  `kill -9 PID`           Send SIGKILL (force     `kill -9 1234`
                          termination)            

  `fuser FILE`            Show processes using a  `fuser -v file.txt`
                          file/filesystem         

  `watch CMD`             Repeat command at an    `watch -n 5 free -h`
                          interval                

  `nohup CMD &`           Run command immune to   `nohup python3 app.py > app.log 2>&1 &`
                          hangup, commonly in     
                          background              
  -----------------------------------------------------------------------------------------

Useful process filters:

``` bash
ps aux | grep ubuntu
ps aux | grep '[n]ginx'
ps aux | awk '{print $2}'
```

The first grep may also match its own process. The `[n]ginx` pattern is
one common way to avoid matching the grep command itself. `nohup` is not
a replacement for a proper service manager; learn `systemd`/`systemctl`
for services.

## 9. Package management

Ubuntu uses APT.

  -----------------------------------------------------------------------
  Command                             Purpose
  ----------------------------------- -----------------------------------
  `sudo apt update`                   Refresh package indexes

  `sudo apt upgrade`                  Upgrade installed packages

  `sudo apt install PACKAGE`          Install package,
                                      e.g. `sudo apt install zip`

  `sudo apt remove PACKAGE`           Remove package

  `sudo apt autoremove`               Remove automatically installed
                                      packages no longer needed

  `apt list --upgradable`             List packages with available
                                      upgrades
  -----------------------------------------------------------------------

Other package managers: - `apt`: Debian/Ubuntu. - `dnf`: modern
Fedora/RHEL family. - `yum`: older RHEL family; compatibility interface
on some newer systems. - `pacman`: Arch Linux. - `portage` / `emerge`:
Gentoo.

Package managers are distribution-specific; do not use APT commands on
every Linux distribution.

## 10. Archives and compression

### ZIP

``` bash
zip archive.zip FILE
zip -r ldf.zip cloud/
unzip ldf.zip
unzip -l ldf.zip
unzip ldf.zip -d extracted/
```

-   `zip -r` recursively archives a directory.
-   `unzip` extracts.
-   `unzip -l` lists contents without extracting.
-   `-d` chooses the extraction destination.

### TAR / GZIP

``` bash
tar -cvf archive.tar DIR/
tar -czvf cloud.tar.gz cloud/
tar -xvf archive.tar
tar -xzvf cloud.tar.gz
tar -tzvf cloud.tar.gz
```

Options: `c` create, `x` extract, `t` list, `v` verbose, `z` gzip
compression, `f` archive filename follows.

Common mistake: `tar -xvzf` is incomplete because `-f` needs a filename.
Use `tar -xzvf cloud.tar.gz`.

## 11. SSH, SCP and rsync

### SSH: connect to EC2

``` bash
ssh -i "testkey.pem" ubuntu@EC2_PUBLIC_DNS
```

### SCP: secure copy

Run from your local computer:

``` bash
# Local -> EC2
scp -i "testkey.pem" ./abc.txt ubuntu@EC2_PUBLIC_DNS:/home/ubuntu/

# EC2 -> local (run from local PowerShell)
scp -i "testkey.pem" ubuntu@EC2_PUBLIC_DNS:/home/ubuntu/abc.txt .
```

### rsync: synchronize files/directories

``` bash
# Local directory -> remote server
rsync -avz -e "ssh -i testkey.pem" ./project/ ubuntu@EC2_PUBLIC_DNS:/home/ubuntu/project/

# Preview only; no transfer
rsync -avzn ./project/ ubuntu@EC2_PUBLIC_DNS:/home/ubuntu/project/
```

Options: `-a` archive mode; `-v` verbose; `-z` compress during transfer;
`-r` recursive; `-n`/`--dry-run` preview; `-e` select remote shell
(usually SSH).

**SCP vs rsync:** `scp` is straightforward secure copying. `rsync` is
useful for repeat synchronization and efficient transfer of changes. A
trailing slash matters: `source/` syncs the contents; `source` syncs the
directory itself.

**Windows:** `scp` is commonly available with Windows OpenSSH. `rsync`
is not normally built into PowerShell; use WSL or another environment
with rsync installed.

## 12. Networking and DNS

  ------------------------------------------------------------------------------------------
  Command                      Purpose                 Example
  ---------------------------- ----------------------- -------------------------------------
  `ip address`                 Show IP                 `ip address`
                               addresses/interfaces    

  `ip route`                   Show routing table      `ip route`

  `ping -c 4 HOST`             Send four ICMP echo     `ping -c 4 google.com`
                               requests                

  `telnet HOST PORT`           Test TCP connection to  `telnet example.com 443`
                               host/port               

  `nslookup DOMAIN`            Query DNS               `nslookup google.com`

  `nslookup DOMAIN 8.8.8.8`    Query a specific DNS    `nslookup google.com 8.8.8.8`
                               resolver                

  `nslookup -type=MX DOMAIN`   Query mail exchanger    `nslookup -type=MX google.com`
                               records                 

  `nslookup -type=NS DOMAIN`   Query name server       `nslookup -type=NS google.com`
                               records                 

  `traceroute HOST`            Trace network hops      `traceroute google.com`

  `tracepath HOST`             Trace path and report   `tracepath google.com`
                               Path MTU                

  `curl URL`                   Make HTTP request /     `curl https://example.com`
                               transfer data           

  `wget URL`                   Download resource       `wget https://example.com/file.zip`
  ------------------------------------------------------------------------------------------

Install missing Ubuntu tools:

``` bash
sudo apt update
sudo apt install dnsutils traceroute iputils-tracepath telnet curl wget
```

Notes: - Successful Telnet connection means TCP connection established,
not necessarily that the application is healthy. - `* * *` in traceroute
means a probe did not receive a reply; it does not automatically mean
the route is broken. - `curl` is especially useful for APIs and HTTP
debugging; `wget` is commonly used for downloading files. - DNS lookup
does not prove a website/application is healthy.

## 13. Routes, scanning and firewall

  -----------------------------------------------------------------------------
  Command                    Purpose                 Example
  -------------------------- ----------------------- --------------------------
  `route -n`                 Display IPv4 routing    `route -n`
                             table (net-tools)       

  `ip route`                 Modern route inspection `ip route`

  `nmap HOST`                Scan a host for         `nmap 192.0.2.10`
                             discovered              
                             ports/services          

  `nmap -p 22,80,443 HOST`   Scan selected ports     `nmap -p 22,80,443 HOST`

  `iptables -L -n -v`        List firewall           `sudo iptables -L -n -v`
                             rules/counters          
  -----------------------------------------------------------------------------

Install if needed: `sudo apt install net-tools nmap iptables`.

-   `route` is an older `net-tools` utility; prefer `ip route` on modern
    Linux.
-   Modern systems may use nftables underneath; learn `nft` too.
-   Only scan systems you own or are explicitly authorized to test.
-   AWS Security Groups and Network ACLs are separate AWS controls;
    `iptables` does not modify them.

## 14. Text processing and log analysis

### grep: search text

``` bash
grep "GET" apt.log
grep -i "get" apt.log
grep -i -c "get" apt.log
grep -n "ERROR" app.log
grep -v "INFO" app.log
grep -r "TODO" .
```

Options: `-i` case-insensitive; `-c` count matching lines; `-n` line
numbers; `-v` non-matching lines; `-r` recursive search.

Correct syntax: `grep PATTERN FILE`. For example,
`grep -i "INFO" apt.log`. `grep -i apt.log` treats `apt.log` as the
pattern, not the filename.

### awk: extract fields and analyse logs

Syntax:

``` bash
awk 'pattern { action }' filename
```

Examples:

``` bash
# Print every line
awk '{print}' apt.log

# Print first and third fields
awk '{print $1, $3}' apt.log

# Print fields 1 and 5 only on lines containing GET
awk '/GET/ {print $1, $5}' apt.log

# Save GET lines to another file
awk '/GET/ {print}' apt.log > a.log

# Count matching lines containing "log"
awk '/log/ {count++} END {print count}' apt.log

# Count INFO lines
awk '/INFO/ {count++} END {print "INFO line count:", count}' apt.log

# Count GET lines
awk '/GET/ {count++} END {print "GET line count:", count}' apt.log

# Print second field (often PID in ps output)
ps aux | awk '{print $2}'
```

AWK basics: `$0` whole line; `$1` first field; `$2` second field; `NF`
number of fields; `NR` current line number; `count++` increments
counter; `END` runs after input processing.

Important: `awk '/GET/ {count++}' apt.log` increments a counter but does
not print it. Add `END {print count}`. This counts matching lines, not
unique IP addresses or every occurrence of a word.

### sed: stream editor

``` bash
# Print lines containing GET
sed -n '/GET/p' apt.log

# Replace POST with YASH in output (does not edit original file)
sed 's/POST/YASH/g' apt.log

# Print line numbers of lines containing GET
sed -n '/GET/=' apt.log

# Apply substitution only to lines 1 through 10
sed '1,10 s/POST/YASH/g' apt.log

# Print only lines 1 through 10
sed -n '1,10p' apt.log
```

Sed basics: `-n` suppresses automatic printing; `/PATTERN/p` prints
matching lines; `s/OLD/NEW/g` replaces all matches on each line;
`/PATTERN/=` prints matching line numbers; `1,10` is a line range; `q`
quits.

Common mistakes: - `sed -n '/INFO' apt.log` is incomplete. Use
`sed -n '/INFO/p' apt.log`. - `sed 's/POST/YASh?g' apt.log` is
malformed. Use `sed 's/POST/YASH/g' apt.log`. - If
`sed -n '/INFO/p' apt.log` prints nothing, there may be no matching INFO
lines. - `sed '1,10p' file` without `-n` prints matching lines plus
default output, causing duplicates. Use `sed -n '1,10p' file`. -
Substitution without `-i` prints transformed output; it does not modify
the original file.

### cut: select bytes or delimited fields

``` bash
cut -b 1-7 file.txt
cut -d ':' -f 1 /etc/passwd
```

`-b` selects byte positions; `-d` sets delimiter; `-f` selects fields.

### diff: compare files

``` bash
diff file1.txt file2.txt
```

Shows differences between two files.

## 15. Shell operators

  ------------------------------------------------------------------------------------
  Operator                Meaning                 Example
  ----------------------- ----------------------- ------------------------------------
  `>`                     Redirect output;        `ls > files.txt`
                          overwrite file          

  `>>`                    Append output           `date >> log.txt`

  `\|`                    Pipe output             `cat file.txt \| grep ERROR`

  `&&`                    Run next only if        `mkdir app && cd app`
                          previous succeeds       

  `\|\|`                  Run next only if        `cd missing \|\| echo "Not found"`
                          previous fails          

  `;`                     Run commands            `date; pwd`
                          sequentially            

  `&`                     Run in background       `sleep 60 &`
  ------------------------------------------------------------------------------------

## 16. Practical workflows

### Check a service port

``` bash
ping -c 4 example.com
telnet example.com 443
curl -I https://example.com
```

### Investigate logs

``` bash
tail -f app.log
grep -i "error" app.log
grep -i -c "error" app.log
awk '/GET/ {print $1, $7}' access.log
```

The AWK field number depends on the actual log format.

### Inspect a process

``` bash
ps aux | grep '[n]ginx'
top
```

### Check disk and memory

``` bash
df -h
du -sh /var/log
free -h
```

### Transfer a file to EC2

``` bash
scp -i "testkey.pem" ./abc.txt ubuntu@EC2_PUBLIC_DNS:/home/ubuntu/
```

### Archive and extract a directory

``` bash
zip -r project.zip project/
unzip -l project.zip
unzip project.zip -d extracted/

tar -czvf project.tar.gz project/
tar -tzvf project.tar.gz
tar -xzvf project.tar.gz
```

### Inspect permissions and ownership

``` bash
ls -l newfile.txt
chmod 640 newfile.txt
sudo chgrp devops newfile.txt
sudo chown ubuntu:devops newfile.txt
```

## 17. Safety notes

1.  Be careful with `rm -r`, `rm -rf`, `chmod -R`, and recursive
    ownership changes. Verify the target path first.
2.  Avoid `chmod 777` as a routine fix.
3.  Be careful changing firewall rules over SSH; a rule can block your
    SSH connection. Keep an alternative access method.
4.  Use `nmap` only on systems you own or are authorized to assess.
5.  Never share private SSH keys (`.pem`) or passwords.
6.  Remember that Linux firewall rules (`iptables`/`nftables`) are
    separate from AWS Security Groups and Network ACLs.
7.  Prefer `systemctl` for managing long-running services rather than
    relying on `nohup` for production applications.

## 18. Next commands to learn

These are useful additions for further study; they are not all commands
already practiced:

``` bash
systemctl
journalctl
ss
lsof
sort
uniq
wc
tee
xargs
basename
dirname
env
export
printenv
crontab
```

Suggested order: 1. `grep`, `sed`, `awk`, `cut`, `sort`, `uniq`, `wc` 2.
`find`, `xargs`, `lsof`, `ss` 3. `systemctl`, `journalctl` 4. `env`,
`export`, `crontab`

------------------------------------------------------------------------

**Learning note:** This cheat sheet combines commands practiced in
terminal sessions and screenshots with related reference commands.
Replace placeholders such as `EC2_PUBLIC_DNS`, `HOST`, and `FILE` with
your own values. Never upload private keys, passwords, or sensitive
server details to a public GitHub repository.
