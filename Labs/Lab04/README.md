# Lab 04: Virtualization and Linux Fundamentals

**Estimated duration:** 3 hours  
**Submission repository:** `CC`  
**Submission directory:** `Labs/Lab04`  
**Screenshot directory:** `Labs/Lab04/screenshots`

Complete the practical work using the Ubuntu Server virtual machine installed in Lab 01. Submit all evidence to your existing GitHub repository named exactly `CC`.

## Lab Objective

This lab provides hands-on practice with virtualization and core Linux fundamentals. You will inspect the Ubuntu Server virtual machine in VMware Workstation, connect to it through SSH, explore the Linux filesystem, perform essential command-line operations, collect system information, and manage a non-root user account.

By the end of this lab, you will be able to:

- Inspect a virtual machine's resources and network configuration.
- Connect to an Ubuntu Server virtual machine through SSH.
- Explore the Linux filesystem hierarchy and hidden files.
- Navigate directories and manage files from the command line.
- Examine kernel, CPU, memory, disk, operating-system, and process information.
- Create and verify a non-root user account.
- Create and run a small shell script as an optional bonus activity.

## Prerequisites

- The Ubuntu Server virtual machine installed in VMware Workstation during Lab 01.
- SSH access to the virtual machine from the host computer.
- A text editor inside the virtual machine, such as `nano` or `vim`.
- Access to the existing GitHub repository named `CC`.
- The Ubuntu username and hostname configured in Lab 01.

Do not use pipes (`|`) or shell redirection (`>` and `>>`) because these topics have not yet been covered in the course.

## Required Folder Structure

Your completed Lab 04 directory must follow this structure:

```text
CC/
└── Labs/
    └── Lab04/
        ├── README.md
        ├── Lab4_Solution.pdf
        ├── workspace/
        │   ├── python_project/
        │   │   ├── README.md
        │   │   ├── main.py
        │   │   └── .env
        │   ├── java_app_copy/
        │   │   └── (copy of the python_project files)
        │   └── run-demo.sh        # optional bonus file
        └── screenshots/
            ├── vm_settings.png
            ├── whoami_pwd.png
            ├── ls_root.png
            └── ...
```

Keep every original evidence screenshot inside `Labs/Lab04/screenshots/`. The `workspace/` directory must preserve the directory structure created during Task 4. Include `run-demo.sh` only if you complete the bonus task.

## Task List

- [Screenshot Rules](#screenshot-rules)
- [Getting Started](#getting-started)
- [Task 1: Verify VM Resources in VMware](#task-1-verify-vm-resources-in-vmware)
- [Task 2: Start the VM and Connect by SSH](#task-2-start-the-vm-and-connect-by-ssh)
- [Task 3: Explore the Linux Filesystem and Hidden Files](#task-3-explore-the-linux-filesystem-and-hidden-files)
- [Task 4: Practice Navigation and File Operations](#task-4-practice-navigation-and-file-operations)
- [Task 5: Collect System and Process Information](#task-5-collect-system-and-process-information)
- [Task 6: Create and Verify a Non-Root User](#task-6-create-and-verify-a-non-root-user)
- [Bonus Task 7: Create and Run a Shell Script](#bonus-task-7-create-and-run-a-shell-script)
- [Exam Evaluation Questions](#exam-evaluation-questions)
- [Summary](#summary)
- [Submission](#submission)
- [Final Submission Checklist](#final-submission-checklist)

---

## Screenshot Rules

1. Save every screenshot inside `Labs/Lab04/screenshots/`.
2. Use the exact filename and capitalization specified for each step.
3. Every terminal screenshot must clearly show:
   - the active terminal prompt;
   - the required command; and
   - the relevant result or output.

   For normal tasks, the prompt must show `<github-username>@ubuntu`. After switching users in Task 6, it must show `lab4user@ubuntu` where applicable.
4. The GitHub username comparison is case-insensitive, but the spelling must match the username registered in the Student Marks Portal.
5. GUI screenshots must clearly show the requested VMware setting or result.
6. Screenshots must be clear, readable, and created from your own work.
7. Do not crop out required identity, commands, filenames, or results.
8. Do not expose passwords, private keys, access tokens, recovery codes, or other credentials.
9. Do not copy, reuse, or modify another student's screenshots.
10. Commit and push the complete `Labs/Lab04` directory before the deadline.

Missing, unclear, incorrectly named, misplaced, edited, or copied evidence may receive zero marks for the affected check.

## Getting Started

1. From the root of your local `CC` repository, create the screenshot directory:

   ```bash
   mkdir -p Labs/Lab04/screenshots
   ```

2. Confirm that the Ubuntu Server virtual machine from Lab 01 starts successfully.
3. Confirm that the Ubuntu terminal prompt follows this pattern:

   ```text
   <github-username>@ubuntu:~$
   ```

4. Read all required tasks and the exam evaluation questions before beginning.

---

## Task 1: Verify VM Resources in VMware

Confirm the VM resources that were allocated in Lab 1.

### Steps
1. Open VMware Workstation and locate the Ubuntu Server VM you used in Lab 1.  
2. Inspect VM settings and note the following (no commands required for GUI): VM name, RAM, CPU, disk, and network adapter type.  
3. Take a screenshot of the VM settings window showing RAM, CPU, disk and networking. Save screenshot as: `vm_settings.png`

📸 Screenshot Required:
- `vm_settings.png`

---

## Task 2: Start the VM and Connect by SSH

Use a single preferred host-terminal method to connect to the VM. Do not switch between methods during the task.

### Steps
1. Start (or resume) the VM in VMware Workstation on your host.  
2. From your host, open your preferred terminal (for example: Windows Command Prompt, PowerShell, macOS Terminal, or Linux Terminal) and connect to the VM using SSH. Example:

```bash
ssh student@<vm-ip-address>
```

- If `whoami_pwd.png` does not clearly show the successful SSH connection and remote prompt, also save the SSH login result as `vm_login.png`.

3. After logging in, run both commands and capture them together in a single screenshot:

```bash
whoami
pwd
```

- Save a single screenshot that clearly shows both outputs as: `whoami_pwd.png`

📸 Screenshot Required:
- `vm_login.png` (optional if `whoami_pwd.png` provided)  
- `whoami_pwd.png` (required; must show both `whoami` and `pwd` outputs)

---

## Task 3: Explore the Linux Filesystem and Hidden Files

Explore Linux filesystem layout and hidden files. Capture outputs using screenshots only (do not create text files). For the short explanation required in this task, write the paragraph in an editor and capture it as a screenshot (`answers_md.png`) — do not supply the `.md` file.

### Steps
1. List root directory contents:

```bash
ls -la /
```

- Save screenshot as: `ls_root.png`

2. Inspect these directories (run each command and screenshot the output):

```bash
ls -la /bin
```
- Save screenshot as `ls_bin.png`.

```bash
ls -la /sbin
```
- Save screenshot as `ls_sbin.png`.

```bash
ls -la /usr
```
- Save screenshot as `ls_usr.png`.

```bash
ls -la /opt
```
- Save screenshot as `ls_opt.png`.

```bash
ls -la /etc
```
- Save screenshot as `ls_etc.png`.

```bash
ls -la /dev
```
- Save screenshot as `ls_dev.png`.

```bash
ls -la /var
```
- Save screenshot as `ls_var.png`.

```bash
ls -la /tmp
```
- Save screenshot as `ls_tmp.png`.

3. List your home directory and show hidden (dot) files:

```bash
ls -la ~
```

- Save screenshot as: `home_ls.png`

4. Write a short paragraph (3–5 sentences) that explains the difference between `/bin`, `/usr/bin` and `/usr/local/bin`. Open your editor:

```bash
nano ~/answers.md
```

- Type the paragraph in the editor, save and exit.  
- After saving, open the editor display (or show the file) and capture a screenshot of the paragraph. Save that screenshot as: `answers_md.png`

> Reminder: Do not upload the `.md` file—submit the screenshot `answers_md.png` as proof of your written answer.

📸 Screenshot Required:
- `ls_root.png`  
- `ls_bin.png`  
- `ls_sbin.png`  
- `ls_usr.png`  
- `ls_opt.png`  
- `ls_etc.png`  
- `ls_dev.png`  
- `ls_var.png`  
- `ls_tmp.png`  
- `home_ls.png`  
- `answers_md.png`

---

## Task 4: Practice Navigation and File Operations

Practice basic commands used daily on Linux systems. Use screenshots only for evidence.

### Steps
1. Create a workspace and navigate:

```bash
mkdir -p ~/lab4/workspace/python_project
```
- Save screenshot as `mkdir_workspace.png`.

```bash
cd ~/lab4/workspace/python_project
```
- Save screenshot as `cd_workspace.png`.

```bash
pwd
```
- Save screenshot as `pwd_workspace.png`.

2. Create files using an editor (open each editor session and save a screenshot showing content):

```bash
nano README.md
```
- Inside nano add: `Lab 4 README` and save.  
- Save screenshot of the editor after saving as `nano_readme.png`.

```bash
nano main.py
```
- Inside nano add: `print("hello lab4")` and save.  
- Save screenshot as `nano_main.png`.

```bash
nano .env
```
- Inside nano add: `ENV=lab4` and save.  
- Save screenshot as `nano_env.png`.

3. List files and capture:

```bash
ls -la
```
- Save screenshot as `workspace_ls.png`.

4. Copy, move and remove:

```bash
cp README.md README.copy.md
```
- After running, save screenshot as `cp_readme.png`.

```bash
mv README.copy.md README.dev.md
```
- After running, save screenshot as `mv_readme.png`.

```bash
rm README.dev.md
```
- After running, save screenshot as `rm_readme.png`.

```bash
mkdir -p ~/lab4/workspace/java_app
```
- Save screenshot as `mkdir_java_app.png`.

```bash
cp -r ~/lab4/workspace/python_project ~/lab4/workspace/java_app_copy
```
- After running, save screenshot as `cp_recursive.png`.

```bash
ls -la ~/lab4/workspace
```
- Save screenshot as `copy_verify.png`.

5. Use command history and tab completion:

```bash
history
```
- Save screenshot as `history.png`.

- Demonstrate tab completion (type partial name and press Tab) and capture that action as `tab_completion.png`.

📸 Screenshot Required:
- `mkdir_workspace.png`  
- `cd_workspace.png`  
- `pwd_workspace.png`  
- `nano_readme.png`  
- `nano_main.png`  
- `nano_env.png`  
- `workspace_ls.png`  
- `cp_readme.png`  
- `mv_readme.png`  
- `rm_readme.png`  
- `mkdir_java_app.png`  
- `cp_recursive.png`  
- `copy_verify.png`  
- `history.png`  
- `tab_completion.png`

---

## Task 5: Collect System and Process Information

Collect system information and observe processes. Use screenshots only.

### Steps
1. Kernel and OS:

```bash
uname -a
```
- Save screenshot as `uname.png`.

2. CPU (ensure `model name` visible):

```bash
cat /proc/cpuinfo
```
- Save screenshot as `cpuinfo.png`.

3. Memory:

```bash
free -h
```
- Save screenshot as `meminfo.png`.

4. Disk:

```bash
df -h
```
- Save screenshot as `diskinfo.png`.

5. View OS release information:

```bash
cat /etc/os-release
```
- Save screenshot as `os-release.png`.

6. Processes (show top lines of ps output):

```bash
ps aux
```
- Save screenshot as `processes.png`.

📸 Screenshot Required:
- `uname.png`  
- `cpuinfo.png`  
- `meminfo.png`  
- `diskinfo.png`  
- `os-release.png`  
- `processes.png`

---

## Task 6: Create and Verify a Non-Root User

Create a non‑root user and verify the account exists. This task does NOT add the created user to the sudo group.

### Steps
1. Create a new user named `lab4user`:

```bash
sudo adduser lab4user
```
- During prompts, capture the terminal and save screenshot as `adduser_lab4user.png`.

2. Verify the user entry:

```bash
getent passwd lab4user
```
- Save screenshot as `lab4user_passwd.png`.

3. Switch to the new user to verify login:

```bash
su - lab4user
```
- Save screenshot as `su_lab4user.png`.

4. (Optional) From the new user, attempt a sudo command to demonstrate that this account has not been granted sudo access:

```bash
sudo whoami
```
- Save screenshot as `sudo_whoami.png`.

5. Return to the original user:

```bash
exit
```
- Save screenshot as `exit_back.png`.

6. (Optional) Remove the test user when finished:

```bash
sudo deluser --remove-home lab4user
```
- If run, save screenshot as `deluser.png`.

📸 Screenshot Required:
- `adduser_lab4user.png`  
- `lab4user_passwd.png`  
- `su_lab4user.png`  
- `sudo_whoami.png` (optional; include it only if you perform the sudo test)  
- `exit_back.png`  
- (optional) `deluser.png`

---

## Bonus Task 7: Create and Run a Shell Script

This task is optional — complete it for extra practice or extra credit. It is not required for passing the core lab tasks.

### Steps
1. Open an editor to create the script:

```bash
nano ~/lab4/workspace/run-demo.sh
```
- Type the following lines into the editor (manually or paste), save and exit:

```
#!/bin/bash
echo "Lab 4 demo: current user is $(whoami)"
echo "Current time: $(date)"
uptime
free -h
```

- Save screenshot of the editor after saving the file as `nano_run_demo.png`.

2. Make the script executable:

```bash
chmod +x ~/lab4/workspace/run-demo.sh
```
- Save screenshot as `chmod_run_demo.png`.

3. Run the script as your regular user:

```bash
~/lab4/workspace/run-demo.sh
```
- Save screenshot of the script output as `run_demo_output.png`.

4. Optionally run it with sudo:

```bash
sudo ~/lab4/workspace/run-demo.sh
```
- Save screenshot as `run_demo_output_sudo.png`.

📸 Screenshot (Optional / Bonus) — Recommended if you complete this task:
- `nano_run_demo.png`  
- `chmod_run_demo.png`  
- `run_demo_output.png`  
- `run_demo_output_sudo.png` (optional)

---

## Exam Evaluation Questions

### 1. Remote Access Verification (Cyber Login Check)

**Scenario:**  
You are part of a SOC (Security Operations Center) investigating unauthorized access to a Linux server hosted on VMware. Prove you can securely connect and verify your identity.

**Steps:**
1. Connect to the Ubuntu VM remotely from your host terminal.  
   - Screenshot as `Q1_remote_connection.png`
2. Verify your current user and home directory path.  
   - Screenshot as `Q1_user_verification.png`
3. Confirm you are connected to the correct host machine.  
   - Screenshot as `Q1_host_confirmation.png`

---

### 2. Filesystem Inspection for Forensic Evidence

**Scenario:**  
The incident response team suspects malicious files in system directories. You must explore the filesystem to locate and document the system’s structure.

**Steps:**
1. Display the contents of the root directory.  
   - Screenshot as `Q2_root_listing.png`
2. Display the OS version and release information.  
   - Screenshot as `Q2_os_version.png`
3. Explore and record directory listings for `/bin`, `/sbin`, `/usr`, `/opt`, `/etc`, `/dev`, `/var`, and `/tmp`.  
   - Screenshot as `Q2_directory_evidence.png`
4. Display all hidden files in your home directory.  
   - Screenshot as `Q2_hidden_files.png`
5. Create a markdown file summarizing your findings on key binary directories.  
   - Screenshot as `Q2_report_file.png`

---

### 3. Evidence Handling and File Operations

**Scenario:**  
You are creating a sandbox environment to safely analyze and handle suspicious files collected from a compromised system.

**Steps:**
1. Create a structured folder hierarchy under your home directory for analysis.  
   - Screenshot as `Q3_workspace_created.png`
2. Create three text files, including one hidden file, in your workspace.  
   - Screenshot as `Q3_files_created.png`
3. Create a backup copy of one file, rename it, and then delete it after verification.  
   - Screenshot as `Q3_backup_handling.png`
4. Copy the entire workspace as an evidence backup folder.  
   - Screenshot as `Q3_workspace_backup.png`
5. Display your command history to document all actions performed.  
   - Screenshot as `Q3_command_history.png`
6. Demonstrate Linux auto-completion by typing a partial command or filename.  
   - Screenshot as `Q3_autocomplete.png`

---

### 4. System Profiling and Process Monitoring

**Scenario:**  
You are investigating a potential malware infection that is consuming excessive resources on the Linux VM.

**Steps:**
1. Display the system’s OS and kernel version for the investigation report.  
   - Screenshot as `Q4_system_info.png`
2. Display CPU, memory, and disk usage information.  
   - Screenshot as `Q4_resource_info.png`
3. Display all active running processes to identify suspicious activity.  
   - Screenshot as `Q4_process_list.png`

---

### 5. User Account Audit and Privilege Escalation Simulation

**Scenario:**  
You are performing a **user activity audit** on a compromised Linux server.  
The SOC suspects a newly created account (`lab4user`) may have been used for unauthorized access.  
Your task is to simulate the account creation, perform privilege tests, and analyze authentication logs for forensic evidence.

**Steps:**
1. Create a new test user named `lab4user`.  
   - Screenshot as `Q5_user_created.png`
2. Verify that the new user record exists in the system’s user database.  
   - Screenshot as `Q5_user_verified.png`
3. Log in as `lab4user` and confirm successful login.  
   - Screenshot as `Q5_user_login.png`
4. Attempt to run an administrative command as `lab4user` (expect permission denied).  
   - Screenshot as `Q5_permission_denied.png`
5. Switch back to your main analyst account.  
   - Screenshot as `Q5_switch_back.png`
6. Inspect the system authentication logs located at `/var/log/auth.log` to determine whether the `lab4user` account attempted any logins (successful or failed).  
   - Screenshot as `Q5_authlog_analysis.png`
7. (Optional) Remove the `lab4user` account after the audit and verify deletion.  
   - Screenshot as `Q5_user_removed.png`
---

**Reminder:**  
Take a screenshot after every step and name it as shown above. Include all screenshots in your submission for full credit.

## Summary

In this lab, you:

- verified the Ubuntu Server virtual machine configuration in VMware Workstation;
- connected to the virtual machine from the host computer using SSH;
- explored the Linux filesystem and hidden files;
- practiced command-line navigation and file operations;
- collected system, resource, and process information;
- created and verified a non-root user account; and
- optionally created and executed a shell script.

---

## Submission

1. Prepare a solution document containing:
   - a title page with your complete name, registration number, course code, section, and GitHub username;
   - headings for Tasks 1 through 6;
   - the exam evaluation questions and their evidence;
   - screenshots in the same order used in this README; and
   - a short caption below every screenshot.

2. Export the solution document using this exact filename:

   ```text
   Lab4_Solution.pdf
   ```

3. Place the PDF at:

   ```text
   Labs/Lab04/Lab4_Solution.pdf
   ```

4. Copy the complete `~/lab4/workspace/` directory into `Labs/Lab04/workspace/`. Preserve its structure:
   - `workspace/python_project/` must contain `README.md`, `main.py`, and `.env`;
   - `workspace/java_app_copy/` must contain the copied project files; and
   - `workspace/run-demo.sh` is included only if you complete the bonus task.

   Git does not track an empty `java_app/` directory, so it does not need to appear on GitHub unless you place a file inside it.

5. Keep all original screenshots inside `Labs/Lab04/screenshots/`.

6. From the root of the `CC` repository, commit and push the completed lab:

   ```bash
   git status
   git add Labs/Lab04
   git commit -m "Submit Lab 04 evidence"
   git push origin main
   ```

7. Open the `CC` repository on GitHub and confirm that the complete `Labs/Lab04` directory is visible.

---

## Final Submission Checklist

- [ ] `README.md` remains inside `Labs/Lab04/`.
- [ ] `Lab4_Solution.pdf` is inside `Labs/Lab04/` and opens correctly.
- [ ] Every required screenshot is inside `Labs/Lab04/screenshots/`.
- [ ] Every screenshot uses the exact filename specified in this README.
- [ ] Every terminal screenshot shows the correct active prompt, the required command, and its relevant output.
- [ ] `workspace/python_project/` contains `README.md`, `main.py`, and `.env`.
- [ ] `workspace/java_app_copy/` contains the copied project files.
- [ ] `workspace/run-demo.sh` is included only if I completed the bonus task.
- [ ] The filename `os-release.png` is used for the operating-system release screenshot.
- [ ] No password, private key, token, recovery code, or other credential is visible.
- [ ] All required tasks and exam evaluation questions are included in the solution PDF.
- [ ] My submission is stored in the GitHub repository named exactly `CC`.
- [ ] I committed and pushed the complete `Labs/Lab04` directory.
