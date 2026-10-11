# Lab 05: Ubuntu Package Management, GUI Applications, and Vim

**Estimated duration:** 3 hours  
**Submission repository:** `CC`  
**Submission directory:** `Labs/Lab05`  
**Screenshot directory:** `Labs/Lab05/screenshots`

Complete the practical work using the Ubuntu Server virtual machine prepared in Lab 01. Submit all evidence to your existing GitHub repository named exactly `CC`.

## Lab Objective

This lab provides hands-on practice with Ubuntu package management, Snap applications, lightweight graphical environments, third-party repositories, and Vim. You will install and remove Java with both `apt` and `apt-get`, compare package-update commands, install Visual Studio Code with Snap, configure XFCE and XRDP, manage graphical boot targets, add external package sources, and edit a Kubernetes manifest with Vim.

By the end of this lab, you will be able to:

- discover which Ubuntu package provides a missing command;
- install and remove packages with `apt` and `apt-get`;
- explain the difference between `apt update` and `apt upgrade`;
- install and verify a Snap application from the command line;
- configure XFCE, XRDP, and LightDM on Ubuntu Server;
- control whether Ubuntu boots to a graphical or command-line target;
- configure a signed third-party APT repository;
- install applications from Personal Package Archives (PPAs);
- create and edit a valid Kubernetes YAML manifest with Vim; and
- use essential Vim navigation, deletion, search, substitution, and undo commands.

## Prerequisites

- The Ubuntu Server virtual machine installed in VMware Workstation during Lab 01.
- SSH access to the virtual machine from the host computer.
- A regular Ubuntu account with `sudo` privileges.
- The Ubuntu username must match the GitHub username registered in the Student Marks Portal. Username comparison is case-insensitive.
- A working internet connection and DNS resolution inside the virtual machine.
- An RDP client on the host computer.
- At least 4 GB of VM memory and sufficient free disk space for the GUI applications.
- Access to the existing GitHub repository named `CC`.

Creating a VMware snapshot before installing the GUI and third-party repositories is strongly recommended.

## Required Folder Structure

Your completed Lab 05 directory must follow this structure:

```text
CC/
└── Labs/
    └── Lab05/
        ├── README.md
        ├── Lab5_Solution.pdf
        ├── workspace/
        │   └── k8s-sample.yaml
        └── screenshots/
            ├── task1_java_suggestion.png
            ├── task2_aptget_install.png
            ├── task3_apt_update.png
            └── ...
```

Keep every original evidence screenshot inside `Labs/Lab05/screenshots/`. The final Kubernetes manifest must be copied into `Labs/Lab05/workspace/`.

## Task List

- [Screenshot Rules](#screenshot-rules)
- [Getting Started](#getting-started)
- [Task 1: Discover a Missing Command and Install Java Using apt](#task-1-discover-a-missing-command-and-install-java-using-apt)
- [Task 2: Install and Remove Java Using apt-get](#task-2-install-and-remove-java-using-apt-get)
- [Task 3: Compare apt update and apt upgrade](#task-3-compare-apt-update-and-apt-upgrade)
- [Task 4: Install Visual Studio Code Using Snap](#task-4-install-visual-studio-code-using-snap)
- [Task 5: Install XFCE and XRDP](#task-5-install-xfce-and-xrdp)
- [Task 6: Configure LightDM and Control GUI Boot](#task-6-configure-lightdm-and-control-gui-boot)
- [Task 7: Install Google Chrome from Its APT Repository](#task-7-install-google-chrome-from-its-apt-repository)
- [Task 8: Install Applications from PPAs](#task-8-install-applications-from-ppas)
- [Task 9: Create a Kubernetes Manifest with Vim](#task-9-create-a-kubernetes-manifest-with-vim)
- [Task 10: Edit and Discard Changes in Vim](#task-10-edit-and-discard-changes-in-vim)
- [Task 11: Practice Vim Editing and Navigation](#task-11-practice-vim-editing-and-navigation)
- [Task 12: Practice Vim Search and Substitution](#task-12-practice-vim-search-and-substitution)
- [Exam Evaluation Question](#exam-evaluation-question)
- [Summary](#summary)
- [Submission](#submission)
- [Final Submission Checklist](#final-submission-checklist)

---

## Screenshot Rules

1. Save every screenshot inside `Labs/Lab05/screenshots/`.
2. Use the exact filename and capitalization specified for each step.
3. Every terminal screenshot must clearly show:
   - the active terminal prompt;
   - the required command; and
   - the relevant result or output.
4. The terminal prompt must show `<github-username>@ubuntu`. The GitHub username comparison is case-insensitive, but its spelling must match the username registered in the Student Marks Portal.
5. GUI screenshots must clearly show the requested application, setting, login screen, or result.
6. Where a task explicitly provides two evidence alternatives, submit only one of the listed alternatives.
7. Screenshots must be clear, readable, and created from your own work.
8. Do not crop out required identity, commands, filenames, or results.
9. Do not expose passwords, private keys, access tokens, recovery codes, or other credentials.
10. Do not copy, reuse, or modify another student's screenshots.
11. Commit and push the complete `Labs/Lab05` directory before the deadline.

Missing, unclear, incorrectly named, misplaced, edited, or copied evidence may receive zero marks for the affected check.

## Getting Started

1. From the root of your local `CC` repository, create the required directories:

   ```bash
   mkdir -p Labs/Lab05/screenshots Labs/Lab05/workspace
   ```

2. Start the Ubuntu Server virtual machine and connect to it through SSH.
3. Confirm that the terminal prompt follows this pattern:

   ```text
   <github-username>@ubuntu:~$
   ```

4. Confirm that internet access, DNS resolution, and `sudo` work correctly.
5. Read all required tasks and the exam evaluation question before beginning.

---

## Task 1: Discover a Missing Command and Install Java Using apt

Use Ubuntu's missing-command suggestion to identify a Java package, install it with `apt`, verify it, and remove it.

### Steps

1. On a clean VM where Java is not installed, run:

   ```bash
   java
   ```

   Capture the missing-command message and its suggested package. Save the screenshot as `task1_java_suggestion.png`.

2. Install one package suggested by Ubuntu. Replace `<java-package>` with the exact package name shown on your VM:

   ```bash
   sudo apt install <java-package> -y
   ```

   Save the completed installation output as `task1_java_install.png`. Record the selected package name because you must reuse it in Task 2.

3. Verify Java:

   ```bash
   java --version
   ```

   Save the result as `task1_java_version.png`.

4. Remove the same package:

   ```bash
   sudo apt remove <java-package> -y
   ```

   Save the result as `task1_java_remove.png`.

5. Run `java` again and capture the missing-command result as `task1_java_not_found.png`.

6. Clear Bash's remembered command locations and test once more:

   ```bash
   hash -r
   java
   ```

   Save both commands and the result in `task1_hash_clear.png`.

📸 Screenshot Required:

- `task1_java_suggestion.png`
- `task1_java_install.png`
- `task1_java_version.png`
- `task1_java_remove.png`
- `task1_java_not_found.png`
- `task1_hash_clear.png`

---

## Task 2: Install and Remove Java Using apt-get

Repeat the Java package workflow with `apt-get`. Use the exact `<java-package>` recorded in Task 1 so that installation and removal refer to the same package.

### Steps

1. Refresh package information and install the package:

   ```bash
   sudo apt-get update
   sudo apt-get install <java-package> -y
   ```

   Save the successful installation as `task2_aptget_install.png`.

2. Verify Java:

   ```bash
   java --version
   ```

   Save the result as `task2_java_version_after_aptget.png`.

3. Remove the same package and any dependencies that are no longer required:

   ```bash
   sudo apt-get remove <java-package> -y
   sudo apt-get autoremove -y
   ```

   Save the completed removal as `task2_aptget_remove.png`.

4. Clear the command cache and confirm that Java is unavailable:

   ```bash
   hash -r
   java
   ```

   Save the result as `task2_hash_after_remove.png`.

📸 Screenshot Required:

- `task2_aptget_install.png`
- `task2_java_version_after_aptget.png`
- `task2_aptget_remove.png`
- `task2_hash_after_remove.png`

---

## Task 3: Compare apt update and apt upgrade

Run both commands and explain their different purposes.

### Steps

1. Refresh the local package index:

   ```bash
   sudo apt update
   ```

   Save the completed result as `task3_apt_update.png`.

2. Install available upgrades for currently installed packages:

   ```bash
   sudo apt upgrade -y
   ```

   Save the completed result as `task3_apt_upgrade.png`.

3. Write a three-to-five-sentence explanation:

   ```bash
   nano ~/apt_update_vs_upgrade.md
   ```

   Your explanation must state that `apt update` refreshes package metadata, while `apt upgrade` installs available upgrades based on that metadata. Save the file, display it, and capture the result:

   ```bash
   cat ~/apt_update_vs_upgrade.md
   ```

   Save the screenshot as `task3_explanation.png`.

📸 Screenshot Required:

- `task3_apt_update.png`
- `task3_apt_upgrade.png`
- `task3_explanation.png`

---

## Task 4: Install Visual Studio Code Using Snap

Install Visual Studio Code from Snap using the command line. Keep it installed for Tasks 5 and 6.

### Steps

1. Install Visual Studio Code:

   ```bash
   sudo snap install code --classic
   ```

   Save the completed installation as `task4_snap_install.png`.

2. Verify that Snap lists the package:

   ```bash
   snap list code
   ```

   Save the result as `task4_snap_list.png`.

3. Display the installed version:

   ```bash
   code --version
   ```

   Save the result as `task4_code_version_or_info.png`.

4. Show the Snap command location:

   ```bash
   ls -l /snap/bin/code
   ```

   Save the result as `task4_snap_bin_location.png`.

Do not remove Visual Studio Code.

📸 Screenshot Required:

- `task4_snap_install.png`
- `task4_snap_list.png`
- `task4_code_version_or_info.png`
- `task4_snap_bin_location.png`

---

## Task 5: Install XFCE and XRDP

Install a lightweight desktop and enable remote access. Then connect from the host and launch Visual Studio Code.

> Installing a desktop environment consumes additional memory and disk space. Take a VMware snapshot first if you may need to restore the server.

### Steps

1. Update installed packages:

   ```bash
   sudo apt update
   sudo apt upgrade -y
   ```

   Save the completed result as `task5_update.png`.

2. Install XFCE and its additional utilities:

   ```bash
   sudo apt install xfce4 xfce4-goodies -y
   ```

   If a display-manager selection appears, select `lightdm`. Save the completed installation as `task5_xfce_install.png`.

3. Install and enable XRDP:

   ```bash
   sudo apt install xrdp -y
   sudo adduser xrdp ssl-cert
   sudo systemctl enable --now xrdp
   ```

   Save the completed commands as `task5_xrdp_enable.png`.

4. Verify the service:

   ```bash
   sudo systemctl status xrdp --no-pager
   ```

   Save the active service result as `task5_xrdp_status.png`.

5. Configure XFCE as the remote session:

   ```bash
   echo xfce4-session > ~/.xsession
   cat ~/.xsession
   ```

   Save the command and verification as `task5_xsession.png`.

6. From the host computer, open an RDP client and connect to the VM's IP address. Log in with the Ubuntu account and save the visible remote desktop as `task5_rdp_connect.png`.

7. From a terminal inside the XFCE session, launch Visual Studio Code:

   ```bash
   code
   ```

   Save a screenshot that clearly shows Visual Studio Code running in the remote desktop as `task5_vscode_launch.png`.

8. Close Visual Studio Code and log out of the RDP session.

📸 Screenshot Required:

- `task5_update.png`
- `task5_xfce_install.png`
- `task5_xrdp_enable.png`
- `task5_xrdp_status.png`
- `task5_xsession.png`
- `task5_rdp_connect.png`
- `task5_vscode_launch.png`

---

## Task 6: Configure LightDM and Control GUI Boot

Configure the LightDM greeter, test graphical and command-line boot targets, and start and stop the GUI manually.

### Steps

1. Install LightDM and its GTK greeter:

   ```bash
   sudo apt install lightdm lightdm-gtk-greeter -y
   ```

   Save the result as `task6_lightdm_install.png`.

2. Create the LightDM configuration:

   ```bash
   sudo mkdir -p /etc/lightdm/lightdm.conf.d
   echo -e "[Seat:*]\ngreeter-session=lightdm-gtk-greeter\nuser-session=xfce\nautologin-user-timeout=0" | sudo tee /etc/lightdm/lightdm.conf.d/99-xfce.conf
   ```

   Save the command and displayed configuration as `task6_lightdm_config.png`.

3. Remove stale session files if they exist:

   ```bash
   sudo rm -f /var/lib/lightdm/.Xauthority
   rm -f ~/.Xauthority
   rm -rf ~/.cache/sessions
   ```

   Save the commands as `task6_lightdm_cleanup.png`.

4. Restart and verify LightDM:

   ```bash
   sudo systemctl restart lightdm
   sudo systemctl status lightdm --no-pager
   ```

   Save the result as `task6_lightdm_restart.png`.

5. Enable graphical boot:

   ```bash
   sudo systemctl enable lightdm
   sudo systemctl set-default graphical.target
   systemctl get-default
   ```

   Save the commands and `graphical.target` result as `task6_gui_enable_boot.png`.

6. Reboot the VM:

   ```bash
   sudo reboot
   ```

   After the VM starts, capture the graphical login screen as `task6_after_reboot_gui.png`.

7. Reconnect through SSH, switch the default boot mode back to the command line, and disable automatic LightDM startup:

   ```bash
   sudo systemctl set-default multi-user.target
   sudo systemctl disable lightdm
   systemctl get-default
   ```

   Save the commands and `multi-user.target` result as `task6_gui_disable_boot.png`.

8. Reboot again:

   ```bash
   sudo reboot
   ```

   After the VM starts, capture the CLI login or SSH session as `task6_after_reboot_cli.png`.

9. Start the GUI manually without changing the default target:

   ```bash
   sudo systemctl start lightdm
   systemctl is-active lightdm
   ```

   Save the active result as `task6_gui_start.png`.

10. Open the GUI through the console or RDP and launch Visual Studio Code. Save Visual Studio Code running as `task6_vscode_launch.png`.

11. Return to SSH or a text console and stop the manually started GUI:

   ```bash
   sudo systemctl stop lightdm
   systemctl is-active lightdm
   ```

   Save the inactive result as `task6_gui_stop.png`.

📸 Screenshot Required:

- `task6_lightdm_install.png`
- `task6_lightdm_config.png`
- `task6_lightdm_cleanup.png`
- `task6_lightdm_restart.png`
- `task6_gui_enable_boot.png`
- `task6_after_reboot_gui.png`
- `task6_gui_disable_boot.png`
- `task6_after_reboot_cli.png`
- `task6_gui_start.png`
- `task6_vscode_launch.png`
- `task6_gui_stop.png`

---

## Task 7: Install Google Chrome from Its APT Repository

First demonstrate why the package is unavailable on a clean Ubuntu system. Then configure Google's signed APT repository and install the package.

Only obtain signing keys and repository information from the software vendor's official domain.

### Steps

1. Before adding Google's repository, attempt the installation:

   ```bash
   sudo apt install google-chrome-stable -y
   ```

   Capture the expected package-not-found result as `task7_install_chrome_error.png`.

2. Inspect the existing third-party source directory:

   ```bash
   ls -la /etc/apt/sources.list.d/
   ```

   Save the result as `task7_ls_sources_list_d.png`.

3. Install the tools needed to download and convert the signing key:

   ```bash
   sudo apt update
   sudo apt install curl gpg -y
   ```

   Save the result as `task7_prerequisites.png`.

4. Create the keyring directory and import Google's signing key:

   ```bash
   sudo mkdir -p /etc/apt/keyrings
   curl -fsSL https://dl.google.com/linux/linux_signing_key.pub | sudo gpg --dearmor --yes -o /etc/apt/keyrings/google-chrome.gpg
   ```

   Save the completed command as `task7_add_key.png`.

5. Create a dedicated HTTPS source file:

   ```bash
   echo "deb [arch=amd64 signed-by=/etc/apt/keyrings/google-chrome.gpg] https://dl.google.com/linux/chrome/deb/ stable main" | sudo tee /etc/apt/sources.list.d/google-chrome.list
   ```

   Save the command and output as `task7_create_google_chrome_list.png`.

6. Verify the exact repository entry:

   ```bash
   cat /etc/apt/sources.list.d/google-chrome.list
   ```

   Save the result as `task7_verify_source.png`.

7. Refresh APT metadata:

   ```bash
   sudo apt update
   ```

   Save the successful result as `task7_apt_update.png`.

8. Install Google Chrome:

   ```bash
   sudo apt install google-chrome-stable -y
   ```

   Save the completed installation as `task7_install_chrome.png`.

9. Verify the installed version:

   ```bash
   google-chrome --version
   ```

   Save the result as `task7_chrome_version.png`.

📸 Screenshot Required:

- `task7_install_chrome_error.png`
- `task7_ls_sources_list_d.png`
- `task7_prerequisites.png`
- `task7_add_key.png`
- `task7_create_google_chrome_list.png`
- `task7_verify_source.png`
- `task7_apt_update.png`
- `task7_install_chrome.png`
- `task7_chrome_version.png`

---

## Task 8: Install Applications from PPAs

Add PPAs for Audacity and OBS Studio, install the applications, and verify each application through either the GUI or its version output.

> A PPA is a third-party source. Confirm that a PPA supports your Ubuntu release before adding it, and never expose credentials in an error screenshot.

### Steps

1. Install the PPA-management utility:

   ```bash
   sudo apt update
   sudo apt install software-properties-common -y
   ```

2. Add the Audacity PPA:

   ```bash
   sudo add-apt-repository ppa:ubuntuhandbook1/audacity -y
   ```

   Save the result as `task8_add_ppa_audacity.png`.

3. Refresh package metadata:

   ```bash
   sudo apt update
   ```

   Save the result as `task8_apt_update_audacity.png`.

4. Install Audacity:

   ```bash
   sudo apt install audacity -y
   ```

   Save the result as `task8_install_audacity.png`.

5. Verify Audacity using one of these methods:
   - launch Audacity in XFCE and save `task8_audacity_launch.png`; or
   - run `audacity --version` and save `task8_audacity_version.png`.

6. Add the OBS Studio PPA:

   ```bash
   sudo add-apt-repository ppa:obsproject/obs-studio -y
   ```

   Save the result as `task8_add_ppa_obs.png`.

7. Refresh package metadata:

   ```bash
   sudo apt update
   ```

   Save the result as `task8_apt_update_obs.png`.

8. Install OBS Studio:

   ```bash
   sudo apt install obs-studio -y
   ```

   Save the result as `task8_install_obs.png`.

9. Verify OBS Studio using one of these methods:
   - launch OBS Studio in XFCE and save `task8_obs_launch.png`; or
   - run `obs --version` and save `task8_obs_version.png`.

📸 Screenshot Required:

- `task8_add_ppa_audacity.png`
- `task8_apt_update_audacity.png`
- `task8_install_audacity.png`
- `task8_audacity_launch.png` or `task8_audacity_version.png`
- `task8_add_ppa_obs.png`
- `task8_apt_update_obs.png`
- `task8_install_obs.png`
- `task8_obs_launch.png` or `task8_obs_version.png`

---

## Task 9: Create a Kubernetes Manifest with Vim

Confirm that Vim is available, create a Lab 05 working directory, and use Vim to create a valid Kubernetes Pod manifest.

### Steps

1. Check Vim:

   ```bash
   vim
   ```

   Save the Vim start screen or the command-not-found result as `task9_vim_check.png`. Press `:q` to exit if Vim opens.

   If Vim is not installed, install it:

   ```bash
   sudo apt update
   sudo apt install vim -y
   ```

   Save `task9_vim_install.png` only when installation was necessary.

2. Create and enter the working directory:

   ```bash
   mkdir -p ~/Lab5
   cd ~/Lab5
   pwd
   ```

   Save the result as `task9_mkdir_cd.png`.

3. Open the manifest:

   ```bash
   vim k8s-sample.yaml
   ```

4. Press `i` and enter this YAML exactly:

   ```yaml
   apiVersion: v1
   kind: Pod
   metadata:
     name: nginx-pod
   spec:
     containers:
       - name: nginx
         image: nginx:1.19
         ports:
           - containerPort: 80
     restartPolicy: Always
   ```

   Save the editor showing the complete YAML as `task9_vim_edit.png`.

5. Press `Esc`, type `:wq`, and press Enter. Verify the saved file:

   ```bash
   ls -l k8s-sample.yaml
   cat k8s-sample.yaml
   ```

   Save the result as `task9_k8s_saved.png`.

📸 Screenshot Required:

- `task9_vim_check.png`
- `task9_vim_install.png` only if Vim was not already installed
- `task9_mkdir_cd.png`
- `task9_vim_edit.png`
- `task9_k8s_saved.png`

---

## Task 10: Edit and Discard Changes in Vim

Add a permanent annotation, then make and discard a temporary edit.

### Steps

1. Open the manifest:

   ```bash
   cd ~/Lab5
   vim k8s-sample.yaml
   ```

2. Under `metadata`, add the annotation so that the section becomes:

   ```yaml
   metadata:
     name: nginx-pod
     annotations:
       lab: lesson11
   ```

3. Press `Esc`, type `:wq`, and press Enter. Verify the saved annotation:

   ```bash
   cat k8s-sample.yaml
   ```

   Save the result as `task10_verify_annotation.png`.

4. Reopen the file, enter insert mode, and add this temporary line:

   ```yaml
   # temp: do-not-keep
   ```

   Save the editor showing the temporary line as `task10_verify_entering_temp_data.png`.

5. Press `Esc`, type `:q!`, and press Enter to discard the edit. Verify that the line is absent:

   ```bash
   cat k8s-sample.yaml
   ```

   Save the result as `task10_verify_no_temp_comment.png`.

📸 Screenshot Required:

- `task10_verify_annotation.png`
- `task10_verify_entering_temp_data.png`
- `task10_verify_no_temp_comment.png`

---

## Task 11: Practice Vim Editing and Navigation

Practice line deletion, undo, and cursor navigation without leaving unwanted changes in the manifest.

### Steps

1. Open the file:

   ```bash
   cd ~/Lab5
   vim k8s-sample.yaml
   ```

2. In normal mode, move to the `image: nginx:1.19` line, press `dd` to delete it, and then press `u` to undo. Save the restored file view as `task11_dd_delete_and_undo.png`.

3. On the same line, press `3dd` to delete three lines and then press `u` to undo. Save the restored file view as `task11_delete3_and_undo.png`.

4. Press `1G` to move to the first line. Save the editor view as `task11_line1.png`.

5. Press `G` to move to the last line. Move to the `containerPort: 80` line, press `$` to move to the end, and press `0` to move back to the beginning. Save the editor view as `task11_navigation.png`.

6. Exit with `:q!` so that no accidental changes are saved.

📸 Screenshot Required:

- `task11_dd_delete_and_undo.png`
- `task11_delete3_and_undo.png`
- `task11_line1.png`
- `task11_navigation.png`

---

## Task 12: Practice Vim Search and Substitution

Search for text, move between matches, add more matches, substitute all matches, and undo the substitution.

### Steps

1. Open the file:

   ```bash
   cd ~/Lab5
   vim k8s-sample.yaml
   ```

2. In normal mode, type `/nginx` and press Enter. Save the highlighted first match as `task12_search_nginx.png`.

3. Press `n` to move to the next match and `N` to return to the previous match. Save the editor view as `task12_n_and_N_navigation.png`.

4. Enter insert mode and add two YAML comment lines that each contain `nginx`. Press `Esc` and type `:w` to save them. Save the editor view as `task12_added_occurrences.png`.

5. Search for `nginx` again and press `n` repeatedly to cycle through the matches. Save a later match as `task12_cycle_matches.png`.

6. Replace every occurrence of `nginx` with `webapp`:

   ```text
   :%s/nginx/webapp/g
   ```

   Save the changed buffer as `task12_substitute_result.png`.

7. Press `u` once to undo the substitution. Save the restored `nginx` text as `task12_undo_and_quit.png`.

8. Type `:wq` and press Enter so the restored manifest remains saved.

📸 Screenshot Required:

- `task12_search_nginx.png`
- `task12_n_and_N_navigation.png`
- `task12_added_occurrences.png`
- `task12_cycle_matches.png`
- `task12_substitute_result.png`
- `task12_undo_and_quit.png`

---

## Exam Evaluation Question

### Docker Desktop Compatibility and Installation

Docker Desktop for Linux requires KVM hardware virtualization, and Docker does not support Docker Desktop for Linux in nested-virtualization environments. Because this Ubuntu Server runs inside VMware Workstation, compatibility depends on the host and VM configuration.

Use the [official Docker Desktop for Linux system requirements](https://docs.docker.com/desktop/setup/install/linux/) as the primary source for your evaluation.

Without using commands or step-by-step guidance from this README:

1. Research the current official Docker Desktop for Linux requirements.
2. Evaluate whether the Ubuntu VM satisfies the CPU, KVM, operating-system, memory, desktop, and nested-virtualization requirements.
3. Save clear terminal evidence of the compatibility evaluation as `exam_docker_environment_check.png`.
4. If the environment is supported, install and launch Docker Desktop. If it is unsupported, document the exact unmet requirement in `Lab5_Solution.pdf` instead of fabricating a successful installation.
5. Save the final outcome as `exam_docker_desktop_result.png`. This screenshot must show either:
   - Docker Desktop running successfully; or
   - clear diagnostic evidence of the unsupported requirement.

Your marks are based on a correct compatibility evaluation and honest evidence, not on forcing Docker Desktop to run in an unsupported nested VM.

📸 Screenshot Required:

- `exam_docker_environment_check.png`
- `exam_docker_desktop_result.png`

---

## Troubleshooting and Notes

- If an APT or PPA command fails, read the complete message and confirm the Ubuntu release, network connection, DNS, repository URL, and signing-key path.
- If an explicitly required third-party repository does not support your Ubuntu release, capture the unedited error and explain it in `Lab5_Solution.pdf`.
- When using Vim, remember: `i` enters insert mode, `Esc` returns to normal mode, `:wq` saves and quits, `:q!` discards unsaved changes, `u` undoes a change, and `Ctrl-R` redoes it.
- Keep the VM's default target set to `multi-user.target` after Task 6 unless your instructor directs otherwise.

## Summary

In this lab, you:

- installed and removed Java with `apt` and `apt-get`;
- compared package-index updates with package upgrades;
- installed and verified Visual Studio Code through Snap;
- configured XFCE, XRDP, and LightDM;
- tested graphical and command-line boot targets;
- configured a signed Google Chrome APT source;
- installed applications from PPAs;
- created and edited a Kubernetes manifest with Vim; and
- evaluated Docker Desktop compatibility for a virtualized Linux environment.

---

## Submission

1. Prepare a solution document containing:
   - a title page with your complete name, registration number, course code, section, and GitHub username;
   - headings for Tasks 1 through 12;
   - the exam evaluation question and its evidence;
   - screenshots in the same order used in this README; and
   - a short caption below every screenshot.

2. Export the solution document using this exact filename:

   ```text
   Lab5_Solution.pdf
   ```

3. Place the PDF at:

   ```text
   Labs/Lab05/Lab5_Solution.pdf
   ```

4. From the root of the local `CC` repository, copy the final Kubernetes manifest into the required workspace directory:

   ```bash
   mkdir -p Labs/Lab05/workspace
   cp ~/Lab5/k8s-sample.yaml Labs/Lab05/workspace/k8s-sample.yaml
   ```

5. Keep all original screenshots inside `Labs/Lab05/screenshots/`.

6. From the root of the `CC` repository, commit and push the completed lab:

   ```bash
   git status
   git add Labs/Lab05
   git commit -m "Submit Lab 05 evidence"
   git push origin main
   ```

7. Open the `CC` repository on GitHub and confirm that the complete `Labs/Lab05` directory is visible.

---

## Final Submission Checklist

- [ ] `README.md` remains inside `Labs/Lab05/`.
- [ ] `Lab5_Solution.pdf` is inside `Labs/Lab05/` and opens correctly.
- [ ] Every required screenshot is inside `Labs/Lab05/screenshots/`.
- [ ] Every screenshot uses the exact filename specified in this README.
- [ ] Every terminal screenshot shows the correct active prompt, required command, and relevant output.
- [ ] For each explicit GUI-or-version alternative in Task 8, I submitted one accepted screenshot.
- [ ] `task9_vim_install.png` is included only if Vim installation was necessary.
- [ ] `workspace/k8s-sample.yaml` contains valid YAML, the required annotation, and the restored `nginx` values.
- [ ] No password, private key, token, recovery code, or other credential is visible.
- [ ] All Tasks 1 through 12 and the exam evaluation question are included in `Lab5_Solution.pdf`.
- [ ] My submission is stored in the GitHub repository named exactly `CC`.
- [ ] I committed and pushed the complete `Labs/Lab05` directory.
