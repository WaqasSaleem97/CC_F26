# 🧰 Project 6 – Linux Fleet Patch and Configuration Compliance System

**Due Date:** To be announced  
**Total Marks:** 100  
**Submission Format:** GitHub Repository + PDF Report  
**Course Coverage:** Labs 04–07 and 11–15

---

## 📋 Project Overview

Build an Ansible-managed Linux fleet with controlled patching, account and permission baselines, SSH/UFW controls, service verification and compliance reporting. Terraform provides the EC2 fleet; Ansible applies changes serially and remediates intentional drift.

### Scope

- Four Ubuntu EC2 instances divided into web and utility groups
- Course-scope controls: users/groups, file modes, SSH key login, UFW, packages and Nginx
- Serial patching with health checks between hosts
- No Systems Manager, commercial scanner, CIS automation framework or external SIEM

### Mandatory Rules

- Use only the AWS services and course tools explicitly listed in this specification.
- Create infrastructure through Terraform; do not substitute manual AWS Console creation for graded resources.
- Use Ansible and/or Bash for configuration tasks as specified; repeated runs must be safe.
- Never commit AWS credentials, private keys, passwords, Terraform state, real tfvars, or generated secret files.
- Capture evidence before destroying resources and remove chargeable resources immediately after verification.

---

## 🎯 Learning Objectives

- Apply Linux account, permission and firewall skills across multiple hosts
- Organize Ansible inventories, roles, variables and handlers
- Perform safe rolling package updates
- Detect and remediate configuration drift
- Create human-readable patch and compliance evidence

---

## 🧱 Architecture Overview

```text
Controller (Ansible)
   | dynamic inventory by Role tag
   +--> web-1 -> web-2 (serial patch + HTTP check)
   +--> util-1 -> util-2 (serial patch + service check)
```

Create your own architecture diagram in the report. The diagram must show network boundaries, traffic direction, security controls and the Terraform/Ansible/Bash responsibility split.

---

## 📝 Requirements

### Part 1: Git Repository, Planning and Security Hygiene (15 marks)

#### 1.1 Repository Structure (5 marks)

Use this minimum structure and extend it where required:

```text
Project6/
├── README.md
├── .gitignore
├── terraform/modules/network
├── terraform/modules/fleet
├── ansible/group_vars
├── ansible/roles/baseline
├── ansible/roles/patching
├── ansible/roles/web
├── ansible/roles/audit
├── ansible/playbooks/site.yml
├── ansible/playbooks/patch.yml
├── ansible/playbooks/audit.yml
├── reports/
├── screenshots/
├── docs/
└── report/
```

**Tasks:**

- [ ] Create the documented folder structure.
- [ ] Explain each major directory in README.md.
- [ ] Keep generated files separate from source code.

**Required evidence:**

- `project6_part1_project_structure.png` — readable `tree` output

#### 1.2 Git Workflow (5 marks)

**Tasks:**

- [ ] Create at least two feature branches.
- [ ] Merge changes through pull requests with meaningful descriptions.
- [ ] Preserve readable commits and add a final release tag named `project6-v1.0`.

**Required evidence:**

- `project6_part1_git_history.png` — branch/commit graph and release tag
- `project6_part1_pull_request.png` — merged pull request

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

- `project6_part1_gitignore.png` — ignore rules and a clean status/secret check

---

### Part 2: Terraform Infrastructure (30 marks)

#### 2.1 Fleet network and groups (10 marks)

**Tasks:**

- [ ] Create network resources and four EC2 instances from a role map.
- [ ] Apply Role, PatchGroup, Environment and Owner tags.
- [ ] Use least-privilege security-group rules.

**Required evidence:**

- `project6_part2_section1_code.png` — relevant Terraform code
- `project6_part2_section1_result.png` — validation, plan, apply, output or AWS verification

#### 2.2 Compute module and outputs (10 marks)

**Tasks:**

- [ ] Use a module/for_each to avoid duplicate EC2 definitions.
- [ ] Output instance IDs and IPs grouped by role.
- [ ] Keep private key material and tfvars outside Git.

**Required evidence:**

- `project6_part2_section2_code.png` — relevant Terraform code
- `project6_part2_section2_result.png` — validation, plan, apply, output or AWS verification

#### 2.3 Validation and state inspection (10 marks)

**Tasks:**

- [ ] Validate allowed roles and fleet size.
- [ ] Run Terraform fmt, validate, plan and apply.
- [ ] Compare Terraform state with Ansible dynamic inventory.

**Required evidence:**

- `project6_part2_section3_code.png` — relevant Terraform code
- `project6_part2_section3_result.png` — validation, plan, apply, output or AWS verification

---

### Part 3: Baseline, Rolling Patch and Audit Roles (25 marks)

#### 3.1 Baseline role (8 marks)

**Tasks:**

- [ ] Create a deployer user/group, install its authorized key and enforce documented file ownership/modes.
- [ ] Configure UFW and course-scope SSH settings without locking out the administrator.
- [ ] Use handlers and back up SSH configuration before changes.

**Required evidence:**

- `project6_part3_section1_code.png` — playbook, role, template or script
- `project6_part3_section1_run.png` — successful execution and relevant result

#### 3.2 Rolling patch and service roles (12 marks)

**Tasks:**

- [ ] Update apt metadata and install available updates with serial: 1.
- [ ] For web hosts, drain by removing a health marker, patch, reboot only if required, wait for SSH and restore health.
- [ ] For utility hosts, verify a chosen service/process after patching.
- [ ] Capture changed packages and per-host success in a report.

**Required evidence:**

- `project6_part3_section2_code.png` — playbook, role, template or script
- `project6_part3_section2_run.png` — successful execution and relevant result

#### 3.3 Audit and remediation (5 marks)

**Tasks:**

- [ ] Audit required user/group, permissions, UFW rules, SSH settings, packages and services.
- [ ] Use check mode where appropriate and write a readable compliance summary.
- [ ] Run remediation and prove a final audit passes.

**Required evidence:**

- `project6_part3_section3_code.png` — playbook, role, template or script
- `project6_part3_section3_run.png` — successful execution and relevant result

---

### Part 4: Rolling Patch and Drift Drill (20 marks)

#### 4.1 Rolling availability test (10 marks)

**Tasks:**

- [ ] Run the patch playbook and show hosts processed one at a time.
- [ ] Prove at least one web endpoint remains available during the sequence.
- [ ] Show the final patch report and healthy services.

**Required evidence:**

- `project6_part4_section1_before.png` — initial state or test setup
- `project6_part4_section1_after.png` — verified result, remediation or recovery

#### 4.2 Controlled drift test (10 marks)

**Tasks:**

- [ ] Change one safe file mode or stop Nginx on one host.
- [ ] Run audit to detect the exact host and rule failure.
- [ ] Run remediation and verify the second audit passes with idempotence.

**Required evidence:**

- `project6_part4_section2_before.png` — initial state or test setup
- `project6_part4_section2_after.png` — verified result, remediation or recovery

---

### Part 5: Documentation, Demonstration and Cleanup (10 marks)

#### 5.1 Documentation and Demonstration (5 marks)

README.md and the PDF report must include:

1. Project overview and architecture diagram
2. Prerequisites and exact deployment sequence
3. Terraform variables, modules, outputs and state strategy
4. Ansible inventory/roles and Bash automation
5. Verification results and screenshots in task order
6. Security decisions and known limitations
7. Troubleshooting and rollback/recovery instructions
8. Cost-control and cleanup instructions

**Required evidence:**

- `project6_part5_readme_report.png` — rendered README and report contents
- `project6_part5_final_demo.png` — final working system

#### 5.2 Cleanup (5 marks)

**Tasks:**

- [ ] Capture all evidence before cleanup.
- [ ] Remove application data that blocks Terraform destroy only through the documented safe method.
- [ ] Run `terraform destroy` for every environment/workspace.
- [ ] Verify that project EC2 instances, volumes, security groups, S3 objects/buckets and other chargeable resources are removed.
- [ ] Keep only source code, sanitized reports and screenshots in GitHub.

**Required evidence:**

- `project6_part5_terraform_destroy.png` — successful destroy
- `project6_part5_aws_cleanup.png` — AWS CLI verification showing no project resources remain

---

## 📦 Submission Guidelines

### GitHub Repository

Use your existing public GitHub repository named `CC`. Store your assigned project in:

```text
CC/Projects/Project-6-fleet-patch-compliance/
```

Submit the direct GitHub URL of this exact project directory. The repository must contain source files, example variables, documentation, screenshots and the final report. It must not contain credentials, private keys, state files or real tfvars.

### PDF Report

Name the report:

```text
Project6_<YourName>_<YourRollNumber>.pdf
```

The report must contain a cover page, table of contents, architecture, implementation, testing, security analysis, challenges, cleanup evidence and conclusion.

---

## 📊 Grading Rubric

| Component | Marks |
|---|---:|
| Part 1: Git Repository, Planning and Security Hygiene | 15 |
| Part 2: Terraform Infrastructure | 30 |
| Part 3: Baseline, Rolling Patch and Audit Roles | 25 |
| Part 4: Rolling Patch and Drift Drill | 20 |
| Part 5: Documentation, Demonstration and Cleanup | 10 |
| **Total** | **100** |

Work that cannot be reproduced, exposes credentials, uses prohibited services, or lacks cleanup evidence may lose marks in the relevant sections.

---

## 💡 Tips for Success

- Start with the smallest working Terraform configuration, then add modules.
- Run `terraform fmt -recursive`, `terraform validate` and review `terraform plan` before every apply.
- Use tags consistently because they connect Terraform, AWS CLI and Ansible.
- Test Ansible twice and investigate any unexpected change on the second run.
- Build failure/recovery tests early rather than at the end.
- Destroy test resources as soon as the evidence is complete.

---

## 🐞 Common Issues and Checks

- **Dynamic inventory is empty:** verify AWS credentials, region and tag filters.
- **Ansible cannot connect:** verify key file permissions, username, public IP and security-group source CIDR.
- **Terraform wants unexpected replacement:** inspect the plan and state before applying.
- **Nginx reload fails:** run `nginx -t` and fix the rendered template first.
- **AWS CLI returns no targets:** run the same command without `--query` and confirm tags/region.
- **Destroy fails:** identify dependencies, remove only project-owned data and rerun destroy.

---

## 📚 Official References

- [GitHub Docs: Pull requests](https://docs.github.com/en/pull-requests)
- [Terraform: Modules](https://developer.hashicorp.com/terraform/language/modules)
- [Terraform: State](https://developer.hashicorp.com/terraform/language/state)
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
- [ ] No secrets, keys, state or real tfvars are committed.
- [ ] Terraform format, validation, plan and apply succeed.
- [ ] Required Terraform modules, variables, outputs and tags are present.
- [ ] Ansible inventory, roles/playbooks and scripts work repeatedly.
- [ ] Functional and failure/recovery tests are documented.
- [ ] All exact screenshot filenames are present under screenshots/.
- [ ] PDF report is complete and matches repository evidence.
- [ ] Every project-owned AWS resource has been destroyed and verified.
