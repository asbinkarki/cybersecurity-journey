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
