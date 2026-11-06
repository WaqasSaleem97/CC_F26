# 👥 Project 7 – Terraform IAM Joiner-Mover-Leaver Automation

**Due Date:** To be announced  
**Total Marks:** 100  
**Submission Format:** GitHub Repository + PDF Report  
**Course Coverage:** Labs 02–03, 09 and 13–14

---

## 📋 Project Overview

Implement a controlled IAM onboarding, role-change and offboarding workflow driven by a reviewed CSV file. Terraform creates groups, users, memberships and approved policy attachments. Bash validates requests, an S3 backend protects shared state, and Ansible publishes a sanitized audit dashboard.

### Scope

- Groups: CloudAdmins, Developers and Auditors with documented AWS-managed policies
- CSV-driven users using csvdecode; no plaintext password or access key may be committed
- Joiner, mover and leaver changes must occur through branches and pull requests
- No AWS Organizations, Identity Center, Lambda or custom web application

### Mandatory Rules

- Use only the AWS services and course tools explicitly listed in this specification.
- Create infrastructure through Terraform; do not substitute manual AWS Console creation for graded resources.
- Use Ansible and/or Bash for configuration tasks as specified; repeated runs must be safe.
- Never commit AWS credentials, private keys, passwords, Terraform state, real tfvars, or generated secret files.
- Capture evidence before destroying resources and remove chargeable resources immediately after verification.

---

## 🎯 Learning Objectives

- Create multiple IAM resources dynamically from reviewed data
- Protect and share Terraform state using a private S3 backend
- Use Git branches, pull requests and history for access-change evidence
- Validate input data and prevent dangerous combinations
- Publish a sanitized user/group audit view without credentials

---

## 🧱 Architecture Overview

```text
users.csv -> validation.sh -> Pull Request review
                         |
                    Terraform apply
                         |
       IAM groups/users/memberships/policies
                         |
      sanitized AWS CLI report -> Nginx audit page
```

Create your own architecture diagram in the report. The diagram must show network boundaries, traffic direction, security controls and the Terraform/Ansible/Bash responsibility split.

---

## 📝 Requirements

### Part 1: Git Repository, Planning and Security Hygiene (15 marks)

#### 1.1 Repository Structure (5 marks)

Use this minimum structure and extend it where required:

```text
Project7/
├── README.md
├── .gitignore
├── terraform/backend
├── terraform/modules/iam_groups
├── terraform/modules/iam_users
├── data/users.csv
├── data/users.example.csv
├── scripts/validate-users.sh
├── scripts/export-audit.sh
├── ansible/roles/audit_dashboard
├── audit/
├── screenshots/
├── docs/
└── report/
```

**Tasks:**

- [ ] Create the documented folder structure.
- [ ] Explain each major directory in README.md.
- [ ] Keep generated files separate from source code.

**Required evidence:**

- `project7_part1_project_structure.png` — readable `tree` output

#### 1.2 Git Workflow (5 marks)

**Tasks:**

- [ ] Create at least two feature branches.
- [ ] Merge changes through pull requests with meaningful descriptions.
- [ ] Preserve readable commits and add a final release tag named `project7-v1.0`.

**Required evidence:**

- `project7_part1_git_history.png` — branch/commit graph and release tag
- `project7_part1_pull_request.png` — merged pull request

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

- `project7_part1_gitignore.png` — ignore rules and a clean status/secret check

---

### Part 2: Terraform Infrastructure (30 marks)

#### 2.1 Remote state and IAM groups (10 marks)

**Tasks:**

- [ ] Create or document bootstrap of a private S3 backend, then migrate state.
- [ ] Create the three required IAM groups and attach only approved managed policies.
- [ ] Output group names and policy ARNs without secrets.

**Required evidence:**

- `project7_part2_section1_code.png` — relevant Terraform code
- `project7_part2_section1_result.png` — validation, plan, apply, output or AWS verification

#### 2.2 CSV-driven users and memberships (10 marks)

**Tasks:**

- [ ] Use csvdecode and for_each to create users and group memberships.
- [ ] Validate each row has unique username, owner, group and status values.
- [ ] Create login profiles or access keys only if explicitly required; mark outputs sensitive and never commit generated credentials.

**Required evidence:**

- `project7_part2_section2_code.png` — relevant Terraform code
- `project7_part2_section2_result.png` — validation, plan, apply, output or AWS verification

#### 2.3 Lifecycle and safety controls (10 marks)

**Tasks:**

- [ ] Represent joiner, mover and leaver changes as CSV edits reviewed through PRs.
- [ ] Prevent deletion of the final CloudAdmins member with a documented pre-apply check.
- [ ] Use terraform plan as the mandatory approval evidence before apply.

**Required evidence:**

- `project7_part2_section3_code.png` — relevant Terraform code
- `project7_part2_section3_result.png` — validation, plan, apply, output or AWS verification

---

### Part 3: Request Validation and Audit Publishing (25 marks)

#### 3.1 Git workflow and CSV validator (8 marks)

**Tasks:**

- [ ] Use feature branches and pull requests for at least one joiner, mover and leaver change.
- [ ] Validate duplicate usernames, unknown groups, missing owners and invalid status values with Bash.
- [ ] Block Terraform execution when validation fails.

**Required evidence:**

- `project7_part3_section1_code.png` — playbook, role, template or script
- `project7_part3_section1_run.png` — successful execution and relevant result

#### 3.2 Sanitized audit export (12 marks)

**Tasks:**

- [ ] Use AWS CLI list-users, list-groups-for-user and attached-policy queries.
- [ ] Create CSV/HTML audit output containing usernames, groups and create dates only.
- [ ] Exclude passwords, access-key secrets and private state data.
- [ ] Compare the export with Terraform outputs/state.

**Required evidence:**

- `project7_part3_section2_code.png` — playbook, role, template or script
- `project7_part3_section2_run.png` — successful execution and relevant result

#### 3.3 Ansible audit dashboard (5 marks)

**Tasks:**

- [ ] Configure a small Nginx page on localhost or a course EC2 instance.
- [ ] Publish only the sanitized report and restrict access with UFW or the security group.
- [ ] Use a handler and prove repeat runs are idempotent.

**Required evidence:**

- `project7_part3_section3_code.png` — playbook, role, template or script
- `project7_part3_section3_run.png` — successful execution and relevant result

---

### Part 4: Joiner-Mover-Leaver Test (20 marks)

#### 4.1 Lifecycle workflow (10 marks)

**Tasks:**

- [ ] Onboard a test user into Developers, move the user to Auditors and then remove the user.
- [ ] Capture a separate plan and reviewed PR for each phase.
- [ ] Verify the live result with AWS CLI after each apply.

**Required evidence:**

- `project7_part4_section1_before.png` — initial state or test setup
- `project7_part4_section1_after.png` — verified result, remediation or recovery

#### 4.2 Secret and safety verification (10 marks)

**Tasks:**

- [ ] Search Git history for credential-like files and prove none are committed.
- [ ] Demonstrate validator rejection for a duplicate username or unknown group.
- [ ] Verify S3 backend and audit dashboard are not public.

**Required evidence:**

- `project7_part4_section2_before.png` — initial state or test setup
- `project7_part4_section2_after.png` — verified result, remediation or recovery

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

- `project7_part5_readme_report.png` — rendered README and report contents
- `project7_part5_final_demo.png` — final working system

#### 5.2 Cleanup (5 marks)

**Tasks:**

- [ ] Capture all evidence before cleanup.
- [ ] Remove application data that blocks Terraform destroy only through the documented safe method.
- [ ] Run `terraform destroy` for every environment/workspace.
- [ ] Verify that project EC2 instances, volumes, security groups, S3 objects/buckets and other chargeable resources are removed.
- [ ] Keep only source code, sanitized reports and screenshots in GitHub.

**Required evidence:**

- `project7_part5_terraform_destroy.png` — successful destroy
- `project7_part5_aws_cleanup.png` — AWS CLI verification showing no project resources remain

---

## 📦 Submission Guidelines

### GitHub Repository

Use your existing public GitHub repository named `CC`. Store your assigned project in:

```text
CC/Projects/Project-7-iam-lifecycle-automation/
```

Submit the direct GitHub URL of this exact project directory. The repository must contain source files, example variables, documentation, screenshots and the final report. It must not contain credentials, private keys, state files or real tfvars.

### PDF Report

Name the report:

```text
Project7_<YourName>_<YourRollNumber>.pdf
```

The report must contain a cover page, table of contents, architecture, implementation, testing, security analysis, challenges, cleanup evidence and conclusion.

---

## 📊 Grading Rubric

| Component | Marks |
|---|---:|
| Part 1: Git Repository, Planning and Security Hygiene | 15 |
| Part 2: Terraform Infrastructure | 30 |
| Part 3: Request Validation and Audit Publishing | 25 |
| Part 4: Joiner-Mover-Leaver Test | 20 |
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
