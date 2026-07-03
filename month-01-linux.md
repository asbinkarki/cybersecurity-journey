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
