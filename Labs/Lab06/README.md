# Lab 06: Linux Users, Groups, Permissions, Pipes, and Bash Scripting

**Estimated duration:** 3 hours  
**Submission repository:** `CC`  
**Submission directory:** `Labs/Lab06`  
**Screenshot directory:** `Labs/Lab06/screenshots`

Complete Tasks 1–13 on the same Ubuntu Server virtual machine used in Lab 05. Task 14 uses GitHub Codespaces for the remote-desktop exercise. Submit all evidence to your existing GitHub repository named exactly `CC`.

## Lab Objective

Practice Linux account administration, file ownership and permissions, log filtering, shell redirection, and incremental Bash scripting. Finish by starting and stopping a browser-accessible desktop in Codespaces.

By the end of this lab, you will be able to:

- switch to root and return to a regular account;
- create and remove practice users and groups;
- inspect account records without exposing password hashes;
- change primary and supplementary group membership;
- manage ownership and symbolic/numeric permissions;
- combine pipes, pagers, grep, and redirection;
- write scripts using variables, tests, arguments, loops, and functions; and
- start, connect to, and stop a Codespaces desktop.

## Prerequisites

- The Ubuntu Server VM used in Lab 05, with SSH access and a normal sudo-capable account.
- The Ubuntu username registered in the Student Marks Portal and hostname `ubuntu`.
- Vim and basic command-line skills from the previous labs.
- Access to your existing GitHub repository named `CC`.
- GitHub Codespaces availability for Task 14; use the source activity’s alternative cloud environment only if your instructor permits it.

## Required Folder Structure

Your completed Lab 06 directory must follow this structure:

```text
CC/
└── Labs/
    └── Lab06/
        ├── README.md
        ├── Lab6_Solution.pdf
        ├── workspace/
        │   └── setup.sh
        └── screenshots/
            ├── task1_set_root_password.png
            └── ...
```

Keep every original evidence screenshot inside `Labs/Lab06/screenshots/`. Store the final setup.sh in `workspace/`; keep passwords, private keys, and credential exports outside this directory.

## Task List

- [Screenshot Rules](#screenshot-rules)
- [Getting Started](#getting-started)
- [Task 1: Switch to Root and Return to Your Normal User](#task-1-switch-to-root-and-return-to-your-normal-user)
- [Task 2: Create tom and Inspect Account Files](#task-2-create-tom-and-inspect-account-files)
- [Task 3: Manage Primary and Secondary Groups](#task-3-manage-primary-and-secondary-groups)
- [Task 4: Compare User Creation and Remove Practice Accounts](#task-4-compare-user-creation-and-remove-practice-accounts)
- [Task 5: Manage File Ownership and Identify File Types](#task-5-manage-file-ownership-and-identify-file-types)
- [Task 6: Change Permissions Using Symbolic Mode](#task-6-change-permissions-using-symbolic-mode)
- [Task 7: Set Permissions Using Symbolic Assignments](#task-7-set-permissions-using-symbolic-assignments)
- [Task 8: Set Permissions Using Numeric Mode](#task-8-set-permissions-using-numeric-mode)
- [Task 9: Use Pipes, Pagers, Grep, and Redirection](#task-9-use-pipes-pagers-grep-and-redirection)
- [Task 10: Build setup.sh with Variables and File Checks](#task-10-build-setupsh-with-variables-and-file-checks)
- [Task 11: Compare Numbers and Strings in setup.sh](#task-11-compare-numbers-and-strings-in-setupsh)
- [Task 12: Print Arguments with a For Loop](#task-12-print-arguments-with-a-for-loop)
- [Task 13: Use While Loops and Functions](#task-13-use-while-loops-and-functions)
- [Task 14: Run a Remote Desktop in GitHub Codespaces](#task-14-run-a-remote-desktop-in-github-codespaces)
- [Exam Evaluation Questions](#exam-evaluation-questions)
- [Troubleshooting and Notes](#troubleshooting-and-notes)
- [Summary](#summary)
- [Submission](#submission)
- [Final Submission Checklist](#final-submission-checklist)

---

## Screenshot Rules

1. Save every required screenshot inside `Labs/Lab06/screenshots/`.
2. Use the exact filename and capitalization specified for its step.
3. Every terminal screenshot must show the active prompt, required command, and relevant output.
4. For normal VM work, the prompt must show `<github-username>@ubuntu`. After a required user switch, show the actual account, such as `root@ubuntu`, `student@ubuntu`, or `scooby@ubuntu`, and retain the transition command in the evidence. In Task 14, show the actual Codespace identity and your fork owner rather than imitating an Ubuntu VM prompt.
5. GitHub username comparison is case-insensitive, but its spelling must match the Student Marks Portal registration.
6. GUI screenshots must clearly show the requested application, setting, account, or result. Instructor reference images are not submission evidence.
7. Where a step groups commands into one screenshot, capture all of those commands and outputs together. Where evidence is explicitly optional or conditional, follow that condition.
8. Screenshots must be readable, unedited evidence from your own work. Do not crop away required identity, commands, filenames, or results. Tile windows before taking a screenshot when a combined view is requested.
9. Do not expose real passwords, password hashes, private keys, access tokens, recovery codes, or payment details. Supplied training-only example values may appear where the task requires them.
10. Do not copy, reuse, or modify another student's screenshots.
11. Commit and push the complete `Labs/Lab06` directory before the deadline.

Missing, unclear, incorrectly named, misplaced, edited, or copied evidence may receive zero marks for the affected check.

## Getting Started

1. From the root of your local `CC` repository, create the submission directories:

   ```bash
   mkdir -p Labs/Lab06/screenshots Labs/Lab06/workspace
   ```

2. Start the VM and use your normal Ubuntu account. Keep practice accounts separate from your normal login account. Tasks 10–13 use `~/Lab6` as the scripting workspace.
3. Read all guided tasks and the submission requirements before beginning.

---

## Task 1: Switch to Root and Return to Your Normal User

Goal: Demonstrate switching to the root account using `su -` and exiting back to your normal user.

### Steps

1. Set a root password (Ubuntu root is disabled by default; this enables `su -` temporarily for the lab):
```bash
sudo passwd root
# Enter a temporary root password for the lab
```

- Save screenshot as: `task1_set_root_password.png`

2. Switch to root and verify:
```bash
su -
whoami
id
```

- Save screenshot as: `task1_su_root.png`

3. Switch back to your normal user:
```bash
exit
whoami
```

- Save screenshot as: `task1_exit_to_user.png`

📸 Screenshot Required:

- `task1_set_root_password.png`
- `task1_su_root.png`
- `task1_exit_to_user.png`

---

## Task 2: Create tom and Inspect Account Files

Goal: Create a user named `tom`, then verify the account in system files.

### Steps

1. Create user `tom` (interactive, sets password and home directory):
```bash
sudo adduser tom
```

- Save screenshot as: `task2_adduser_tom.png`

2. Verify `tom` in system files (view and visually confirm presence):
```bash
cat /etc/passwd
```

- Save screenshot as: `task2_verify_passwd.png`

```bash
cat /etc/group
```

- Save screenshot as: `task2_verify_group.png`

```bash
sudo cut -d: -f1,3-9 /etc/shadow
```

- Save screenshot as: `task2_verify_shadow.png`

Notes:

- `/etc/shadow` stores password hashes and password-aging fields. The `cut` command displays the username and aging fields while omitting the password-hash field. Use `sudo` to read this protected file; do not publish password hashes.

📸 Screenshot Required:

- `task2_adduser_tom.png`
- `task2_verify_passwd.png`
- `task2_verify_group.png`
- `task2_verify_shadow.png`

---

## Task 3: Manage Primary and Secondary Groups

Goal: Create groups `developer`, `devops`, and `designer`. Change `tom`’s primary group and manage secondary groups.

### Steps

1. Create groups and verify by viewing `/etc/group` (visually confirm entries exist):
```bash
sudo groupadd developer
sudo groupadd devops
sudo groupadd designer
cat /etc/group
```

- Save screenshot as: `task3_groupadd.png`

2. Change `tom`’s primary group to `designer` and verify:
```bash
sudo usermod -g designer tom
id tom
```

- Save screenshot as: `task3_change_primary_group.png`

3. Add secondary groups `developer` and `devops` to `tom` and verify:
```bash
sudo usermod -aG developer,devops tom
id tom
groups tom
```

- Save screenshot as: `task3_add_secondary_groups.png`

4. Replace all secondary groups so only `tom` (user’s own group) remains and verify:
```bash
sudo usermod -G tom tom
id tom
groups tom
```

- Save screenshot as: `task3_reset_secondary_groups.png`

📸 Screenshot Required:

- `task3_groupadd.png`
- `task3_change_primary_group.png`
- `task3_add_secondary_groups.png`
- `task3_reset_secondary_groups.png`

---

## Task 4: Compare User Creation and Remove Practice Accounts

Goal: Create users using both `adduser` and `useradd`, demonstrate login/password/home directory differences, then delete users/groups.

### Steps

1. Create users:
```bash
sudo adduser jerry
sudo useradd scooby
```

- Save screenshot as: `task4_add_users.png`

2. Try to log in as Scooby immediately (expected authentication failure because there is no password yet):
```bash
su - scooby
```

- Save screenshot as: `task4_scooby_su_auth_failure.png`

3. Set a password for Scooby:
```bash
sudo passwd scooby
```

- Save screenshot as: `task4_set_password_scooby.png`

4. Try logging in as Scooby again (home directory still missing; expect a message such as “No directory, logging in with HOME=/”):
```bash
su - scooby
```

- Save screenshot as: `task4_scooby_su_no_home.png`

5. Show that Scooby’s home directory does not exist yet and what `/etc/passwd` says:
```bash
exit
cat /etc/passwd
ls -ld /home/scooby
```

- Save screenshot as: `task4_scooby_no_home.png`

6. Manually create Scooby’s home directory and set proper ownership and permissions:
```bash
sudo mkdir -p /home/scooby
sudo chown scooby:scooby /home/scooby
sudo chmod 750 /home/scooby
ls -ld /home/scooby
```

- Save screenshot as: `task4_scooby_create_home.png`

7. Log in as Scooby again and verify you land in the correct home directory:
```bash
su - scooby
pwd
ls -la
```

- Save screenshot as: `task4_scooby_login_success.png`

8. Verify users in system files and observe shell of `Scooby`:
```bash
exit
cat /etc/passwd
```

- Save screenshot as: `task4_verify_users.png`

9. Change the shell from `/bin/sh` to `/bin/bash`
```bash
sudo usermod -s /bin/bash scooby
su - scooby
echo "$SHELL"
exit
```

- Save screenshot as: `task4_shell_switching.png`

10.  Create groups:
```bash
sudo addgroup jolly
sudo groupadd anime
```

- Save screenshot as: `task4_add_groups.png`

11. Verify groups:
```bash
cat /etc/group
```

- Save screenshot as: `task4_verify_groups.png`

12. Delete groups and users:
```bash
sudo delgroup jolly
sudo groupdel anime
cat /etc/group

sudo deluser --remove-home jerry
sudo userdel -r scooby
cat /etc/passwd
```

- Save screenshots as: `task4_delete_groups.png`, `task4_delete_users.png`

📸 Screenshot Required:

- `task4_add_users.png`
- `task4_scooby_su_auth_failure.png`
- `task4_set_password_scooby.png`
- `task4_scooby_su_no_home.png`
- `task4_scooby_no_home.png`
- `task4_scooby_create_home.png`
- `task4_scooby_login_success.png`
- `task4_verify_users.png`
- `task4_shell_switching.png`
- `task4_add_groups.png`
- `task4_verify_groups.png`
- `task4_delete_groups.png`
- `task4_delete_users.png`

---

## Task 5: Manage File Ownership and Identify File Types

1. Create `Student`:
```bash
sudo adduser student
```

- Save screenshot as: `task5_create_student.png`

### Steps

2. Switch to Student and create files:
```bash
su - student
touch file1
mkdir -p dir1
touch dir1/file2
ls -l
```

- Save screenshot as: `task5_create_files.png`

3. Return from `student` to your normal sudo-capable account, then change the owner and group with separate commands. The practice `student` account does not need administrator privileges:
```bash
exit
sudo chown tom /home/student/file1
sudo ls -l /home/student/file1

sudo chgrp devops /home/student/file1
sudo ls -l /home/student/file1
```

- Save screenshots as: `task5_chown_file1.png`, `task5_chgrp_file1.png`

4. Restore the file owner to `student` before the permission exercises, then switch back to `student` and identify the files, directory, and device:
```bash
sudo chown student /home/student/file1
su - student
ls -l
ls -l dir1
ls -l /dev/null
file file1 dir1 /dev/null
```

- Save screenshot as: `task5_file_types.png`

5. Exit Student:
```bash
exit
```

- Save screenshot as: `task5_exit_student.png`

📸 Screenshot Required:

- `task5_create_student.png`
- `task5_create_files.png`
- `task5_chown_file1.png`
- `task5_chgrp_file1.png`
- `task5_file_types.png`
- `task5_exit_student.png`

---

## Task 6: Change Permissions Using Symbolic Mode

Target file: `~/file1` (run these as the Student user)

### Steps

1. Ensure Student and file present:
```bash
su - student
cd ~
ls -l file1
```

- Save screenshot as: `task6_su_student.png`

2. Remove all permissions:
```bash
chmod -rwx file1
ls -l file1
```

- Save screenshot as: `task6_chmod_remove_rwx.png`

3. Add read to all:
```bash
chmod +r file1
ls -l file1
```

- Save screenshot as: `task6_chmod_add_r.png`

4. Add execute to user:
```bash
chmod u+x file1
ls -l file1
```

- Save screenshot as: `task6_chmod_u_plus_x.png`

5. Add write to user and group:
```bash
chmod ug+w file1
ls -l file1
```

- Save screenshot as: `task6_chmod_ug_plus_w.png`

6. Remove all permissions (explicit):
```bash
chmod ugo-rwx file1
ls -l file1
```

- Save screenshot as: `task6_chmod_ugo_minus_rwx.png`

Return to your normal account before continuing:

```bash
exit
```

📸 Screenshot Required:

- `task6_su_student.png`
- `task6_chmod_remove_rwx.png`
- `task6_chmod_add_r.png`
- `task6_chmod_u_plus_x.png`
- `task6_chmod_ug_plus_w.png`
- `task6_chmod_ugo_minus_rwx.png`

---

## Task 7: Set Permissions Using Symbolic Assignments

Ensure you are Student:
```bash
su - student
cd ~
ls -l file1
```

- Save screenshot as: `task7_student_context.png`

### Steps

1. Set all to rwx:
```bash
chmod u=rwx,g=rwx,o=rwx file1
ls -l file1
```

- Save screenshot as: `task7_chmod_set_all_rwx.png`

2. Remove execute from group and others:
```bash
chmod g=rw,o=rw file1
ls -l file1
```

- Save screenshot as: `task7_remove_exec_go.png`

3. Remove all permissions:
```bash
chmod u=,g=,o= file1
ls -l file1
```

- Save screenshot as: `task7_remove_all_perms.png`

Return to your normal account before continuing:

```bash
exit
```

📸 Screenshot Required:

- `task7_student_context.png`
- `task7_chmod_set_all_rwx.png`
- `task7_remove_exec_go.png`
- `task7_remove_all_perms.png`

---

## Task 8: Set Permissions Using Numeric Mode

Ensure you are Student:
```bash
su - student
cd ~
ls -l file1
```

- Save screenshot as: `task8_student_context.png`

### Steps

Run each command and capture screenshot after each ls:

1.
```bash
chmod 777 file1
ls -l file1
```

- `task8_chmod_777.png`

2.
```bash
chmod 700 file1
ls -l file1
```

- `task8_chmod_700.png`

3.
```bash
chmod 744 file1
ls -l file1
```

- `task8_chmod_744.png`

4.
```bash
chmod 640 file1
ls -l file1
```

- `task8_chmod_640.png`

5.
```bash
chmod 664 file1
ls -l file1
```

- `task8_chmod_664.png`

6.
```bash
chmod 775 file1
ls -l file1
```

- `task8_chmod_775.png`

7.
```bash
chmod 750 file1
ls -l file1
```

- `task8_chmod_750.png`

Return to your normal account before continuing:

```bash
exit
```

📸 Screenshot Required:

- `task8_student_context.png`
- `task8_chmod_777.png`
- `task8_chmod_700.png`
- `task8_chmod_744.png`
- `task8_chmod_640.png`
- `task8_chmod_664.png`
- `task8_chmod_775.png`
- `task8_chmod_750.png`

---

## Task 9: Use Pipes, Pagers, Grep, and Redirection

1. less:
```bash
sudo cat /var/log/syslog | less
# quit q
```

- `task9_grep_less.png`

### Steps

2. more:
```bash
sudo cat /var/log/syslog | more
```

- `task9_grep_more.png`

3. grep failures/errors:
```bash
sudo grep -E 'fail|error' /var/log/syslog | head
```

- `task9_grep_head.png`

4. redirect:
```bash
sudo grep -i systemd /var/log/syslog > ~/syslog_systemd.txt
```

- `task9_redirect_overwrite.png`

append:
```bash
sudo grep -i network /var/log/syslog >> ~/syslog_systemd.txt
cat ~/syslog_systemd.txt
```

- `task9_redirect_append.png`

Alternative (journalctl) if needed:
```bash
sudo journalctl | less
sudo journalctl --no-pager | grep -i error > ~/journal_errors.txt
```

- `task9_journalctl_alternative.png`

📸 Screenshot Required:

- `task9_grep_less.png`
- `task9_grep_more.png`
- `task9_grep_head.png`
- `task9_redirect_overwrite.png`
- `task9_redirect_append.png`
- `task9_journalctl_alternative.png` (only when using the journal fallback)

---

## Task 10: Build setup.sh with Variables and File Checks

Goal: Using vim, write a script named `setup.sh` that implements each numbered step below. After writing the code for each step, run the script and capture screenshots showing the vim editor (script content) and the script output for that step. Students must add the code for each step into the same file `setup.sh` step-by-step (i.e., write 1., save, run and screenshot; then append 2., save, run and screenshot; and so on).

### Steps

For each step you MUST:

- Open vim and edit setup.sh
- Insert only the code shown for that step (append to the existing file)
- Save and quit vim (:wq)
- Make the file executable if not already: chmod +x setup.sh
- Run the script: ./setup.sh
- Capture two screenshots:
  - One showing the vim editor with the script content after you added the step (use the vim screen before :wq)
  - One showing the terminal output after running the script (show the command and the output)

Use your normal Ubuntu account for Tasks 10–13. Create the scripting workspace and keep working in this directory:

```bash
mkdir -p ~/Lab6
cd ~/Lab6
```

1. Include bash shebang

- Code to add (enter in vim as the first line of the file):
```bash
#!/bin/bash
```

- Steps:
  1. vim setup.sh → add the shebang line → save and quit
  2. chmod +x setup.sh
  3. ./setup.sh

- Screenshots:
  - vim editor showing the shebang: `task10_b1_vim.png`
  - script run output (likely no output but show ./setup.sh run): `task10_b1_run.png`

2. Define variable var1 and echo it

- Code to append:
```bash
# Define and show var1
var1="Hello from Lab 6"
echo "var1: $var1"
```

- Steps:
  1. vim setup.sh → append the code above → save and quit
  2. ./setup.sh

- Screenshots:
  - vim editor showing var1 code appended: `task10_b2_vim.png`
  - script run output showing var1 printed: `task10_b2_run.png`

3. Save output of ls -l into variable allFiles and echo it

- Code to append:
```bash
# Save ls -l to variable and display
allFiles="$(ls -l)"
echo "allFiles (ls -l):"
echo "$allFiles"
```

- Steps:
  1. vim setup.sh → append the code above → save and quit
  2. ./setup.sh

- Screenshots:
  - vim editor showing allFiles code appended: `task10_b3_vim.png`
  - script run output showing the ls -l content echoed: `task10_b3_run.png`

4. If directory dir1 exists echo a message; else create it

- Code to append:
```bash
# Directory check
if [ -d "dir1" ]; then
  echo "Directory dir1 exists."
else
  echo "Directory dir1 does not exist. Creating..."
  mkdir -p "dir1"
  echo "Directory dir1 created."
fi
```

- Steps:
  1. vim setup.sh → append the code above → save and quit
  2. ./setup.sh

- Screenshots:
  - vim editor showing dir1 check code: `task10_b4_vim.png`
  - script run output showing directory message or creation: `task10_b4_run.png`

5. If file dir1/file2 does not exist, create it

- Code to append:
```bash
# File check
if [ -f "dir1/file2" ]; then
  echo "file2 already exists."
else
  echo "file2 does not exist. Creating..."
  touch "dir1/file2"
  chmod a-rwx "dir1/file2"
  echo "file2 created."
fi
```

- Steps:
  1. vim setup.sh → append the code above → save and quit
  2. ./setup.sh

- Screenshots:
  - vim editor showing file2 check code: `task10_b5_vim.png`
  - script run output showing file creation message or existence: `task10_b5_run.png`

6. Check read, write, execute permissions on dir1/file2; grant missing user perms and show final ls

- Code to append:
```bash
# Permission checks for dir1/file2 (user permissions)
f="dir1/file2"
if [ ! -r "$f" ]; then
  echo "Read permission missing; granting to user..."
  chmod u+r "$f"
fi

if [ ! -w "$f" ]; then
  echo "Write permission missing; granting to user..."
  chmod u+w "$f"
fi

if [ ! -x "$f" ]; then
  echo "Execute permission missing; granting to user..."
  chmod u+x "$f"
fi

echo "Final permissions for $f:"
ls -l "$f"
```

- Steps:
  1. vim setup.sh → append the code above → save and quit
  2. ./setup.sh

- Screenshots:
  - vim editor showing permission-check code: `task10_b6_vim.png`
  - script run output showing the permission grants and final `ls -l dir1/file2`: `task10_b6_run.png`

Important notes for Task 10:

- Students MUST add the code incrementally exactly as described above (do not replace the file each time — append).
- Capture the vim editor screen (showing your code in the buffer) before you save and quit for each step (these are the `task10_b*_vim.png` files).
- After saving, run the script and capture the terminal output for that run (these are the `task10_b*_run.png` files).
- If a step reports that a directory/file already exists, that is acceptable — still capture the output screenshot showing the script's message.

📸 Screenshot Required:

- `task10_b1_vim.png`
- `task10_b1_run.png`
- `task10_b2_vim.png`
- `task10_b2_run.png`
- `task10_b3_vim.png`
- `task10_b3_run.png`
- `task10_b4_vim.png`
- `task10_b4_run.png`
- `task10_b5_vim.png`
- `task10_b5_run.png`
- `task10_b6_vim.png`
- `task10_b6_run.png`

---

## Task 11: Compare Numbers and Strings in setup.sh

Overwrite `setup.sh` for this exercise, then add one comparison at a time. After each edit, save the file, run the example arguments, and capture the editor and output before continuing.

### Steps

Important overall instructions

- Start by overwriting setup.sh (vim setup.sh) and add only what the step asks (do not add all tests at once).
- After editing in vim, save (:wq), make executable (chmod +x setup.sh) if needed, then run the script with the example commands shown for each step.
- For each step capture two screenshots:
  - A vim screenshot showing the current file buffer with the newly added lines (before :wq) — name as specified for the step.
  - A terminal screenshot showing the commands you ran (chmod +x setup.sh if necessary) and the script outputs for the example invocations — name as specified for the step.

- For the numeric comparisons, set a variable num=$1 at the top of the file before adding the individual if-tests (this will be the initial step). For string checks, set str=$2 before adding the string if-tests.

1. create file with shebang and set num and str variables

- In vim create/overwrite setup.sh and insert:
```bash
#!/bin/bash
num=$1
str=$2
```

- Save and quit (:wq)
- Make executable and run with examples:
```bash
chmod +x setup.sh
./setup.sh 10 Student
```

- Screenshots:
  - vim content: `task11_b0_vim.png`
  - run output: `task11_b0_run.png`

2. add the -eq test (equal)

- Append to setup.sh:
```bash
if [ "$num" -eq 10 ]; then
  echo "$num is equal to 10 (-eq)."
else
  echo "$num is NOT equal to 10 (-eq)."
fi
```

- Save and quit; then run these commands (capture both in one terminal screenshot):
```bash
./setup.sh 10 Student
./setup.sh 7 Student
```

- Screenshots:
  - vim content after edit: `task11_b1_vim.png`
  - run output demonstrating both cases: `task11_b1_run.png`

3. add the -ne test (not equal)

- Append to setup.sh:
```bash
if [ "$num" -ne 10 ]; then
  echo "$num is not equal to 10 (-ne)."
else
  echo "$num is equal to 10 (-ne false)."
fi
```

- Save and quit; run:
```bash
./setup.sh 7 Student
./setup.sh 10 Student
```

- Screenshots:
  - vim content: `task11_b2_vim.png`
  - run output: `task11_b2_run.png`

4. add the -gt test (greater than)

- Append:
```bash
if [ "$num" -gt 10 ]; then
  echo "$num is greater than 10 (-gt)."
else
  echo "$num is NOT greater than 10 (-gt)."
fi
```

- Run:
```bash
./setup.sh 12 Student
./setup.sh 9 Student
```

- Screenshots:
  - vim content: `task11_b3_vim.png`
  - run output: `task11_b3_run.png`

5. add the -lt test (less than)

- Append:
```bash
if [ "$num" -lt 10 ]; then
  echo "$num is less than 10 (-lt)."
else
  echo "$num is NOT less than 10 (-lt)."
fi
```

- Run:
```bash
./setup.sh 5 Student
./setup.sh 11 Student
```

- Screenshots:
  - vim content: `task11_b4_vim.png`
  - run output: `task11_b4_run.png`

6. add the -ge test (greater than or equal)

- Append:
```bash
if [ "$num" -ge 10 ]; then
  echo "$num is greater than or equal to 10 (-ge)."
else
  echo "$num is NOT greater than or equal to 10 (-ge)."
fi
```

- Run:
```bash
./setup.sh 10 Student
./setup.sh 8 Student
```

- Screenshots:
  - vim content: `task11_b5_vim.png`
  - run output: `task11_b5_run.png`

7. add the -le test (less than or equal)

- Append:
```bash
if [ "$num" -le 10 ]; then
  echo "$num is less than or equal to 10 (-le)."
else
  echo "$num is NOT less than or equal to 10 (-le)."
fi
```

- Run:
```bash
./setup.sh 10 Student
./setup.sh 12 Student
```

- Screenshots:
  - vim content: `task11_b6_vim.png`
  - run output: `task11_b6_run.png`

8. string equality test ( = )

- Ensure `str=$2` exists at top (1.). Append:
```bash
if [ "$str" = "Student" ]; then
  echo "Second argument equals 'Student' ( = )."
else
  echo "Second argument does NOT equal 'Student' ( = )."
fi
```

- Run:
```bash
./setup.sh 10 Student
./setup.sh 10 Test
```

- Screenshots:
  - vim content: `task11_b7_vim.png`
  - run output: `task11_b7_run.png`

9. string inequality test ( != )

- Append:
```bash
if [ "$str" != "Student" ]; then
  echo "Second argument is not equal to 'Student' ( != )."
else
  echo "Second argument equals 'Student' ( != false)."
fi
```

- Run:
```bash
./setup.sh 10 Test
./setup.sh 10 Student
```

- Screenshots:
  - vim content: `task11_b8_vim.png`
  - run output: `task11_b8_run.png`

10. check if second argument is empty (zero-length)

- Append:
```bash
if [ -z "$str" ]; then
  echo "Second argument is empty (zero-length)."
else
  echo "Second argument is not empty."
fi
```

- Run:
```bash
./setup.sh 10
./setup.sh 10 Student
```

- Screenshots:
  - vim content: `task11_b9_vim.png`
  - run output: `task11_b9_run.png`

Notes for Task 11

- Students MUST follow the incremental approach: add one test at a time and capture the vim buffer and run output screenshots for each step.
- Using the two example runs per step in a single terminal screenshot helps demonstrate both true and false results for the operator being taught.
- If a non-numeric value is used for $1 during integer comparisons the shell may print an error; that is expected here because we are demonstrating the operator behavior step-by-step. (If you want to avoid runtime errors, you can add integer validation before the comparisons — but for this exercise we are isolating each if-test into its own step.)

📸 Screenshot Required:

- `task11_b0_vim.png`
- `task11_b0_run.png`
- `task11_b1_vim.png`
- `task11_b1_run.png`
- `task11_b2_vim.png`
- `task11_b2_run.png`
- `task11_b3_vim.png`
- `task11_b3_run.png`
- `task11_b4_vim.png`
- `task11_b4_run.png`
- `task11_b5_vim.png`
- `task11_b5_run.png`
- `task11_b6_vim.png`
- `task11_b6_run.png`
- `task11_b7_vim.png`
- `task11_b7_run.png`
- `task11_b8_vim.png`
- `task11_b8_run.png`
- `task11_b9_vim.png`
- `task11_b9_run.png`

---

## Task 12: Print Arguments with a For Loop

Use the same `~/Lab6` workspace. This first example deliberately uses unquoted `$*`: the quoted input `"two words"` is split into two output words. The exam exercise uses `"$@"` to preserve each original argument.

### Steps

1. Create the script with shebang and basic structure

- Open vim and overwrite setup.sh:
```bash
vim setup.sh
```

- Insert these lines (first step — shebang and a short comment):
```bash
#!/bin/bash
# Script to demonstrate printing all user-entered arguments using $*
```

- Save and quit (:wq)
- Screenshots:
  - vim editor showing the shebang and comment: `task12_b1_vim.png`
  - run (no output expected but show ./setup.sh run): `task12_b1_run.png`

2. Append the for loop using $* and print each argument

- Re-open setup.sh in vim and append the following lines:
```bash
# Print all arguments using $*
echo "Printing all arguments using \$*:"
for arg in $*; do
  echo "Argument: $arg"
done
```

- Save and quit (:wq)
- Make the script executable and run it with example arguments:
```bash
chmod +x setup.sh
./setup.sh one "two words" three
```

- Screenshots:
  - vim editor showing the for-loop appended: `task12_b2_vim.png`
  - script run output showing the printed arguments: `task12_b2_run.png`

📸 Screenshot Required:

- `task12_b1_vim.png`
- `task12_b1_run.png`
- `task12_b2_vim.png`
- `task12_b2_run.png`

---

## Task 13: Use While Loops and Functions

Clear the previous code of setup.sh and write a new script, step-by-step, that:

### Steps

- Starts with a shebang line
- Implements an interactive while loop that prompts the user to enter numbers and keeps a running total until the user types `q` to quit; after each input the script echoes "Total Score: <current_total>"
- Implements a function `sum_two()` that runs its own interactive while loop doing the same accumulation and echoes the running totals
- Adds a second function that takes two numeric arguments, sums them, and returns the result via echo (demonstrated in the script)
- Important: if you move the while-loop logic into the `sum_two()` function, delete the standalone while-loop code to avoid running the same loop twice

1. Add the shebang line

- Open vim and overwrite setup.sh with the shebang line:
```bash
#!/bin/bash
```

- Save and quit (:wq)
- Make executable and run (no output expected):
```bash
chmod +x setup.sh
./setup.sh
```

- Screenshots:
  - vim editor showing shebang: `task13_b1_vim.png`
  - run output: `task13_b1_run.png`

2. Add the while-loop summation (interactive)

- Re-open setup.sh in vim and append the while-loop:
```bash
# While-loop summation (interactive)
sum=0
while true; do
  read -p "Enter a number (or 'q' to quit): " input
  if [ "$input" = "q" ]; then
    break
  fi

  sum=$((sum + input))
  echo "Total Score: $sum"
done
echo "Final total: $sum"
```

- Save and quit (:wq)
- Run the script and demonstrate a short session (example): enter `5`, then `7`, then `q`
```bash
./setup.sh
# interactively enter:
# 5
# 7
# q
```

- Screenshots:
  - vim editor showing while-loop appended: `task13_b2_vim.png`
  - run output showing the interactive session and totals: `task13_b2_run.png`

3. Add the interactive summation function and demonstrate it

- Re-open setup.sh in vim and append the function `sum_two()` which contains its own interactive while-loop:
```bash
# Function to accumulate scores interactively
sum_two() {
  sum=0
  while true; do
    read -p "Enter a number (or 'q' to quit): " input
    if [ "$input" = "q" ]; then
      break
    fi

    sum=$((sum + input))
    echo "Total Score: $sum"
  done
  echo "Function final total: $sum"
}

# Demonstrate the function
echo "Now calling sum_two function:"
sum_two
```

- Save and quit (:wq)
- Important: If you have the standalone while-loop from step 2 and you place this function into the script, delete the standalone loop to avoid executing the same interactive logic twice when running the script.
- Run the script and demonstrate a short session (example): enter `3`, `4`, `q` when prompted by the function:
```bash
./setup.sh
# when prompted by the function enter:
# 3
# 4
# q
```

- Screenshots:
  - vim editor showing function appended: `task13_b3_vim.png`
  - run output showing the function prompts and final total: `task13_b3_run.png`

4. Add a function that accepts two numbers and prints the sum for command substitution

- Re-open setup.sh in vim and append the following function and demonstration. This function accepts two numeric arguments, adds them, and return the sum. The script captures the printed result with command substitution. Bash `return` sets an exit status rather than returning an arbitrary numeric value.
```bash
# Function that sums two arguments and returns the result
sum_args() {
  a=$1
  b=$2
  echo $((a + b))
}

# Demonstrate sum_args function
echo "Now demonstrating sum_args function:"
result="$(sum_args 3 4)"
echo "sum_args(3,4) returned: $result"
```

- Save and quit (:wq)
- Run the script and capture the demonstration output:
```bash
chmod +x setup.sh
./setup.sh
# Observe the output that shows "sum_args(3,4) returned: 7"
```

- Screenshots:
  - vim editor showing function appended: `task13_b4_vim.png`
  - run output showing function demonstration and returned sum: `task13_b4_run.png`

Notes for Task 13:

- Overwrite previous contents of setup.sh at the start of Task 13 (step 1).
- Add code incrementally, save, run, and capture both the vim buffer and the run output screenshots for each numbered step.
- The while-loop and the functions are interactive; include the user inputs in the run screenshots to demonstrate the behavior.
- If you decide to use the function-based approach for interactive summation, remove the earlier standalone while-loop to avoid duplicate interaction.

📸 Screenshot Required:

- `task13_b1_vim.png`
- `task13_b1_run.png`
- `task13_b2_vim.png`
- `task13_b2_run.png`
- `task13_b3_vim.png`
- `task13_b3_run.png`
- `task13_b4_vim.png`
- `task13_b4_run.png`

---

## Task 14: Run a Remote Desktop in GitHub Codespaces

Task 14 is the Codespaces-specific activity. Tasks 1–13 remain on your Ubuntu Server VM. Use the actual Codespace account and hostname in its terminal; show your fork owner in GitHub evidence.

### Steps

Goal: Fork the specified repository to your GitHub account, open it in GitHub Codespaces, run the provided script to start a desktop GUI, connect to the GUI via the Codespaces forwarded port (6080) -> vnc.html, and then stop the GUI using the provided stop script.

Important notes before starting:

- GitHub Codespaces must be enabled for your account/org. Codespaces availability and billing may apply.
- The instructions below assume you have permission and capacity to create a Codespace for your fork.
- If Codespaces is not available, you may perform this step on another cloud environment that exposes the same port and scripts, but the screenshot filenames below assume Codespaces.

1. Fork the repository to your GitHub account

- Open the repo URL in your browser:
  - [Ubuntu Machine](https://github.com/WaqasSaleem97/UbuntuMachine)

- Click "Fork" (top-right) and fork it to your account.
- Save screenshot as: `task14_fork.png`

2. Open a Codespace on your fork

- In your forked repository on GitHub, click the green "Code" button → "Open with Codespaces" → "Create codespace on main" (or appropriate branch).
- Wait for the Codespace to initialize.
- Save screenshot as: `task14_codespace_launch.png`

3. Verify the start script is present and executable (capture evidence)

- In the Codespace terminal list files in the repo root and show the start script and stop script exist:
```bash
ls -l start-desktop.sh stop-desktop.sh
```

- If not executable, make it executable:
```bash
chmod +x start-desktop.sh stop-desktop.sh
```

- Save a screenshot showing the `ls -l` output (file listing) and the chmod command if applied:
  - `task14_start_script_ls.png`

4. Run the start script inside the Codespace terminal

- In the Codespace terminal run:
```bash
# Ensure the start script is executable
chmod +x start-desktop.sh

# Start the desktop GUI
./start-desktop.sh
```

- Capture the terminal output showing successful start messages.
- Save screenshot as: `task14_start_run.png`

5. Verify forwarded ports in Codespaces (Ports view)

- Open the Codespaces **Ports** view, confirm that port 6080 is forwarded, and keep its visibility set to **Private**.
- Save a screenshot of the Ports view showing port 6080 and its status:
  - `task14_ports_view.png`

6. Open forwarded port 6080 and connect to VNC HTML page

- In the Codespaces UI, open the forwarded port's preview URL or copy the forwarded URL and open it in your browser.
- Visit the port 6080 address and click the `vnc.html` link.
- When prompted for a password enter:
```
codespace
```

- Capture screenshots of:
  - The browser showing the forwarded port URL in the address bar / Codespaces preview: `task14_vnc_url.png`
  - The VNC password prompt (showing password field; do NOT include typed password in a screenshot): `task14_vnc_password_prompt.png`
  - The VNC session after successful connection showing the GUI/desktop: `task14_vnc_desktop.png`
  - (Optional) A focused screenshot of vnc.html UI showing the "Connect" button before/after connecting: `task14_vnc_connect.png`

7. Stop the GUI

- When finished, return to the Codespace terminal and run:
```bash
./stop-desktop.sh
```

- Capture the terminal output that shows the GUI stopping and any cleanup messages.
- Save screenshot as: `task14_stop_run.png`

Troubleshooting tips:

- If port 6080 is not visible, check Codespaces "Ports" view and forward it manually.
- If the VNC page fails to connect, verify the `start-desktop.sh` completed without errors and that the VNC server is listening on the expected port inside the Codespace.
- If Codespaces is unavailable for your account, consider forking and running the same scripts on another cloud VM that forwards port 6080 and adapt screenshots/file names accordingly.

📸 Screenshot Required:

- `task14_fork.png`
- `task14_codespace_launch.png`
- `task14_start_script_ls.png`
- `task14_start_run.png`
- `task14_ports_view.png`
- `task14_vnc_url.png`
- `task14_vnc_password_prompt.png`
- `task14_vnc_desktop.png`
- `task14_vnc_connect.png` (optional)
- `task14_stop_run.png`

---

## Exam Evaluation Questions

Complete these seven evaluation scenarios independently using the concepts practiced in this lab. Include your explanations and evidence in `Lab6_Solution.pdf`. Keep this README as the task instructions.

---

### 1. Group Management and Membership

**Scenario:**  
Create groups and manage a user’s primary and supplementary group memberships.

**Steps:**
1. Create the practice user `examuser` if it does not already exist, then create groups `g1`, `g2`, and `g3`.  
   - Screenshot: `Q1_groups_created.png`  
2. Change `examuser`’s primary group to `g3` and add `g1` and `g2` as supplementary groups.  
   - Screenshot: `Q1_group_changes.png`  
3. Show the final `id` and `/etc/group` lines that prove the changes.  
   - Screenshot: `Q1_group_verification.png`

📸 Screenshot Required:

- `Q1_groups_created.png`
- `Q1_group_changes.png`
- `Q1_group_verification.png`

---

### 2. Ownership and Permission Tasks

**Scenario:**  
Demonstrate ownership changes and apply both symbolic and numeric permission changes.

**Steps:**
1. Create `workspace/secret.txt`, change its owner to `examuser` and group to `g1`.  
   - Screenshot: `Q2_chown_chgrp.png`  
2. Remove all permissions for group and others using a symbolic command, then using a numeric command to achieve the same result.  
   - Screenshot: `Q2_symbolic_numeric.png`  
3. Show `ls -l` for the file after each change to document the permission bits.  
   - Screenshot: `Q2_permissions_ls.png`

📸 Screenshot Required:

- `Q2_chown_chgrp.png`
- `Q2_symbolic_numeric.png`
- `Q2_permissions_ls.png`

---

### 3. Pipes, Grep, and Redirection Practice

**Scenario:**  
Filter system logs and save results using redirection and piping.

**Steps:**
1. Use `grep` (or `journalctl` where applicable) with a pipe to find lines containing "error" or "fail" and show the first 20 results.  
   - Screenshot: `Q3_grep_pipe.png`  
2. Save the filtered results to a file `~/logs/errors.txt` using overwrite, then append additional matching lines using append redirection.  
   - Screenshot: `Q3_redirect_overwrite_append.png`  
3. Use a pager to view the saved file.  
   - Screenshot: `Q3_pager_view.png`

📸 Screenshot Required:

- `Q3_grep_pipe.png`
- `Q3_redirect_overwrite_append.png`
- `Q3_pager_view.png`

---

### 4. Script: Variables, Command Substitution, File & Dir Checks

**Scenario:**  
Build and run a script incrementally that demonstrates variables, command substitution, and filesystem checks.

**Steps:**
1. Create `setup.sh` with a shebang and a variable `var1` that you echo.  
   - Screenshot: `Q4_step1_var1.png`  
2. Append command substitution that stores `ls -l` output into a variable and echo it.  
   - Screenshot: `Q4_step2_allfiles.png`  
3. Append directory and file checks that create `dir1` and `dir1/file2` if missing, and display their final permissions.  
   - Screenshot: `Q4_step3_dirfile_checks.png`

📸 Screenshot Required:

- `Q4_step1_var1.png`
- `Q4_step2_allfiles.png`
- `Q4_step3_dirfile_checks.png`

---

### 5. Script: Comparisons and String Tests

**Scenario:**  
Incrementally add numeric and string comparison tests to a script and show both true/false cases.

**Steps:**
1. Overwrite `setup.sh` to set `num=$1` and `str=$2`, and add an `-eq` test showing true and false examples.  
   - Screenshot: `Q5_eq_examples.png`  
2. Append `-ne`, `-gt`, `-lt`, `-ge`, and `-le` tests and demonstrate at least one true and one false invocation for each.  
   - Screenshot: `Q5_numeric_tests.png`  
3. Append string equality (`=`) and inequality (`!=`) checks and a `-z` (zero-length) test for the second argument, demonstrating true/false cases.  
   - Screenshot: `Q5_string_tests.png`

📸 Screenshot Required:

- `Q5_eq_examples.png`
- `Q5_numeric_tests.png`
- `Q5_string_tests.png`

---

### 6. Script: For Loop and Argument Handling

**Scenario:**  
Write a script that prints all provided arguments and demonstrate correct handling of quoted multi-word arguments.

**Steps:**
1. Create/overwrite `setup.sh` to print every argument using `"$@"` in a for loop and save the file.  
   - Screenshot: `Q6_script_forloop_vim.png`  
2. Run the script with mixed single and quoted multi-word arguments and capture the output showing each argument on its own line.  
   - Screenshot: `Q6_forloop_run.png`

📸 Screenshot Required:

- `Q6_script_forloop_vim.png`
- `Q6_forloop_run.png`

---

### 7. Script: While Loop Summation and Functions

**Scenario:**  
Implement an interactive or non-interactive summation function and a demonstrated function that returns a numeric result.

**Steps:**
1. Write an interactive while-loop that accumulates numbers until `q` is entered and shows running totals.  
   - Screenshot: `Q7_while_session.png`  
2. Add a function that accepts two numeric arguments, returns their sum, and demonstrate capturing its result in a variable.  
   - Screenshot: `Q7_function_sum.png`

📸 Screenshot Required:

- `Q7_while_session.png`
- `Q7_function_sum.png`

---

## Troubleshooting and Notes

- Run administrative commands from your normal sudo-capable account. The practice accounts do not need sudo access.
- Restore the owner of `/home/student/file1` to `student` before Tasks 6–8. Only its owner or root can change its permissions.
- Return to the normal account after each permission task; avoid nesting repeated `su - student` sessions.
- If `/var/log/syslog` is unavailable, use `journalctl` as described in Task 9. For the same five required evidence points, substitute `sudo journalctl --no-pager` as the input to the pager/grep commands, and record the fallback in `task9_journalctl_alternative.png`.
- A grep search can legitimately return no matching log lines. Capture the command and result without inventing log entries.
- Use only integers and `q` in the interactive summation example. `return` is an exit status; Task 13 uses printed output and command substitution for the sum.
- After completing the root-login demonstration, lock the temporary root password when it is no longer needed with `sudo passwd -l root`.
- Keep Codespaces port 6080 private, stop the desktop with `./stop-desktop.sh`, and stop the Codespace when finished.

Source material: [CC_F25 Lab 06](https://github.com/WaqasSaleem97/CC_F25/blob/5490eee82f398eeb11a19fb04a89b34d0b2bdac9/Lab_Tasks/Lab_06/README.md). See the [Bash manual](https://www.gnu.org/software/bash/manual/bash.html) and [GitHub Codespaces port-forwarding documentation](https://docs.github.com/en/codespaces/developing-in-a-codespace/forwarding-ports-in-your-codespace) for the scripting and remote-desktop behavior.

## Summary

In this lab, you:

- managed root, regular users, and groups;
- inspected account records and changed group membership;
- changed file ownership and symbolic/numeric permissions;
- filtered logs with pipes, pagers, grep, and redirects;
- built and tested Bash variables, comparisons, argument loops, and functions; and
- launched and stopped a Codespaces desktop.

---

## Submission

1. Prepare a solution document containing:
   - a title page with your complete name, registration number, course code, section, and GitHub username;
   - headings for Tasks 1 through 14;
   - the exam evaluation questions and their evidence;
   - screenshots in the same order used in this README; and
   - a short caption below every screenshot.

2. Export the document using this exact filename:

   ```text
   Lab6_Solution.pdf
   ```

3. Place the PDF at `Labs/Lab06/Lab6_Solution.pdf`.

4. From your `CC` clone on the Ubuntu VM, copy the final Task 13 script:

   ```bash
   cp ~/Lab6/setup.sh Labs/Lab06/workspace/setup.sh
   ```

   If your `CC` clone is on the host instead, transfer the final script to that same relative destination before committing.

5. Keep every original screenshot inside `Labs/Lab06/screenshots/`.

6. From the root of the `CC` repository, commit and push the completed lab. The explicit PDF add includes the required solution even if your repository ignores PDF files:

   ```bash
   git status
   git add Labs/Lab06
   git add -f Labs/Lab06/Lab6_Solution.pdf
   git commit -m "Submit Lab 06 evidence"
   git push origin main
   ```

7. Open your `CC` repository on GitHub and confirm that the complete `Labs/Lab06` directory is visible.

---

## Final Submission Checklist

- [ ] `README.md` remains inside `Labs/Lab06/`.
- [ ] `Lab6_Solution.pdf` is inside `Labs/Lab06/` and opens correctly.
- [ ] Every required screenshot is inside `Labs/Lab06/screenshots/` with the exact filename.
- [ ] Terminal and GUI evidence follows the identity and screenshot rules, including the stated account/platform exceptions.
- [ ] `workspace/setup.sh` contains the final file required by this lab.
- [ ] Tasks 1 through 14 and all required results are included in the PDF.
- [ ] All exam evaluation questions and their evidence are included in the PDF.
- [ ] The Task 14 Codespace is stopped after collecting its evidence.
- [ ] No real password, password hash, private key, token, recovery code, payment detail, or credential export is included.
- [ ] My submission is stored in the GitHub repository named exactly `CC`.
- [ ] I committed and pushed the complete `Labs/Lab06` directory.
