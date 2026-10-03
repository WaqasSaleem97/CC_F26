# Assignment 01: Git, Gitea, GitHub, Git LFS, and GitHub Pages

**Deadline:** 10 October 2026  
**Total marks:** 10  
**Submission repository:** `CC`  
**Submission directory:** `Assignments/Assignment01`  
**Screenshot directory:** `Assignments/Assignment01/screenshots`  
**Required assignment files:** 3  
**Required screenshots:** 12

Complete all practical work using the environment specified for each task. Submit the required evidence to your existing public GitHub repository named exactly `CC` using the directory structure shown below.

```text
CC/
└── Assignments/
    └── Assignment01/
        ├── Assignment01.md
        ├── Assignment01_Solution.docx
        ├── Assignment01_Solution.pdf
        └── screenshots/
            ├── task1_gitea_running.png
            ├── task1_gitea_push.png
            ├── task1_gitea_repository.png
            ├── task2_remotes.png
            ├── task2_github_push.png
            ├── task2_github_repository.png
            ├── task3_lfs_setup.png
            ├── task3_lfs_files.png
            ├── task3_lfs_push.png
            ├── task4_pages_repository.png
            ├── task4_pages_deployment.png
            └── task4_portfolio_live.png
```

---

## Assignment Objective

In this assignment, you will run a Gitea service in GitHub Codespaces, use Git from your Ubuntu server, push the same repository to Gitea and GitHub, manage large files with Git LFS, and publish a portfolio or CV with GitHub Pages.

By the end of this assignment, you will be able to:

- Run and access a Gitea service in GitHub Codespaces.
- Create and manage a Git repository from an Ubuntu server.
- Work with multiple Git remotes.
- Authenticate safely without exposing credentials.
- Track and push large files with Git LFS.
- Publish a static portfolio or CV with GitHub Pages.

## Required Working Environments

Use the following environment for each type of work:

| Work | Required environment |
| --- | --- |
| Run the Gitea service | GitHub Codespaces |
| Perform Git terminal work for Tasks 1, 2, and 3 | Ubuntu server |
| Use Gitea and GitHub web pages | Web browser |
| Store assignment evidence | Existing GitHub repository named `CC` |

Do not create or push the Task 1 repository from the Codespaces terminal. GitHub Codespaces is used only to run Gitea. All Git commands for Tasks 1, 2, and 3 must be completed on your Ubuntu server.

Your Ubuntu terminal prompt must show your registered GitHub username and the hostname `ubuntu`:

```text
<github-username>@ubuntu:~$
```

The username comparison is case-insensitive. For example, `StudentName` on GitHub and `studentname` on Ubuntu are treated as the same username when the spelling is otherwise identical.

## Task List

- [Getting Started](#getting-started)
- [Task 1: Run Gitea in Codespaces and Push from the Ubuntu Server](#task-1-run-gitea-in-codespaces-and-push-from-the-ubuntu-server)
- [Task 2: Push the Same Repository to GitHub](#task-2-push-the-same-repository-to-github)
- [Task 3: Track Three Large Files with Git LFS](#task-3-track-three-large-files-with-git-lfs)
- [Task 4: Create a Portfolio or CV with GitHub Pages](#task-4-create-a-portfolio-or-cv-with-github-pages)
- [Assignment Summary](#assignment-summary)
- [Final Submission Checklist](#final-submission-checklist)

---

## Getting Started

1. Inside your existing `CC` repository, create the assignment screenshot directory:

   ```bash
   mkdir -p Assignments/Assignment01/screenshots
   ```

2. Confirm that your Ubuntu server terminal prompt follows this pattern:

   ```text
   <github-username>@ubuntu:~$
   ```

3. Confirm that Git and Git LFS are installed on the Ubuntu server.

4. Read all four tasks before beginning the assignment.

5. Create the following two solution files inside `Assignments/Assignment01/` and add your evidence to them as you complete each task:

   ```text
   Assignment01_Solution.docx
   Assignment01_Solution.pdf
   ```

---

## Task 1: Run Gitea in Codespaces and Push from the Ubuntu Server

### Step 1.1: Run Gitea in GitHub Codespaces

1. Run a Gitea server inside your GitHub Codespace.
2. Forward port `3000`.
3. Temporarily change the port visibility to **Public** so that your Ubuntu server can reach the Gitea HTTPS URL.
4. Keep the Codespace and Gitea service running while completing Task 1.
5. Open Gitea in the browser and confirm that the public URL uses this format:

   ```text
   https://<codespace-name>-3000.app.github.dev
   ```

The browser must show the Gitea interface and either the forwarded Codespaces URL or port `3000`. A normal Gitea page such as **Sign In**, **Register**, **Dashboard**, **Explore**, or **Repositories** must be visible.

📸 **Screenshot required immediately after this step:** Save it as `task1_gitea_running.png`.

### Step 1.2: Create and Push the Initial Repository

1. In Gitea, create an **empty public repository** named exactly `Assignment01`.
2. Do not initialize it with a README, `.gitignore`, or license.
3. Connect to your Ubuntu server and create a separate local repository outside the existing `CC` repository:

   ```bash
   cd ~
   mkdir Assignment01
   cd Assignment01
   git init
   git branch -M main
   ```

4. Create a `README.md` containing:
   - your full name; and
   - your registration number.
5. Commit the README:

   ```bash
   git add README.md
   git commit -m "Add student information"
   ```

6. In Gitea, generate a personal access token:
   - Open your profile menu and select **Settings**.
   - Open **Applications** and then **Manage Access Tokens**.
   - Use `ubuntu-server-token` as the token name.
   - Give the token read-and-write repository permission (`write:repository`).
   - Copy the token when it is displayed. Do not place it in a screenshot.
7. On the Ubuntu server, add the public Codespaces Gitea repository URL as the `gitea` remote. Do not include your username, password, or token in the URL:

   ```bash
   git remote add gitea https://<codespace-name>-3000.app.github.dev/<gitea-username>/Assignment01.git
   ```

8. Push the initial commit:

   ```bash
   git push -u gitea main
   ```

9. If Git requests authentication, enter your Gitea username and use the token as the password. Password and token input is hidden in the terminal.

The terminal must show the `git push` command, the `gitea` remote, a successful result, and your `<github-username>@ubuntu` prompt. The token must not be visible.

📸 **Screenshot required immediately after this step:** Save it as `task1_gitea_push.png`.

### Step 1.3: Verify the Gitea Repository

1. Open the `Assignment01` repository in Gitea.
2. Confirm that the repository name is visible.
3. Confirm that the rendered `README.md` shows your full name and registration number.

📸 **Screenshot required immediately after this step:** Save it as `task1_gitea_repository.png`.

After capturing the screenshot, change port `3000` back to **Private** or stop the Codespace. Do not leave the forwarded port public after completing Task 1.

**Task 1 result:** You ran Gitea in Codespaces, created a repository locally on the Ubuntu server, and pushed the repository to Gitea through HTTPS.

---

## Task 2: Push the Same Repository to GitHub

### Step 2.1: Create the GitHub Repository and Verify Both Remotes

1. Create an **empty public GitHub repository** named exactly `Assignment01`.
2. Do not initialize it with a README, `.gitignore`, or license.
3. On the Ubuntu server, open the same local `Assignment01` repository used in Task 1.
4. Add GitHub as the second remote:

   ```bash
   cd ~/Assignment01
   git remote add github https://github.com/<your-github-username>/Assignment01.git
   ```

5. Verify both remotes:

   ```bash
   git remote -v
   ```

The output must show both `gitea` and `github`, including their fetch and push URLs. No token or password may appear in either URL.

📸 **Screenshot required immediately after this step:** Save it as `task2_remotes.png`.

### Step 2.2: Push to GitHub

Push the `main` branch to the GitHub remote:

```bash
git push -u github main
```

The terminal must show the command, the GitHub remote, a successful result, and your `<github-username>@ubuntu` prompt.

📸 **Screenshot required immediately after this step:** Save it as `task2_github_push.png`.

### Step 2.3: Verify the GitHub Repository

Open the following repository page:

```text
https://github.com/<your-github-username>/Assignment01
```

Verify that:

- the repository owner is your GitHub account;
- the repository name is `Assignment01`;
- the repository is public; and
- the rendered `README.md` shows your full name and registration number.

Keep the repository owner, repository name, and rendered README visible.

📸 **Screenshot required immediately after this step:** Save it as `task2_github_repository.png`.

**Task 2 result:** The same local repository is connected to both Gitea and GitHub and is available publicly on GitHub.

---

## Task 3: Track Three Large Files with Git LFS

### Step 3.1: Configure Git LFS

On the Ubuntu server, open the local `Assignment01` repository, initialize Git LFS, confirm its version, and configure tracking for `.bin` files:

```bash
cd ~/Assignment01
git lfs install
git lfs version
git lfs track "*.bin"
cat .gitattributes
```

The terminal must show:

- successful Git LFS initialization;
- the Git LFS version;
- the `git lfs track "*.bin"` command; and
- the resulting `*.bin` rule in `.gitattributes`.

📸 **Screenshot required immediately after this step:** Save it as `task3_lfs_setup.png`.

### Step 3.2: Create and Verify Three LFS Files

1. Create three different files, each larger than **100 MiB**. For example:

   ```bash
   truncate -s 101M large-file-1.bin
   truncate -s 102M large-file-2.bin
   truncate -s 103M large-file-3.bin
   ```

2. Stage `.gitattributes` and the three files:

   ```bash
   git add .gitattributes large-file-1.bin large-file-2.bin large-file-3.bin
   ```

3. Verify that all three files are tracked by Git LFS and display their sizes:

   ```bash
   git lfs ls-files --size
   ```

The output must clearly list all three files, and every file must be larger than 100 MiB.

📸 **Screenshot required immediately after this step:** Save it as `task3_lfs_files.png`.

### Step 3.3: Commit and Push the LFS Files

Commit and push the files to the GitHub `Assignment01` repository:

```bash
git commit -m "Add three files using Git LFS"
git push github main
```

Capture the first successful upload. The terminal should show the LFS object upload completing successfully, preferably `100% (3/3)`.

📸 **Screenshot required immediately after this step:** Save it as `task3_lfs_push.png`.

**Task 3 result:** Three files larger than 100 MiB are tracked with Git LFS and pushed to the GitHub `Assignment01` repository.

---

## Task 4: Create a Portfolio or CV with GitHub Pages

### Step 4.1: Create and Push the Portfolio Repository

1. Create a new **public** GitHub repository named:

   ```text
   <your-github-username>.github.io
   ```

2. Create a portfolio or CV using HTML and CSS.
3. Include at least these files:
   - `index.html`
   - `styles.css`
4. The portfolio must visibly contain these sections:
   - About Me
   - Education
   - Skills
   - Projects
5. Commit and push the source files to the `main` branch.
6. Open the GitHub repository page. Keep the repository owner, repository name, `index.html`, and `styles.css` visible.

📸 **Screenshot required immediately after this step:** Save it as `task4_pages_repository.png`.

### Step 4.2: Publish with GitHub Pages

1. Open the portfolio repository on GitHub.
2. Go to **Settings → Pages**.
3. Under **Build and deployment**, select **Deploy from a branch**.
4. Select the `main` branch and the `/ (root)` folder, then save.
5. Wait until GitHub reports that the site is published or live.

The page must show **GitHub Pages**, a successful published or live status, and your `<github-username>.github.io` URL.

📸 **Screenshot required immediately after this step:** Save it as `task4_pages_deployment.png`.

### Step 4.3: Verify the Live Portfolio

Open the following URL in your browser:

```text
https://<your-github-username>.github.io/
```

Confirm that the site loads successfully. Keep the complete `github.io` URL and the portfolio content visible, including at least one required section such as **About Me**, **Education**, **Skills**, or **Projects**.

📸 **Screenshot required immediately after this step:** Save it as `task4_portfolio_live.png`.

**Task 4 result:** Your portfolio or CV is stored in a public GitHub repository and published as a live GitHub Pages site.

---

## Assignment Summary

In this assignment, you:

- ran a Gitea service in GitHub Codespaces;
- performed Git work from an Ubuntu server with your GitHub identity visible;
- pushed the same local repository to Gitea and GitHub using two remotes;
- configured Git LFS and pushed three large files; and
- created and published a portfolio or CV with GitHub Pages;
- prepared a complete Word solution; and
- exported the completed Word solution as a readable PDF.

---

## Final Submission Checklist

Before submission, confirm that:

- [ ] I completed the assignment individually.
- [ ] `Assignment01.md` is inside `CC/Assignments/Assignment01/`.
- [ ] `Assignment01_Solution.docx` is inside `CC/Assignments/Assignment01/` and opens correctly.
- [ ] `Assignment01_Solution.pdf` is inside `CC/Assignments/Assignment01/`, matches the Word solution, and opens correctly.
- [ ] My `CC` repository contains `Assignments/Assignment01/screenshots/`.
- [ ] All 12 required screenshots are present and readable.
- [ ] Every screenshot was captured immediately after the requested step.
- [ ] Every screenshot was created from my own work.
- [ ] My GitHub username is visible in every Ubuntu server terminal screenshot.
- [ ] Every terminal screenshot shows the required command and its relevant result.
- [ ] Every browser screenshot shows the required repository, URL, page, or status.
- [ ] No screenshot contains a token, password, private key, or other credential.
- [ ] My `CC`, GitHub `Assignment01`, and `<username>.github.io` repositories are public.
- [ ] My Gitea `Assignment01` repository was created and pushed from the Ubuntu server.
- [ ] All repositories use the `main` branch.
- [ ] I pushed the complete `Assignments/Assignment01` directory before the deadline.
- [ ] I uploaded all three required assignment files to Google Classroom.
- [ ] I submitted the three required links on Google Classroom.

## Academic Integrity

Marks are awarded for completing and demonstrating the required process, not only for showing a final result. You may be asked to explain or demonstrate any submitted step. Copied work, copied screenshots, or evidence that does not belong to the submitting student will not receive credit.

## Official Documentation

- [Creating a GitHub repository](https://docs.github.com/en/repositories/creating-and-managing-repositories/creating-a-new-repository)
- [About large files on GitHub](https://docs.github.com/en/repositories/working-with-files/managing-large-files/about-large-files-on-github)
- [Git Large File Storage](https://docs.github.com/en/repositories/working-with-files/managing-large-files/about-git-large-file-storage)
- [Creating a GitHub Pages site](https://docs.github.com/en/pages/getting-started-with-github-pages/creating-a-github-pages-site)
- [Forwarding and sharing ports in GitHub Codespaces](https://docs.github.com/en/codespaces/developing-in-a-codespace/forwarding-ports-in-your-codespace)
- [Gitea API tokens and repository permissions](https://docs.gitea.com/1.26/development/api-usage)
