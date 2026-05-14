# AWS + Kubernetes CI/CD Automation Project

> End-to-end CI/CD pipeline using **Jenkins · Ansible · Kubernetes · AWS EC2** — fully automated, zero manual steps

---

## 📌 Project Overview

This repository contains **2 real-world DevOps projects** built on AWS EC2 infrastructure, demonstrating complete CI/CD automation from GitHub code push to cloud resource provisioning and container deployment.

| | Project 1 | Project 2 |
|---|---|---|
| **Goal** | Auto-provision AWS EC2 instances | Deploy Kubernetes pods remotely |
| **Trigger** | GitHub Webhook | GitHub Webhook |
| **Key Tool** | Jenkins + Bash Scripts | Jenkins + Ansible Playbooks |
| **Result** | EC2 running in under 3 min | Kubernetes pod in Running state |

---

## 🏗️ Architecture

```
GitHub Repository  (code push → webhook trigger)
        │
        ▼
  Jenkins Server   (3-stage CI/CD pipeline · AWS EC2 t3.micro · ap-south-1)
        │
        ▼
  SCP File Transfer  (playbooks transferred to Ansible server)
        │
        ▼
  Ansible Server   (playbook execution via SSH · AWS EC2 t3.micro)
        │
   ┌────┴────┐
   ▼         ▼
Project 1  Project 2
AWS EC2    Kubernetes
Provision  Pod Deploy
```

---

## 📁 Project 1 — Automated AWS EC2 Provisioning via Jenkins CI/CD

### Objective
Automate AWS EC2 instance provisioning using Jenkins and parameterised Bash scripts — eliminating 100% of manual AWS Console steps.

### Tools Used
`Jenkins` `GitHub Webhooks` `AWS EC2` `AWS IAM` `Bash Scripting` `Linux Ubuntu`

### Pipeline Flow

**Stage 1 — GitHub Connection**
- Jenkins pipeline triggers automatically on every GitHub code push via Webhook
- Fully event-driven — no manual build start required
- Version-controlled auditability across 2 repositories

**Stage 2 — Parameterised Bash Script Execution**
- Jenkins executes parameterised Bash shell scripts to provision AWS EC2 resources on demand
- Improves deployment repeatability and reduces human error by eliminating manual input

**Stage 3 — AWS EC2 Provisioning**
- EC2 instance provisioned programmatically using AWS CLI commands in the Bash script
- AWS IAM least-privilege access policies enforced on Linux Ubuntu
- Cloud security best practices applied across Jenkins and AWS EC2 interactions

### Jenkins Pipeline Code

```groovy
node {
  stage('GitHub connect') {
    git branch: 'main',
    url: 'https://github.com/PushpenderKumar7505/Devops-automation-jenkins-ansible.git'
  }
  stage('Provision EC2') {
    sh 'bash provision_ec2.sh'
  }
}
```

### Results
- ✅ Provisioning time reduced from **15+ minutes → under 3 minutes**
- ✅ 100% manual AWS Console steps eliminated
- ✅ IAM least-privilege access enforced
- ✅ Fully event-driven via GitHub Webhooks

---

## ☸️ Project 2 — 3-Tier CI/CD: Jenkins + Ansible + Kubernetes on AWS EC2

### Objective
Deploy Kubernetes pods remotely using a 3-tier CI/CD pipeline — Jenkins orchestrates Ansible, which provisions AWS EC2 instances and deploys Kubernetes pods across a multi-node cluster.

### Tools Used
`Jenkins` `Ansible` `Kubernetes` `AWS EC2` `GitHub` `Linux Ubuntu` `SSH / SCP` `python3-boto` `MobaXterm`

### Infrastructure Setup — 3 AWS EC2 Instances

| Server | Instance Type | Region | Role |
|---|---|---|---|
| Jenkins | t3.micro | ap-south-1 | CI/CD orchestration |
| Ansible | t3.micro | ap-south-1 | Playbook execution |
| Kubernetes | c7i.flex.large | ap-south-1 | Pod deployment |

### Pipeline Flow

**Stage 1 — GitHub Code Checkout**
- Jenkins pulls latest code from GitHub repository
- Ansible playbook files (`ec2.yml`, `pod1.yml`) are version-controlled in the repo

**Stage 2 — SCP File Transfer to Ansible**
- Jenkins workspace se playbook files SCP ke through Ansible server pe transfer
- SSH Agent plugin used for passwordless authentication between Jenkins and Ansible

**Stage 3 — Remote Ansible Playbook Execution**
- Jenkins SSH se Ansible server pe connect karta hai
- `ec2.yml` — Ansible playbook using `amazon.aws` collection to provision AWS EC2 instances (security group creation, AMI selection, key-pair configuration)
- `pod1.yml` — Ansible playbook to remotely deploy Kubernetes pod on the cluster

**Passwordless SSH Setup**
- SSH key-pair generated and added to `authorized_keys` on Kubernetes server
- Verified with: `ansible -m ping` → **0 failures**

### Jenkins Pipeline Code

```groovy
node {
  stage('Github connect') {
    git branch: 'main',
    url: 'https://github.com/PushpenderKumar7505/Devops-automation-jenkins-ansible.git'
  }
  stage('ansible-server') {
    sshagent(['ansible']) {
      sh 'scp /var/lib/jenkins/workspace/Aws-project/* ubuntu@<ansible-ip>:/home/ubuntu'
    }
  }
  stage('ansible deploy') {
    sshagent(['ansible']) {
      sh 'ssh ubuntu@<ansible-ip> ansible-playbook ec2.yml'
      sh 'ssh ubuntu@<ansible-ip> ansible-playbook pod1.yml'
    }
  }
}
```

### Ansible Playbook — ec2.yml (EC2 Provisioning)

```yaml
# ec2.yml — Provisions AWS EC2 instance using amazon.aws collection
- hosts: localhost
  tasks:
    - name: Provision EC2 instance
      amazon.aws.ec2_instance:
        name: "ansible-managed"
        instance_type: t3.micro
        image_id: <ami-id>
        region: ap-south-1
        key_name: <key-pair>
        security_group: devops-sg
        state: present
```

### Ansible Playbook — pod1.yml (Kubernetes Pod Deployment)

```yaml
# pod1.yml — Deploys Kubernetes pod remotely via Ansible
- hosts: kubernetes
  tasks:
    - name: Deploy ubuntu pod
      command: kubectl apply -f pod-config.yml
```

### Results
- ✅ 3 AWS EC2 instances provisioned and managed simultaneously
- ✅ Kubernetes pod deployed in **Running state** — verified via `kubectl get pods`
- ✅ Passwordless SSH authentication between all 3 nodes — **0 failures**
- ✅ All manual infrastructure steps eliminated
- ✅ Multi-node SSH session management via MobaXterm

---

## ⚙️ Complete Tech Stack

| Category | Tools |
|---|---|
| CI/CD | Jenkins, GitHub Webhooks, Pipeline Configuration |
| Configuration Management | Ansible, Ansible Playbooks, SSH Key Authentication |
| Container Orchestration | Kubernetes, kubectl, Pods, Deployments |
| Cloud Platform | AWS EC2, AWS IAM, AWS S3, AWS VPC, AWS CloudWatch |
| Scripting | Bash Scripting, Shell Scripting |
| Operating System | Linux Ubuntu, Command Line |
| Version Control | Git, GitHub |
| Networking | SSH, SCP, Passwordless Auth |
| Tools | MobaXterm, python3-boto |

---

## 📸 Project Screenshots

| File | Description |
|---|---|
| `P1-01-jenkins-dashboard.png` | Jenkins dashboard overview |
| `P1-02-jenkins-pipeline-overview.png` | Pipeline stages view |
| `P1-03-jenkins-pipeline-config.png` | Pipeline configuration |
| `P1-04-jenkins-build-history.png` | Successful build history |
| `P1-05-ansible-ec2-playbook.png` | ec2.yml playbook execution output |
| `P2-01-kubernetes-pod-running.png` | Pod in Running state — kubectl verified |
| `P2-02-ansible-pod-deploy-recap.png` | Ansible pod deployment recap |
| `P2-03-jenkins-k8s-pipeline-config.png` | Jenkins K8s pipeline config |
| `P2-04-jenkins-both-projects.png` | Both projects in Jenkins |
| `P2-05-aws-ec2-3-instances.png` | 3 EC2 instances running in AWS Console |
| `P2-06-jenkins-both-projects-2.png` | Jenkins dashboard with both pipelines |
| `P2-07-mobaxterm-sessions.png` | MobaXterm multi-node SSH sessions |

---

## 👨‍💻 Author

**Pushpender Kumar**
B.Tech CSE — GLA University, 2024

- 🔗 [LinkedIn](https://linkedin.com/in/pushpender-kumar-5280b7226)
- 🐙 [GitHub](https://github.com/PushpenderKumar7505)
- 📧 pushpender7505@gmail.com
