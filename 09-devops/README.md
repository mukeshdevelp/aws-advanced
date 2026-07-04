# 9. DevOps Services ⭐⭐⭐⭐⭐

CI/CD automation and Infrastructure as Code.

**Topics:**
1. [CodeCommit](#1-aws-codecommit)
2. [CodeBuild](#2-aws-codebuild)
3. [CodeDeploy](#3-aws-codedeploy)
4. [CodePipeline](#4-aws-codepipeline)
5. [CloudFormation](#5-aws-cloudformation)
6. [Systems Manager](#6-aws-systems-manager)

---

# 1. AWS CodeCommit

## What is it?

Managed private Git repositories integrated with AWS CI/CD. Encrypted, IAM-controlled.

## Architecture

```
Developer → git push → CodeCommit → Triggers CodePipeline
```

## Components

| Component | Notes |
|-----------|-------|
| **Repository** | Private Git repo in AWS |
| **Branch** | Git branch for feature/workflow isolation |
| **Trigger** | CloudWatch Events rule to start pipeline |
| **Credential Helper** | IAM-based Git authentication helper |

## Related AWS Services

- [CodePipeline](#4-aws-codepipeline)
- [CodeBuild](#2-aws-codebuild)

## Exam Tips

- Often replaced by GitHub/GitLab as CodePipeline source
- Know it exists for exams

## One Line Definition

**CodeCommit = managed private Git repositories.**

---

# 2. AWS CodeBuild

## What is it?

Managed build service — compiles code, runs tests, produces artifacts. Pay per build minute.

## Architecture

```
Source → CodeBuild → buildspec.yml → Artifact (S3/ECR)
```

## Components

| Component | Notes |
|-----------|-------|
| **Build Project** | Config linking source, env, and artifacts |
| **buildspec.yml** | YAML defining build phases and commands |
| **Environment** | OS, runtime, or custom Docker image |
| **Cache** | Speed up builds by caching dependencies |
| **Reports** | Test and coverage report output |

## Workflow

1. Pipeline Triggers
2. Read Source
3. Run buildspec phases
4. Output Artifact

## Related AWS Services

- [CodePipeline](#4-aws-codepipeline)
- [ECR](../13-containers/README.md#2-amazon-ecr)

## Exam Tips

buildspec.yml phases: install, pre_build, build, post_build. Artifacts → S3 or ECR.

## One Line Definition

**CodeBuild = managed build and test service.**

---

# 3. AWS CodeDeploy

## What is it?

Automated deployments to EC2, Lambda, ECS, on-premises. In-place or blue/green strategies.

## Architecture

```
Artifact → CodeDeploy → Deployment Group → EC2/Lambda/ECS
                              ↓
                    In-Place or Blue/Green
```

## Components

| Component | Notes |
|-----------|-------|
| **Application** | Logical grouping of deployment targets |
| **Deployment Group** | EC2/Lambda/ECS targets for deployment |
| **AppSpec** | YAML/JSON defining deployment lifecycle hooks |
| **Deployment Config** | OneAtATime, HalfAtATime, or AllAtOnce |

## Comparison

| Blue/Green | In-Place |
|------------|----------|
| New instances, switch traffic | Update existing |
| Zero downtime | Faster, some downtime |

## Related AWS Services

- [EC2](../01-compute/README.md#1-amazon-ec2)
- [ECS](../13-containers/README.md#3-amazon-ecs)
- [ALB](../05-load-balancing/README.md)

## Exam Tips

Blue/green = new instances + traffic switch. Requires agent on EC2/on-prem.

## One Line Definition

**CodeDeploy = automated application deployment service.**

---

# 4. AWS CodePipeline

## What is it?

Managed CI/CD orchestration — source, build, test, deploy stages with manual approval gates.

## Architecture

```
Source → Build → Test → Deploy Staging → Approval → Deploy Production
 GitHub  CodeBuild        CodeDeploy         CloudFormation
```

## Components

| Component | Notes |
|-----------|-------|
| **Pipeline** | End-to-end CI/CD workflow definition |
| **Stage** | Logical boundary (Build, Test, Deploy) |
| **Action** | Task within a stage (CodeBuild, Deploy) |
| **Artifact Store** | S3 bucket holding pipeline artifacts |
| **Webhook** | Trigger pipeline on source code push |

## Related AWS Services

- [CodeBuild](#2-aws-codebuild)
- [CodeDeploy](#3-aws-codedeploy)
- [CloudFormation](#5-aws-cloudformation)

## Exam Tips

Source: CodeCommit, GitHub, S3, ECR. Artifact store = S3 bucket.

## One Line Definition

**CodePipeline = managed CI/CD orchestration.**

---

# 5. AWS CloudFormation

## What is it?

Infrastructure as Code — define AWS resources in YAML/JSON templates as stacks.

## Architecture

```
Template → Stack → AWS Resources
              ↓
         Change Set (preview)
```

## Components

| Component | Notes |
|-----------|-------|
| **Template** | YAML/JSON defining AWS resources |
| **Stack** | Live set of resources from a template |
| **Parameter** | Input values at stack creation |
| **Change Set** | Preview of stack update changes |
| **StackSet** | Deploy stacks across accounts/regions |
| **DeletionPolicy** | Retain/Snapshot resources on stack delete |

## Comparison

| CloudFormation | Terraform |
|----------------|-----------|
| AWS-native | Multi-cloud |
| StackSets | Modules |

## Related AWS Services

- [CodePipeline](#4-aws-codepipeline)
- [Organizations](../12-governance/README.md#1-aws-organizations)

## Exam Tips

DeletionPolicy: Delete, Retain, Snapshot. Change Set = preview. StackSets = multi-account/region.

## One Line Definition

**CloudFormation = Infrastructure as Code for AWS.**

---

# 6. AWS Systems Manager

## What is it?

Unified ops hub — **Run Command**, **Patch Manager**, **Session Manager** (no SSH), **Parameter Store**, **State Manager**.

## Architecture

```
Systems Manager → SSM Agent → Run Command / Patch / Session
                        ↓
              Parameter Store / Automation
```

## Components

| Component | Notes |
|-----------|-------|
| **Run Command** | Execute commands on fleet without SSH |
| **Patch Manager** | Automate OS patching across instances |
| **Session Manager** | Browser-based shell; no SSH keys needed |
| **Parameter Store** | Secure config and secrets storage |
| **State Manager** | Maintain desired configuration state |
| **Automation Document** | Reusable SSM automation runbooks |

## Advantages

- No SSH keys (Session Manager), centralized patching, automation

## Limitations

- Requires SSM Agent
- VPC endpoints for private instances
## Comparison

| Session Manager | SSH |
|-----------------|-----|
| IAM-based, no keys | Key management |
| Audit in CloudTrail | Manual logging |

## Related AWS Services

- [EC2](../01-compute/README.md#1-amazon-ec2)
- [Secrets Manager](../07-security/README.md#3-aws-secrets-manager)
- [CloudWatch](../08-monitoring/README.md#1-amazon-cloudwatch)

## Exam Tips

Parameter Store Standard = free. Advanced = paid, larger, policies. Session Manager = no bastion needed.

## One Line Definition

**Systems Manager = unified EC2/on-prem operations and management.**
