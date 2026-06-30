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
