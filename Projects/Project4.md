# 💰 Project 4 – Scheduled EC2 Cost-Control and Resource Lifecycle Manager

**Due Date:** To be announced  
**Total Marks:** 100  
**Submission Format:** GitHub Repository + PDF Report  
**Course Coverage:** Labs 06–07 and 09–13

---

## 📋 Project Overview

Create a tag-controlled EC2 lifecycle manager and a near-real-time Nginx web dashboard. Terraform provisions the disposable development, testing and demonstration environments, AWS CLI and Bash perform safe lifecycle checks, cron evaluates schedules, and the browser continuously displays the current state, next action and remaining time.

The dashboard uses a generated `status.json` file as its data source. It polls for updated AWS state every 30 seconds, while JavaScript updates visible countdowns every second without requiring the user to reload the page.

### Problem to Solve

**Think of this as:** an automatic power switch combined with a live departure board for cloud environments.

Development and testing EC2 environments often remain powered on outside working hours and create unnecessary cost. Administrators may also be unable to see which environment is active, when its next start or stop will occur, how much time remains before that action, or which expired environment has been approved for destruction. Manual operations are repetitive, difficult to audit and can accidentally affect production resources.

### Scope

- Three non-production environments named `dev`, `test` and `demo`, managed through separate Terraform workspaces
- One EC2 instance per environment, plus a protected production mock/control entry used only to prove exclusion
- Controller runs on the course Ubuntu server or Codespace and hosts Bash automation plus the Nginx dashboard
- Required tags: `Project`, `Environment`, `Schedule`, `StartAt`, `StopAt`, `Timezone`, `ExpiresAt`, `AutoDestroy`, `DestroyApproved`, `Owner` and `ManagedBy`
- Browser polls lifecycle data every 30 seconds; the countdown display updates every second
- Start, stop and approved expiry-based destruction must use exact environment/workspace selection and safe preview modes
- No Lambda, EventBridge, Systems Manager, billing API, database, Node.js/Python web framework or third-party scheduler

### Expected Final Result

A responsive Nginx web dashboard that shows every environment in a table or card layout with its name, instance ID, current state, schedule, timezone, current time, next action, scheduled action time, live countdown, expiry time, destruction eligibility, last action result and last refresh time.

The lifecycle engine starts and stops approved instances according to schedule. An expired non-production environment is destroyed only when `AutoDestroy=true`, `DestroyApproved=true`, its `ExpiresAt` time has passed and the exact Terraform workspace has been selected. The dashboard updates automatically and retains a sanitized history entry so the user can see that the environment was destroyed.

### Mandatory Rules

- Use only the AWS services and course tools explicitly listed in this specification.
- Create and destroy graded infrastructure through Terraform; do not substitute manual AWS Console creation or `aws ec2 terminate-instances`.
- Production resources and environments with `AutoDestroy=false` or `DestroyApproved=false` must never be destroyed.
- Destruction must default to dry-run, verify the exact non-production workspace and create a logged destroy plan before applying it.
- Use one documented timezone consistently and store action timestamps in ISO 8601 format.
- Generate dashboard JSON atomically through a temporary file and rename so the browser never reads a partial file.
- Never expose AWS credentials, private keys, Terraform state or command secrets in HTML, JSON, logs, screenshots or Git.
- Capture evidence before final cleanup and remove chargeable resources immediately after verification.

---

## 🎯 Learning Objectives

- Use AWS CLI filters and queries to discover exact lifecycle targets safely
- Write robust Bash functions for schedules, ISO 8601 timestamps, timezones, countdown calculations and audit logging
- Model environment, schedule, expiry and destruction-safety tags in Terraform
- Isolate disposable environments through Terraform workspaces
- Build a static Nginx dashboard that reads JSON and refreshes state without a manual page reload
- Demonstrate scheduled start/stop operations and approved destruction without affecting production-tagged resources

---

## 🧱 Architecture Overview

![Project 4 architecture overview showing the real-time lifecycle dashboard and workflow](images/project4-architecture.png)

**Color guide:** Blue represents the operator, purple represents Terraform, orange represents Bash and AWS CLI automation, green represents the running dashboard and AWS environments, and gold represents live status data.

### Simple Project Flow

1. **Observe:** The Nginx dashboard polls `status.json`; visible countdowns update every second.
2. **Schedule:** Cron evaluates start, stop and expiry times every minute.
3. **Act:** AWS CLI starts or stops approved EC2 instances, while Terraform destroys only approved expired workspaces.
4. **Prove:** The web page and audit logs show current state, next action, time remaining, last result and refresh time.

> The picture is a conceptual guide. Your report must include your own architecture diagram using the actual environment names, workspace design, data-refresh flow and safety controls from your implementation.

Create your own architecture diagram in the report. It must show the browser, Nginx dashboard, `status.json`, lifecycle scripts, cron, Terraform workspaces, managed environments, action flow and production/destruction safeguards.

---

## 📝 Requirements

### Part 1: Git Repository, Planning and Security Hygiene (15 marks)

#### 1.1 Repository Structure (5 marks)

Use this minimum structure and extend it where required:

```text
Project4/
├── README.md
├── .gitignore
├── terraform/
│   ├── modules/network
│   ├── modules/instances
│   └── environments/
├── ansible/roles/web_marker
├── dashboard/
│   ├── index.html
│   ├── app.js
│   ├── styles.css
│   └── data/status.json.example
├── scripts/
│   ├── list-targets.sh
│   ├── start-scheduled.sh
│   ├── stop-scheduled.sh
│   ├── destroy-expired.sh
│   ├── generate-status.sh
│   └── lifecycle-report.sh
├── logs/
├── screenshots/
├── docs/
└── report/
```

**Tasks:**

- [ ] Create the documented folder structure.
- [ ] Explain each major directory in README.md.
- [ ] Keep generated files separate from source code.

**Required evidence:**

- `project4_part1_project_structure.png` — readable `tree` output

#### 1.2 Git Workflow (5 marks)

**Tasks:**

- [ ] Create at least two feature branches.
- [ ] Merge changes through pull requests with meaningful descriptions.
- [ ] Preserve readable commits and add a final release tag named `project4-v1.0`.

**Required evidence:**

- `project4_part1_git_history.png` — branch/commit graph and release tag
- `project4_part1_pull_request.png` — merged pull request

#### 1.3 .gitignore and Secret Protection (5 marks)

Your .gitignore must exclude at minimum:

```gitignore
.terraform/
*.tfstate
*.tfstate.*
terraform.tfvars
*.pem
*.key
*.crt
.aws/
.env
__pycache__/
```

Commit example files only, such as `terraform.tfvars.example`. Search staged files and Git history before submission to confirm no credential or private key was committed.

**Required evidence:**

- `project4_part1_gitignore.png` — ignore rules and a clean status/secret check

---

### Part 2: Terraform Infrastructure (30 marks)

#### 2.1 Tagged environments and workspaces (10 marks)

**Tasks:**

- [ ] Create a small VPC, subnet and least-privilege security group.
- [ ] Create separate `dev`, `test` and `demo` Terraform workspaces, with one schedule-managed EC2 instance in each.
- [ ] Apply all required environment, schedule, timezone, expiry, approval, owner and management tags from validated variables or maps.
- [ ] Add a documented protected production mock/control entry that can appear in tests but must never become a lifecycle target.

**Required evidence:**

- `project4_part2_section1_code.png` — relevant Terraform code
- `project4_part2_section1_result.png` — workspace list, validation, plan, apply, outputs or AWS verification

#### 2.2 Schedule, expiry and safety validation (10 marks)

**Tasks:**

- [ ] Validate allowed environment and schedule names.
- [ ] Validate `StartAt` and `StopAt` as documented 24-hour times and `ExpiresAt` as an ISO 8601 timestamp.
- [ ] Prevent `production`, an empty environment, `AutoDestroy=false` or `DestroyApproved=false` from satisfying destruction conditions.
- [ ] Output environment name, workspace, instance ID, schedule, timezone and expiry information without secrets.

**Required evidence:**

- `project4_part2_section2_code.png` — relevant Terraform code
- `project4_part2_section2_result.png` — successful and rejected validation examples

#### 2.3 State and live AWS comparison (10 marks)

**Tasks:**

- [ ] Run `terraform fmt -check`, `terraform validate`, `terraform workspace list`, plan and apply.
- [ ] Compare Terraform outputs with `aws ec2 describe-instances` filtered by project tags.
- [ ] Document the relationship among each workspace, Terraform state, live EC2 data and the dashboard environment record.
- [ ] Prove that every destroy preview identifies one exact non-production workspace and never relies on a broad AWS termination command.

**Required evidence:**

- `project4_part2_section3_code.png` — workspace/state and query implementation
- `project4_part2_section3_result.png` — Terraform and AWS CLI comparison

---

### Part 3: Lifecycle Automation and Real-Time Dashboard (25 marks)

#### 3.1 Nginx lifecycle web dashboard (8 marks)

Build a static HTML/CSS/JavaScript dashboard served by Nginx on the controller.

**Tasks:**

- [ ] Display the documented current time and timezone at the top of the page.
- [ ] Display each environment's name, instance ID, current state, schedule, start/stop times, next action, scheduled action time, live time remaining, `ExpiresAt`, destruction status, last action result and last refresh time.
- [ ] Use clear status labels such as `Running`, `Stopped`, `Pending Start`, `Pending Stop`, `Waiting for Expiry`, `Awaiting Approval`, `Scheduled for Destruction` and `Destroyed`.
- [ ] Fetch `data/status.json` every 30 seconds and update countdown values every second without reloading the page.
- [ ] Obtain displayed state from generated data; do not hard-code results into HTML.

**Required evidence:**

- `project4_part3_section1_code.png` — dashboard HTML, CSS, JavaScript and Nginx configuration
- `project4_part3_section1_run.png` — readable dashboard showing all required fields

#### 3.2 Safe start, stop and expiry scripts (12 marks)

**Tasks:**

- [ ] Implement `--list` and `--dry-run` modes before every real lifecycle action.
- [ ] Use exact project/environment tag filters and refuse an empty target list.
- [ ] Refuse all operations on `Environment=production`.
- [ ] Allow destruction only when the environment is non-production, `AutoDestroy=true`, `DestroyApproved=true` and the current time is at or after `ExpiresAt`.
- [ ] Select the exact Terraform workspace, create and log a destroy plan, then apply that plan; do not terminate Terraform-managed EC2 instances directly through AWS CLI.
- [ ] Prevent overlapping executions and log timestamp, caller, environment, workspace, action, target ID and result.
- [ ] Regenerate `status.json` immediately after every successful or failed action.

**Required evidence:**

- `project4_part3_section2_code.png` — lifecycle and destruction safety logic
- `project4_part3_section2_run.png` — list, dry-run, refusal and approved-action results

#### 3.3 Cron, live data and audit history (5 marks)

**Tasks:**

- [ ] Create documented cron entries that evaluate start, stop and expiry conditions every minute.
- [ ] Generate valid sanitized JSON through a temporary file and atomic rename.
- [ ] Preserve the most recent action and a sanitized destroyed-environment history entry even after its EC2 resource no longer exists.
- [ ] Configure Nginx to serve the dashboard and JSON with appropriate no-cache behavior.
- [ ] Keep logs free of credentials and document log rotation without adding an unapproved service.

**Required evidence:**

- `project4_part3_section3_code.png` — cron, JSON generation and Nginx configuration
- `project4_part3_section3_run.png` — automatic refresh, audit log and JSON validation

---

### Part 4: Dashboard and Destruction Safety Test (20 marks)

#### 4.1 Real-time schedule and state test (10 marks)

**Tasks:**

- [ ] Configure short test start and stop times for one non-production environment.
- [ ] Show the environment state, next action, scheduled time and countdown changing on the web page without a manual browser reload.
- [ ] Demonstrate the `Running` and `Stopped` states and confirm Nginx health after the instance returns to running.
- [ ] Confirm that the page shows the last action result and a recent refresh timestamp.
- [ ] Capture list, dry-run, web-dashboard and real-action evidence.

**Required evidence:**

- `project4_part4_section1_before.png` — dashboard before the scheduled action with visible countdown
- `project4_part4_section1_after.png` — automatically refreshed dashboard after the state change

#### 4.2 Expiry and destruction-protection test (10 marks)

**Tasks:**

- [ ] Give one disposable test environment a short `ExpiresAt` value, `AutoDestroy=true` and `DestroyApproved=true`.
- [ ] Show its dashboard status progressing from waiting for expiry to scheduled for destruction.
- [ ] Capture the destroy dry-run/plan, allow the approved workflow to destroy the exact workspace and show the retained `Destroyed` history result on the dashboard.
- [ ] Prove that the production control entry and an environment with `AutoDestroy=false` or `DestroyApproved=false` are excluded.
- [ ] Attempt a deliberately broad or incorrect workspace input and prove that the script refuses it.

**Required evidence:**

- `project4_part4_section2_before.png` — expiry countdown, eligibility and destroy preview
- `project4_part4_section2_after.png` — protected exclusions plus destroyed status/history

---

### Part 5: Documentation, Demonstration and Cleanup (10 marks)

#### 5.1 Documentation and Demonstration (5 marks)

README.md and the PDF report must include:

1. Project overview and architecture diagram
2. Prerequisites and exact deployment sequence
3. Terraform workspaces, variables, tags, outputs and state strategy
4. Dashboard fields, `status.json` schema, refresh interval and browser countdown logic
5. Timezone choice, schedule calculation and ISO 8601 expiry format
6. Start, stop, approval and safe-destruction workflow
7. Verification results and screenshots in task order
8. Security decisions, known limitations and production safeguards
9. Troubleshooting, rollback and recovery instructions
10. Cost-control and final cleanup instructions

**Required evidence:**

- `project4_part5_readme_report.png` — rendered README and report contents
- `project4_part5_final_demo.png` — final dashboard with active, stopped and destroyed/history states

#### 5.2 Cleanup (5 marks)

**Tasks:**

- [ ] Capture all evidence before cleanup.
- [ ] Stop cron while performing final cleanup so it cannot start or recreate activity.
- [ ] Run `terraform destroy` for every remaining environment/workspace.
- [ ] Verify that project EC2 instances, volumes, security groups and other chargeable resources are removed.
- [ ] Keep only source code, sanitized sample JSON, reports, logs and screenshots in GitHub.

**Required evidence:**

- `project4_part5_terraform_destroy.png` — successful destroy for all remaining workspaces
- `project4_part5_aws_cleanup.png` — AWS CLI verification showing no project resources remain

---

## 📦 Submission Guidelines

### GitHub Repository

Use your existing public GitHub repository named `CC`. Store your assigned project in:

```text
CC/Projects/Project-4-ec2-cost-control-manager/
```

Submit the direct GitHub URL of this exact project directory. The repository must contain source files, example variables, documentation, screenshots and the final report. It must not contain credentials, private keys, state files or real tfvars.

### PDF Report

Name the report:

```text
Project4_<YourName>_<YourRollNumber>.pdf
```

The report must contain a cover page, table of contents, architecture, implementation, testing, security analysis, challenges, cleanup evidence and conclusion.

---

## 📊 Grading Rubric

| Component | Marks |
|---|---:|
| Part 1: Git Repository, Planning and Security Hygiene | 15 |
| Part 2: Terraform Infrastructure | 30 |
| Part 3: Lifecycle Automation and Real-Time Dashboard | 25 |
| Part 4: Dashboard and Destruction Safety Test | 20 |
| Part 5: Documentation, Demonstration and Cleanup | 10 |
| **Total** | **100** |

Work that cannot be reproduced, exposes credentials, uses prohibited services, or lacks cleanup evidence may lose marks in the relevant sections.

---

## 💡 Tips for Success

- Use short schedules only for controlled testing; use realistic office hours in the final configuration.
- Convert every comparison to one documented timezone and keep stored timestamps in ISO 8601 format.
- Calculate countdowns from absolute timestamps instead of decrementing an unsynchronized number.
- Write `status.json` to a temporary file, validate it and rename it atomically.
- Keep destruction disabled until start/stop behavior, dashboard refresh and protection tests work correctly.
- Verify the selected Terraform workspace immediately before creating and applying a destroy plan.
- Stop cron before the final course cleanup and destroy every remaining workspace.

---

## 🐞 Common Issues and Checks

- **Dashboard data does not change:** verify `generate-status.sh`, cron, JSON permissions, Nginx path and browser network requests.
- **Countdown is incorrect:** compare browser time, controller time, configured timezone and the ISO 8601 action timestamp.
- **Browser shows old data:** disable caching for `status.json` and confirm the last-refresh value changes.
- **JSON sometimes fails to load:** write and validate a temporary file before atomically renaming it.
- **AWS CLI returns no targets:** verify AWS credentials, region and exact project/environment tags.
- **Environment is wrongly eligible for destruction:** require all safety conditions and fail closed when any value is missing.
- **Wrong Terraform workspace is selected:** stop immediately, reselect the expected workspace and rerun dry-run/plan.
- **Concurrent cron actions overlap:** use a lock and log the refused duplicate execution.
- **Nginx reload fails:** run `nginx -t` and correct the configuration before reloading.
- **Final destroy fails:** stop cron, inspect workspace state and dependencies, then rerun the documented Terraform cleanup.

---

## 📚 Official References

- [GitHub Docs: Pull requests](https://docs.github.com/en/pull-requests)
- [Terraform: Modules](https://developer.hashicorp.com/terraform/language/modules)
- [Terraform: State](https://developer.hashicorp.com/terraform/language/state)
- [Terraform: Workspaces](https://developer.hashicorp.com/terraform/cli/workspaces)
- [MDN Web Docs: Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API)
- [MDN Web Docs: setInterval](https://developer.mozilla.org/en-US/docs/Web/API/Window/setInterval)
- [Terraform AWS Provider](https://registry.terraform.io/providers/hashicorp/aws/latest/docs)
- [Ansible: Roles](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_reuse_roles.html)
- [Ansible: Amazon EC2 dynamic inventory](https://docs.ansible.com/ansible/latest/collections/amazon/aws/aws_ec2_inventory.html)
- [AWS CLI EC2 Reference](https://docs.aws.amazon.com/cli/latest/reference/ec2/)
- [AWS IAM User Guide](https://docs.aws.amazon.com/IAM/latest/UserGuide/)
- [Amazon S3 User Guide](https://docs.aws.amazon.com/AmazonS3/latest/userguide/)
- [NGINX Documentation](https://nginx.org/en/docs/)

---

## ❓ FAQ

**Can I use a service not listed in the project?**  
No. Request instructor approval before adding a service outside the stated scope.

**Can I configure graded resources manually in the AWS Console?**  
No. The final reproducible implementation must use Terraform and the specified automation. The Console may be used only to inspect results.

**May my team use AI-generated code?**  
Follow the course academic-integrity policy. Every team member must understand, explain and reproduce all submitted work. Cite external assistance where required.

**What if exact AWS values differ from the sample?**  
Resource IDs and IP addresses will differ. Your architecture, tags, security rules, behavior and evidence must satisfy the specification.

---

## 📜 Academic Integrity

Team size and project allocation will be announced by the instructor. Discussion of concepts is allowed, but copying another team's implementation, screenshots, report or Git history is not allowed. Every member must be able to explain the complete solution.

---

## ✅ Final Checklist

- [ ] Repository structure and README are complete.
- [ ] Feature branches, pull requests and release tag are visible.
- [ ] No secrets, keys, state, real tfvars or sensitive live data are committed.
- [ ] Terraform format, validation, workspace, plan and apply steps succeed.
- [ ] Required schedule, timezone, expiry and destruction-safety tags are present.
- [ ] Nginx serves the dashboard and sanitized `status.json`.
- [ ] Every required environment field, schedule, next action and countdown is visible.
- [ ] Browser data refreshes automatically and countdowns update every second.
- [ ] Start and stop actions work through exact filters and safe modes.
- [ ] Production, unapproved and non-disposable environments are protected.
- [ ] Approved expiry destruction uses the exact Terraform workspace and logged plan.
- [ ] Functional, real-time and destruction-safety tests are documented.
- [ ] All exact screenshot filenames are present under `screenshots/`.
- [ ] PDF report is complete and matches repository evidence.
- [ ] Cron is stopped and every project-owned AWS resource has been destroyed and verified.
