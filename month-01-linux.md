# Day 1 — June 28

What I did
- Read articles about Kali Linux, pentesting, cybersecurity & digital forensics
- Revised basic Linux commands: pwd, ls, cd, cat, less, head, tail
- Ran nmap for the first time — scanned own machine and a demo target

Commands practiced
pwd, ls, ls -la, cd, cat, less, head, tail
nmap 127.0.0.1
nmap -sV 127.0.0.1

What I learned
- nmap shows open ports on a target
- -sV flag detects what service/version is running on each port
- Every open port is a potential entry point for an attacker

Tomorrow
- Day 2: File operations (mkdir, cp, mv, rm)
- Continue nmap: try scanning my local network range
## Day 2 — June 29

What I did
- Learned and practiced file operation commands in terminal
- Did exercises with creating, copying, renaming and deleting files
- Tested understanding with SOC analyst scenarios

Commands practiced
mkdir, touch, cp, mv, rm, rm -r, rm -rf

What I learned
- mkdir = create new folder
- touch = create empty file
- cp = duplicate file, original stays (2 files)
- mv = rename or move file, original gone (1 file)
- rm = delete file permanently, no recycle bin
- rm -r = delete entire folder and everything inside
- rm -rf = force delete, no confirmation prompt
- cp is used in forensics to preserve original evidence
- mv is used to rename or relocate files

SOC context
- Always cp suspicious files before analyzing — never touch the original
- Attackers use rm -rf to delete their tools and cover tracks
- Forensics investigators take disk image before touching anything

Tomorrow
Day 3 — Reading files properly (cat, less, head, tail, tail -f)
## Day 3 — Reading Files

 What I did
- Learned the 4 main file-reading commands: cat, less, head, tail
- Practiced with a real system file (/etc/os-release)
- Learned about file permissions blocking log access (sudo)
- Created a test file (numbers.txt, 1-100) to safely practice without empty logs
- Compared cat vs less vs head vs tail directly

Commands practiced
cat, sudo cat, wc -l, less, head, tail, seq

What I learned
- cat = prints whole file instantly — good for small files, floods screen on big ones
- less = scrollable view, press q to quit — better for large files
- head = shows first 10 lines by default
- tail = shows last 10 lines by default
- sudo needed to read root-owned files like system logs ("Permission denied" without it)
- wc -l counts lines in a file — useful to check if a log is empty before reading it
- Some Kali logs were empty (boot.log, dpkg.log) — likely rotated into .gz files

Still to do
- tail -f (live log watching) — continuing tomorrow

Tomorrow
Finish Day 3: tail -f live log watching
Then Day 4: grep — searching inside files
## Day 4 — July 3 (Continuation of Day 3)

 What I did
- Finished tail -f concept from Day 3
- Learned about binary files vs readable log files
- Learned where login attempts are actually recorded
- Understood how real SOC analysts monitor logs live
- Learned what SIEM is and why it exists

 Commands practiced
sudo tail /var/log/installer/syslog
sudo tail -f /var/log/installer/syslog
sudo find /var/log -type f -size +0c 2>/dev/null
last -f /var/log/btmp.1

 What I learned
- tail -f = live log watching, like CCTV for log files
- auth.log = where SSH login attempts are recorded
- apache2/access.log = where website login attempts are recorded
- Same IP + hundreds of failed logins per minute = brute force attack
- Binary files look like garbage text — need special commands to read
- SIEM = collects logs from ALL servers into one place, fires alerts automatically
- SOC analyst flow: SIEM alert → investigate → grep/tail logs → find IP → block it

SOC context
- In real companies nobody watches log files manually all day
- SIEM tools like Splunk do the watching and fire alerts
- tail and grep are investigation tools used AFTER an alert fires
- Will learn Splunk properly in Month 3

Tomorrow
Day 5 — grep (searching inside log files)
Learning style: Case first → Why → Practice in Kali → Understand output

## Day 5 — July 5

 What I did
- Learned grep — most important SOC log analysis command
- Practiced 4 different grep flags with real examples
- Searched through actual installer logs on Kali

 Commands practiced
grep "word" file
grep -c "word" file
grep -n "word" file
grep -i "word" file
grep -r "word" /var/log/installer/

 What I learned
- grep = Ctrl+F but for millions of lines instantly
- -c = count only, no lines shown
- -n = show line number of each match
- -i = ignore uppercase/lowercase (important in SOC)
- -r = search ALL files in a folder at once
- Different systems write Failed/FAILED/failed differently — always use -i

SOC context
- SIEM fires alert → you grep the logs for suspicious IP or keyword
- grep -r searches all log files at once — saves huge time during investigation
- grep -c gives instant count — 2847 failed logins = definitely brute force attack

 Tomorrow
- Finish grep: pipes with grep (most powerful part)
- Then Day 6: find command
## Day 6 — July 5

 What I did
- Completed grep pipes — most powerful part of grep
- Built the full SOC attacker detection command from scratch
- Learned how to chain commands together with pipes
- Created a fake auth.log and practiced real SOC investigation

 Commands practiced
grep "Failed password" auth.log | wc -l
grep "1" numbers.txt | sort
grep "1" numbers.txt | sort | uniq -c
grep "1" numbers.txt | sort | uniq -c | sort -rn
grep "Failed password" auth.log | awk '{print $11}' | sort | uniq -c | sort -rn | head -10

 What I learned
- Pipe | sends output of one command into the next
- wc -l = counts lines of any input
- sort = organizes output in order
- uniq -c = removes duplicates and counts occurrences
- sort -rn = reverse numerical order, highest first
- head -10 = show only top 10 results
- awk '{print $11}' = extracts 11th word from each line (IP address)
- grep -c counts within grep only, wc -l counts anything piped into it

 SOC context
- Full attacker detection command:
  grep "Failed password" auth.log | awk '{print $11}' | sort | uniq -c | sort -rn | head -10
- Output shows which IP failed most times = attacker
- This command finds attackers in seconds from millions of log lines
- Used by SOC analysts daily for brute force detection

 Tomorrow
Day 7 — Week 1 review and self test (no notes!)
Based on what you shared, here's a GitHub update that reflects your actual progress today.

### Day 7 — July 6

 What I did

* Learned how Linux pipelines (`|`) work by connecting multiple commands together.
* Built a complete SOC log analysis pipeline to identify attackers from SSH login logs.
* Practiced understanding the logic behind each command instead of just memorizing the syntax.

 Commands practiced

```bash
grep -i "Failed password" auth.log
grep -i "Failed password" auth.log | awk '{print $6}'
grep -i "Failed password" auth.log | awk '{print $6}' | sort
grep -i "Failed password" auth.log | awk '{print $6}' | sort | uniq -c
grep -i "Failed password" auth.log | awk '{print $6}' | sort | uniq -c | sort -rn | head -10
```

 What I learned

* `|` (pipe) passes the output of one command to the next.
* `awk '{print $6}'` extracts the source IP address from log entries.
* `sort` groups identical IP addresses together.
* `uniq -c` counts how many times each unique IP appears.
* `sort -rn` displays the highest counts first.
* `head -10` shows only the top results.

 SOC context

* Extracted attacker IP addresses from authentication logs.
* Counted failed login attempts for each IP.
* Ranked attackers by the number of failed login attempts to quickly identify the most suspicious sources.
* Learned that understanding the logic behind each step is more important than simply memorizing commands.

 Tomorrow

* Practice building Linux pipelines without help.
* Start learning the `find` command for searching files and directories efficiently.

## Day 8 — July 10, 2026

 Topic: Log Analysis with grep & awk (Linux CLI basics for SOC)

**What I did:**
- Practiced parsing a sample `auth.log` file for failed SSH login attempts
- Used `grep -i "Failed password" auth.log` to filter matching log lines (case-insensitive)
- Debugged command mistakes myself: typo'd command (`greps`), wrong bracket in awk (`]` instead of `}`), missing pipe before `awk`
- Learned how `awk '{print $6}'` extracts the 6th whitespace-separated field (the source IP) from each line
- Chained `grep | awk` to extract just the offending IPs from failed login attempts
- Started building toward `sort | uniq -c` to count failed attempts per IP (worst-offender triage)

Commands practiced:
```bash
grep -i "Failed password" auth.log
grep -i "Failed password" auth.log | awk '{print $6}'
grep -i "Failed password" auth.log | awk '{print $6}' | sort | uniq -c
```
## Day 9 — July 12

 What I did
- Completed the full SOC attacker detection pipeline
- Built it step by step myself this time
- Third time doing this pipeline — understanding improving each session

 Full pipeline command
grep -i "Failed password" auth.log | awk '{print $6}' | sort | uniq -c | sort -rn | head -10

 What each step does
grep -i  → find all failed login lines (ignore case)
awk '{print $6}' → extract only the IP address (6th word)
sort     → group same IPs together
uniq -c  → count each unique IP
sort -rn → highest count first
head -10 → show top 10 only

 What I learned
- awk extracts specific columns, $6 means 6th word in each line
- sort must come before uniq -c otherwise counting won't work
- -i flag in grep catches both Failed and failed
- Pipeline output: 4 192.168.1.105 = attacker, block immediately

 SOC context
- This command finds top attacker in seconds from millions of log lines
- 192.168.1.105 attacked 4 times = block this IP
- Real auth.log would have thousands of entries, same command works

 Week 1 Complete ✅
- Navigation and file operations
- Reading files (cat, less, head, tail, tail -f)
- grep and pipes
- Built first SOC investigation command

 Tomorrow
Week 2 — Permissions, Users and Processes

Key takeaway:
`awk` doesn't "know" what an IP is — it just prints whatever column number you tell it to (`$6` = 6th word). Understanding column position in log lines is the real skill, not memorizing the command.

Next up: Sort `uniq -c` output by count (descending) to surface the top attacking IP first — closer to real SOC triage workflow.

### Week 2 Started
## Day 1 — July 13

 What I did
- Learned Linux file permissions (Week 2 begins)
- Read permissions from real files on my Kali system
- Changed permissions using chmod
- Learned SUID and why it is dangerous
- Found all SUID files on my system

Commands practiced
ls -la
chmod +x scan.sh
chmod 755 file
chmod 644 file
find / -perm -4000 2>/dev/null

 What I learned
- Permission format: -rwxrwxrwx (type, owner, group, others)
- r=4, w=2, x=1 — add them for chmod numbers
- 7=rwx, 6=rw-, 5=r-x, 4=r--, 0=---
- chmod 644 = owner rw, group r, others r (most common for files)
- chmod 755 = owner rwx, group rx, others rx (common for scripts)
- SUID (s) = file runs as root regardless of who executes it
- find / -perm -4000 = finds all SUID files on system

SOC context
- Attacker drops malicious script → chmod +x → executes it
- SUID files in /tmp or /home = red flag, not normal system files
- Always check SUID files during incident response
- Compare against known baseline to spot new/suspicious ones
- archive-key.asc owned by root in kali home folder = suspicious in real scenario

 Tomorrow
Day 9 — Users and Groups
## Week 2 Day 2 — July 14

### What I did
- Learned Linux users and groups
- Read /etc/passwd and understood each field
- Created, switched to, and deleted a test user
- Understood how attackers create backdoor accounts

### Commands practiced
cat /etc/passwd
grep "kali" /etc/passwd
whoami
id
sudo adduser testuser
su - testuser
sudo userdel testuser
grep "testuser" /etc/passwd

### What I learned
- /etc/passwd = stores all users (55 accounts on my Kali)
- /etc/shadow = stores encrypted passwords
- UID 1000+ = real human accounts, below 1000 = system accounts
- id command shows all groups you belong to
- sudo group = can run root commands
- testuser could not run sudo — not in sudoers group
- No output from grep after deletion = user successfully removed

### SOC context
- Attacker creates backdoor user after breaching server
- Check cat /etc/passwd | tail -10 for recently added accounts
- Unknown user with UID 1000+ = red flag
- Attacker tries to add backdoor user to sudo group = full root access
- Always check id username to see if suspicious user has sudo

### Tomorrow
Day 10 — sudo and Privilege Escalation concepts
