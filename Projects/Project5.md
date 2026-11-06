# 🐤 Project 5 – Canary Release Platform with Nginx Weighted Routing

**Due Date:** To be announced  
**Total Marks:** 100  
**Submission Format:** GitHub Repository + PDF Report  
**Course Coverage:** Labs 03 and 10–15

---

## 📋 Project Overview

Implement a canary deployment workflow using an Nginx load balancer, stable backend servers and one canary backend. Terraform provisions the infrastructure; Ansible deploys versions, adjusts Nginx weights, performs smoke checks and rolls back when validation fails.

### Scope

- One Nginx load-balancer EC2 instance, two stable backends and one canary backend
- Weighted upstream routing such as stable weight 9 and canary weight 1
- Application may be static HTML or a simple PHP page with visible version identity
- No ALB, ASG, containers, Kubernetes or external deployment platform

### Mandatory Rules

- Use only the AWS services and course tools explicitly listed in this specification.
- Create infrastructure through Terraform; do not substitute manual AWS Console creation for graded resources.
- Use Ansible and/or Bash for configuration tasks as specified; repeated runs must be safe.
- Never commit AWS credentials, private keys, passwords, Terraform state, real tfvars, or generated secret files.
- Capture evidence before destroying resources and remove chargeable resources immediately after verification.

---

## 🎯 Learning Objectives

- Build modular multi-instance infrastructure
- Use Nginx weighted upstream routing within course scope
- Deploy versioned content with Ansible roles and templates
- Automate smoke tests and rollback decisions with Bash
- Collect evidence that traffic distribution and rollback work

---

## 🧱 Architecture Overview

```text
Clients -> Nginx LB (HTTPS optional)
              | weight 9
              +--------> Stable A / Stable B (v1)
              | weight 1
              +--------> Canary C (v2)
```

Create your own architecture diagram in the report. The diagram must show network boundaries, traffic direction, security controls and the Terraform/Ansible/Bash responsibility split.

---

## 📝 Requirements

### Part 1: Git Repository, Planning and Security Hygiene (15 marks)

#### 1.1 Repository Structure (5 marks)

Use this minimum structure and extend it where required:

```text
Project5/
├── README.md
├── .gitignore
├── terraform/modules/network
├── terraform/modules/load_balancer
├── terraform/modules/backends
├── ansible/roles/backend
├── ansible/roles/load_balancer
├── ansible/playbooks/deploy-canary.yml
├── ansible/playbooks/promote.yml
├── scripts/smoke-test.sh
├── scripts/sample-traffic.sh
├── scripts/rollback.sh
├── screenshots/
├── docs/
└── report/
```

**Tasks:**

- [ ] Create the documented folder structure.
- [ ] Explain each major directory in README.md.
- [ ] Keep generated files separate from source code.

**Required evidence:**

- `project5_part1_project_structure.png` — readable `tree` output

#### 1.2 Git Workflow (5 marks)

**Tasks:**

- [ ] Create at least two feature branches.
- [ ] Merge changes through pull requests with meaningful descriptions.
- [ ] Preserve readable commits and add a final release tag named `project5-v1.0`.

**Required evidence:**

- `project5_part1_git_history.png` — branch/commit graph and release tag
- `project5_part1_pull_request.png` — merged pull request

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

- `project5_part1_gitignore.png` — ignore rules and a clean status/secret check

---

### Part 2: Terraform Infrastructure (30 marks)

#### 2.1 Network and security (10 marks)

**Tasks:**

- [ ] Create VPC, subnets, routing and security groups.
- [ ] Expose HTTP/HTTPS only on the load balancer; allow backend web traffic only from its security group.
- [ ] Restrict SSH to the administrator CIDR.

**Required evidence:**

- `project5_part2_section1_code.png` — relevant Terraform code
- `project5_part2_section1_result.png` — validation, plan, apply, output or AWS verification

#### 2.2 Load balancer and backend modules (10 marks)

**Tasks:**

- [ ] Create one load balancer, two stable and one canary instance with role/version tags.
- [ ] Use for_each or maps rather than copy-pasted instance blocks.
- [ ] Output role-grouped private IPs for automation.

**Required evidence:**

- `project5_part2_section2_code.png` — relevant Terraform code
- `project5_part2_section2_result.png` — validation, plan, apply, output or AWS verification

#### 2.3 Terraform validation and reporting (10 marks)

**Tasks:**

- [ ] Validate deployment_mode and canary_weight inputs.
- [ ] Run fmt, validate, plan and apply.
- [ ] Produce an inventory-ready output or use tags for dynamic inventory.

**Required evidence:**

- `project5_part2_section3_code.png` — relevant Terraform code
- `project5_part2_section3_result.png` — validation, plan, apply, output or AWS verification

---

### Part 3: Canary Deployment and Promotion Automation (25 marks)

#### 3.1 Backend and load-balancer roles (8 marks)

**Tasks:**

- [ ] Deploy versioned pages to stable and canary groups.
- [ ] Template the Nginx upstream with explicit weights and run nginx -t before reload.
- [ ] Use handlers and prove idempotence.

**Required evidence:**

- `project5_part3_section1_code.png` — playbook, role, template or script
- `project5_part3_section1_run.png` — successful execution and relevant result

#### 3.2 Canary, smoke-test and promotion playbooks (12 marks)

**Tasks:**

- [ ] Deploy v2 only to canary while stable remains v1.
- [ ] Run HTTP status and content smoke tests directly against canary and through the load balancer.
- [ ] Promote by deploying v2 to stable and resetting the upstream configuration only after tests pass.
- [ ] Record the active version in a deployment log.

**Required evidence:**

- `project5_part3_section2_code.png` — playbook, role, template or script
- `project5_part3_section2_run.png` — successful execution and relevant result

#### 3.3 Traffic sampler and rollback (5 marks)

**Tasks:**

- [ ] Send at least 50 requests and summarize stable/canary responses.
- [ ] Create a rollback script/playbook that removes canary from routing or restores the last stable version.
- [ ] Require nginx -t and a confirmation/status check.

**Required evidence:**

- `project5_part3_section3_code.png` — playbook, role, template or script
- `project5_part3_section3_run.png` — successful execution and relevant result

---

### Part 4: Canary Success and Failure Drill (20 marks)

#### 4.1 Weighted-routing test (10 marks)

**Tasks:**

- [ ] Show v1 and v2 responses while canary mode is active.
- [ ] Summarize sampled counts and explain why exact percentages can vary.
- [ ] Promote a healthy version and confirm all responses report v2.

**Required evidence:**

- `project5_part4_section1_before.png` — initial state or test setup
- `project5_part4_section1_after.png` — verified result, remediation or recovery

#### 4.2 Automated rollback test (10 marks)

**Tasks:**

- [ ] Deploy a deliberately failing canary page or stop only the canary service.
- [ ] Run smoke tests and prove promotion is blocked.
- [ ] Execute rollback and verify stable traffic remains healthy.

**Required evidence:**

- `project5_part4_section2_before.png` — initial state or test setup
- `project5_part4_section2_after.png` — verified result, remediation or recovery

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

- `project5_part5_readme_report.png` — rendered README and report contents
- `project5_part5_final_demo.png` — final working system

#### 5.2 Cleanup (5 marks)

**Tasks:**

- [ ] Capture all evidence before cleanup.
- [ ] Remove application data that blocks Terraform destroy only through the documented safe method.
- [ ] Run `terraform destroy` for every environment/workspace.
- [ ] Verify that project EC2 instances, volumes, security groups, S3 objects/buckets and other chargeable resources are removed.
- [ ] Keep only source code, sanitized reports and screenshots in GitHub.

**Required evidence:**

- `project5_part5_terraform_destroy.png` — successful destroy
- `project5_part5_aws_cleanup.png` — AWS CLI verification showing no project resources remain

---

## 📦 Submission Guidelines

### GitHub Repository

Create or use your assigned course repository and submit the URL of:

```text
CC_<YourName>_<YourRollNumber>/Project-5-canary-release-platform/
```

The repository must contain source files, example variables, documentation, screenshots and the final report. It must not contain credentials, private keys, state files or real tfvars.

### PDF Report

Name the report:

```text
Project5_<YourName>_<YourRollNumber>.pdf
```

The report must contain a cover page, table of contents, architecture, implementation, testing, security analysis, challenges, cleanup evidence and conclusion.

---

## 📊 Grading Rubric

| Component | Marks |
|---|---:|
| Part 1: Git Repository, Planning and Security Hygiene | 15 |
| Part 2: Terraform Infrastructure | 30 |
| Part 3: Canary Deployment and Promotion Automation | 25 |
| Part 4: Canary Success and Failure Drill | 20 |
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
