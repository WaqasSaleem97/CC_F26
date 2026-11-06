# 📊 Project 1 – AWS Resource Inventory and Compliance Dashboard

**Due Date:** To be announced  
**Total Marks:** 100  
**Submission Format:** GitHub Repository + PDF Report  
**Course Coverage:** Labs 06–07 and 09–14

---

## 📋 Project Overview

Build a tag-driven AWS inventory and compliance dashboard. Terraform provisions a small monitored fleet and a dashboard server. AWS CLI queries and Bash scripts collect resource facts, Ansible configures the fleet, and Nginx publishes a read-only HTML compliance report.

### Scope

- One VPC, one public subnet, one dashboard EC2 instance and two managed EC2 instances
- Required tags: Project, Environment, Owner and ManagedBy
- Compliance checks for missing tags, unexpected public ports, instance state and Nginx health
- No AWS Config, CloudWatch, Lambda, Python or third-party monitoring platform

### Mandatory Rules

- Use only the AWS services and course tools explicitly listed in this specification.
- Create infrastructure through Terraform; do not substitute manual AWS Console creation for graded resources.
- Use Ansible and/or Bash for configuration tasks as specified; repeated runs must be safe.
- Never commit AWS credentials, private keys, passwords, Terraform state, real tfvars, or generated secret files.
- Capture evidence before destroying resources and remove chargeable resources immediately after verification.

---

## 🎯 Learning Objectives

- Create repeatable AWS infrastructure with Terraform modules and outputs
- Use AWS CLI filters and --query expressions for structured inventory reporting
- Write Bash functions and loops that produce deterministic compliance results
- Configure Nginx and fleet baselines through reusable Ansible roles
- Schedule report refreshes and document remediation decisions

---

## 🧱 Architecture Overview

```text
Administrator / Codespace
          |
          | Terraform + AWS CLI + Ansible
          v
+---------------- AWS VPC ----------------+
| Dashboard EC2 ----> Managed EC2 A       |
|      |               Managed EC2 B       |
|      +-- Nginx: inventory/compliance     |
+------------------------------------------+
```

Create your own architecture diagram in the report. The diagram must show network boundaries, traffic direction, security controls and the Terraform/Ansible/Bash responsibility split.

---

## 📝 Requirements

### Part 1: Git Repository, Planning and Security Hygiene (15 marks)

#### 1.1 Repository Structure (5 marks)

Use this minimum structure and extend it where required:

```text
Project1/
├── README.md
├── .gitignore
├── terraform/modules/network
├── terraform/modules/compute
├── ansible/roles/baseline
├── ansible/roles/dashboard
├── scripts/collect-inventory.sh
├── scripts/check-compliance.sh
├── dashboard/
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

- `project1_part1_project_structure.png` — readable `tree` output

#### 1.2 Git Workflow (5 marks)

**Tasks:**

- [ ] Create at least two feature branches.
- [ ] Merge changes through pull requests with meaningful descriptions.
- [ ] Preserve readable commits and add a final release tag named `project1-v1.0`.

**Required evidence:**

- `project1_part1_git_history.png` — branch/commit graph and release tag
- `project1_part1_pull_request.png` — merged pull request

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

- `project1_part1_gitignore.png` — ignore rules and a clean status/secret check

---

### Part 2: Terraform Infrastructure (30 marks)

#### 2.1 Network and security module (10 marks)

**Tasks:**

- [ ] Create a VPC, Internet Gateway, public subnet, route table and least-privilege security groups.
- [ ] Allow SSH only from your current public IP and HTTP only to the dashboard server.
- [ ] Expose VPC, subnet and security-group IDs as outputs.

**Required evidence:**

- `project1_part2_section1_code.png` — relevant Terraform code
- `project1_part2_section1_result.png` — validation, plan, apply, output or AWS verification

#### 2.2 Dashboard and managed fleet modules (10 marks)

**Tasks:**

- [ ] Create one dashboard instance and two managed instances with distinct Name and Role tags.
- [ ] Use variables, maps or objects to define instance names and tags without duplicating resources.
- [ ] Use user_data only for the minimum SSH/Python prerequisite needed by Ansible.

**Required evidence:**

- `project1_part2_section2_code.png` — relevant Terraform code
- `project1_part2_section2_result.png` — validation, plan, apply, output or AWS verification

#### 2.3 State, variables and validation (10 marks)

**Tasks:**

- [ ] Separate main.tf, variables.tf, locals.tf and outputs.tf.
- [ ] Validate environment and allowed instance-count values.
- [ ] Run terraform fmt -check, validate and plan before apply; inspect the state with terraform state list.

**Required evidence:**

- `project1_part2_section3_code.png` — relevant Terraform code
- `project1_part2_section3_result.png` — validation, plan, apply, output or AWS verification

---

### Part 3: Inventory, Compliance and Dashboard Automation (25 marks)

#### 3.1 Dynamic inventory and baseline role (8 marks)

**Tasks:**

- [ ] Use the amazon.aws.aws_ec2 inventory plugin filtered by project tags.
- [ ] Create a baseline role that installs Nginx, enables it and places a unique health page on each managed host.
- [ ] Prove a second Ansible run reports no unintended changes.

**Required evidence:**

- `project1_part3_section1_code.png` — playbook, role, template or script
- `project1_part3_section1_run.png` — successful execution and relevant result

#### 3.2 AWS inventory and compliance scripts (12 marks)

**Tasks:**

- [ ] Use aws ec2 describe-instances and describe-security-groups with filters and --query.
- [ ] Generate inventory.html and compliance.html without embedding credentials.
- [ ] Flag missing required tags, instances not running, port 22 open to 0.0.0.0/0 and failed HTTP health checks.
- [ ] Return exit code 0 when compliant and a non-zero code when violations exist.

**Required evidence:**

- `project1_part3_section2_code.png` — playbook, role, template or script
- `project1_part3_section2_run.png` — successful execution and relevant result

#### 3.3 Scheduled dashboard publishing (5 marks)

**Tasks:**

- [ ] Use Ansible to deploy scripts and the generated reports to the dashboard Nginx document root.
- [ ] Create a cron entry that refreshes the reports at a documented interval.
- [ ] Keep raw reports under reports/ and exclude transient/private files through .gitignore.

**Required evidence:**

- `project1_part3_section3_code.png` — playbook, role, template or script
- `project1_part3_section3_run.png` — successful execution and relevant result

---

### Part 4: Compliance Test and Remediation Drill (20 marks)

#### 4.1 Functional inventory verification (10 marks)

**Tasks:**

- [ ] Verify all three instances appear with correct IDs, states, private IPs and tags.
- [ ] Open the Nginx dashboard and confirm inventory and compliance pages are readable.
- [ ] Compare one dashboard value with the corresponding AWS CLI result.

**Required evidence:**

- `project1_part4_section1_before.png` — initial state or test setup
- `project1_part4_section1_after.png` — verified result, remediation or recovery

#### 4.2 Controlled violation and remediation (10 marks)

**Tasks:**

- [ ] Temporarily remove one non-critical tag or introduce a test-only security-group violation.
- [ ] Run the compliance script and capture the detected failure.
- [ ] Restore the correct Terraform configuration, apply it, rerun the checks and prove PASS status.

**Required evidence:**

- `project1_part4_section2_before.png` — initial state or test setup
- `project1_part4_section2_after.png` — verified result, remediation or recovery

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

- `project1_part5_readme_report.png` — rendered README and report contents
- `project1_part5_final_demo.png` — final working system

#### 5.2 Cleanup (5 marks)

**Tasks:**

- [ ] Capture all evidence before cleanup.
- [ ] Remove application data that blocks Terraform destroy only through the documented safe method.
- [ ] Run `terraform destroy` for every environment/workspace.
- [ ] Verify that project EC2 instances, volumes, security groups, S3 objects/buckets and other chargeable resources are removed.
- [ ] Keep only source code, sanitized reports and screenshots in GitHub.

**Required evidence:**

- `project1_part5_terraform_destroy.png` — successful destroy
- `project1_part5_aws_cleanup.png` — AWS CLI verification showing no project resources remain

---

## 📦 Submission Guidelines

### GitHub Repository

Use your existing public GitHub repository named `CC`. Store your assigned project in:

```text
CC/Projects/Project-1-resource-compliance-dashboard/
```

Submit the direct GitHub URL of this exact project directory. The repository must contain source files, example variables, documentation, screenshots and the final report. It must not contain credentials, private keys, state files or real tfvars.

### PDF Report

Name the report:

```text
Project1_<YourName>_<YourRollNumber>.pdf
```

The report must contain a cover page, table of contents, architecture, implementation, testing, security analysis, challenges, cleanup evidence and conclusion.

---

## 📊 Grading Rubric

| Component | Marks |
|---|---:|
| Part 1: Git Repository, Planning and Security Hygiene | 15 |
| Part 2: Terraform Infrastructure | 30 |
| Part 3: Inventory, Compliance and Dashboard Automation | 25 |
| Part 4: Compliance Test and Remediation Drill | 20 |
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
