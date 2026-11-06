# 🔏 Project 8 – Multi-Application HTTPS Gateway with Certificate Rotation

**Due Date:** To be announced  
**Total Marks:** 100  
**Submission Format:** GitHub Repository + PDF Report  
**Course Coverage:** Labs 07 and 11–15

---

## 📋 Project Overview

Build a single Nginx HTTPS gateway in front of three different applications: a static Nginx site, an Apache/HTTPD site and a PHP-FPM site. Terraform provisions the hosts; Ansible configures path-based proxying, self-signed certificates, certificate-expiry checks and safe automated rotation.

### Scope

- One public gateway and three private-style backend roles within the course network design
- Routes: /static/, /apache/ and /php/
- Self-signed certificates generated and rotated by Ansible
- No ACM, Route 53, external CA automation, managed load balancer or Kubernetes

### Mandatory Rules

- Use only the AWS services and course tools explicitly listed in this specification.
- Create infrastructure through Terraform; do not substitute manual AWS Console creation for graded resources.
- Use Ansible and/or Bash for configuration tasks as specified; repeated runs must be safe.
- Never commit AWS credentials, private keys, passwords, Terraform state, real tfvars, or generated secret files.
- Capture evidence before destroying resources and remove chargeable resources immediately after verification.

---

## 🎯 Learning Objectives

- Provision role-tagged web infrastructure with Terraform modules
- Configure multiple Linux web stacks through Ansible roles
- Implement Nginx HTTPS termination and path-based reverse proxying
- Use openssl checks, templates and handlers for safe certificate rotation
- Test backend isolation, redirects, headers and service continuity

---

## 🧱 Architecture Overview

```text
Client --HTTPS--> Nginx Gateway
                    | /static/ -> Nginx backend
                    | /apache/ -> HTTPD backend
                    | /php/    -> Nginx + PHP-FPM backend
```

Create your own architecture diagram in the report. The diagram must show network boundaries, traffic direction, security controls and the Terraform/Ansible/Bash responsibility split.

---

## 📝 Requirements

### Part 1: Git Repository, Planning and Security Hygiene (15 marks)

#### 1.1 Repository Structure (5 marks)

Use this minimum structure and extend it where required:

```text
Project8/
├── README.md
├── .gitignore
├── terraform/modules/network
├── terraform/modules/gateway
├── terraform/modules/backends
├── ansible/roles/gateway
├── ansible/roles/static_app
├── ansible/roles/apache_app
├── ansible/roles/php_app
├── ansible/roles/certificate
├── ansible/playbooks/rotate-cert.yml
├── scripts/check-certificate.sh
├── screenshots/
├── docs/
└── report/
```

**Tasks:**

- [ ] Create the documented folder structure.
- [ ] Explain each major directory in README.md.
- [ ] Keep generated files separate from source code.

**Required evidence:**

- `project8_part1_project_structure.png` — readable `tree` output

#### 1.2 Git Workflow (5 marks)

**Tasks:**

- [ ] Create at least two feature branches.
- [ ] Merge changes through pull requests with meaningful descriptions.
- [ ] Preserve readable commits and add a final release tag named `project8-v1.0`.

**Required evidence:**

- `project8_part1_git_history.png` — branch/commit graph and release tag
- `project8_part1_pull_request.png` — merged pull request

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

- `project8_part1_gitignore.png` — ignore rules and a clean status/secret check

---

### Part 2: Terraform Infrastructure (30 marks)

#### 2.1 Gateway network and security (10 marks)

**Tasks:**

- [ ] Create VPC, subnets, routes and separate gateway/backend security groups.
- [ ] Expose 80/443 only on the gateway and allow backend HTTP only from the gateway security group.
- [ ] Restrict SSH to the administrator CIDR.

**Required evidence:**

- `project8_part2_section1_code.png` — relevant Terraform code
- `project8_part2_section1_result.png` — validation, plan, apply, output or AWS verification

#### 2.2 Gateway and heterogeneous backends (10 marks)

**Tasks:**

- [ ] Create one gateway and three tagged backend instances.
- [ ] Use a module and role map instead of four copied resource blocks.
- [ ] Output gateway address and backend private IPs.

**Required evidence:**

- `project8_part2_section2_code.png` — relevant Terraform code
- `project8_part2_section2_result.png` — validation, plan, apply, output or AWS verification

#### 2.3 Variables and validation (10 marks)

**Tasks:**

- [ ] Validate backend role names and certificate validity days.
- [ ] Keep sensitive/private files out of Git.
- [ ] Run fmt, validate, plan, apply and state verification.

**Required evidence:**

- `project8_part2_section3_code.png` — relevant Terraform code
- `project8_part2_section3_result.png` — validation, plan, apply, output or AWS verification

---

### Part 3: Web Roles, HTTPS Gateway and Certificate Lifecycle (25 marks)

#### 3.1 Three application roles (8 marks)

**Tasks:**

- [ ] Configure a static Nginx backend, Apache/HTTPD backend and Nginx + PHP-FPM backend.
- [ ] Each application must return a unique role/version response.
- [ ] Prove all services are enabled and idempotent.

**Required evidence:**

- `project8_part3_section1_code.png` — playbook, role, template or script
- `project8_part3_section1_run.png` — successful execution and relevant result

#### 3.2 Gateway and TLS role (12 marks)

**Tasks:**

- [ ] Generate a self-signed key/certificate with protected file modes.
- [ ] Configure HTTP-to-HTTPS redirect and the three path routes.
- [ ] Set useful proxy headers, run nginx -t and reload only after validation.
- [ ] Return a controlled error page when a backend is unavailable.

**Required evidence:**

- `project8_part3_section2_code.png` — playbook, role, template or script
- `project8_part3_section2_run.png` — successful execution and relevant result

#### 3.3 Expiry check and rotation (5 marks)

**Tasks:**

- [ ] Create a script that prints subject, issuer, dates and days remaining.
- [ ] Rotate only when forced or below a documented threshold.
- [ ] Back up the previous certificate and use an Ansible handler for validated reload.

**Required evidence:**

- `project8_part3_section3_code.png` — playbook, role, template or script
- `project8_part3_section3_run.png` — successful execution and relevant result

---

### Part 4: Routing, Isolation and Rotation Test (20 marks)

#### 4.1 Gateway functional/security test (10 marks)

**Tasks:**

- [ ] Verify all three HTTPS paths and HTTP redirect.
- [ ] Show direct public backend access is blocked.
- [ ] Inspect the certificate and proxy response headers.

**Required evidence:**

- `project8_part4_section1_before.png` — initial state or test setup
- `project8_part4_section1_after.png` — verified result, remediation or recovery

#### 4.2 Certificate rotation drill (10 marks)

**Tasks:**

- [ ] Record the original certificate fingerprint.
- [ ] Run forced rotation and verify a different fingerprint and valid dates.
- [ ] Confirm all three applications remain reachable and Nginx configuration passes.

**Required evidence:**

- `project8_part4_section2_before.png` — initial state or test setup
- `project8_part4_section2_after.png` — verified result, remediation or recovery

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

- `project8_part5_readme_report.png` — rendered README and report contents
- `project8_part5_final_demo.png` — final working system

#### 5.2 Cleanup (5 marks)

**Tasks:**

- [ ] Capture all evidence before cleanup.
- [ ] Remove application data that blocks Terraform destroy only through the documented safe method.
- [ ] Run `terraform destroy` for every environment/workspace.
- [ ] Verify that project EC2 instances, volumes, security groups, S3 objects/buckets and other chargeable resources are removed.
- [ ] Keep only source code, sanitized reports and screenshots in GitHub.

**Required evidence:**

- `project8_part5_terraform_destroy.png` — successful destroy
- `project8_part5_aws_cleanup.png` — AWS CLI verification showing no project resources remain

---

## 📦 Submission Guidelines

### GitHub Repository

Use your existing public GitHub repository named `CC`. Store your assigned project in:

```text
CC/Projects/Project-8-https-gateway-rotation/
```

Submit the direct GitHub URL of this exact project directory. The repository must contain source files, example variables, documentation, screenshots and the final report. It must not contain credentials, private keys, state files or real tfvars.

### PDF Report

Name the report:

```text
Project8_<YourName>_<YourRollNumber>.pdf
```

The report must contain a cover page, table of contents, architecture, implementation, testing, security analysis, challenges, cleanup evidence and conclusion.

---

## 📊 Grading Rubric

| Component | Marks |
|---|---:|
| Part 1: Git Repository, Planning and Security Hygiene | 15 |
| Part 2: Terraform Infrastructure | 30 |
| Part 3: Web Roles, HTTPS Gateway and Certificate Lifecycle | 25 |
| Part 4: Routing, Isolation and Rotation Test | 20 |
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
