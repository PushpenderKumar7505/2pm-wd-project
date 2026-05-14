# AWS + Kubernetes CI/CD Automation Project

## 📌 Project Overview

This project demonstrates an end-to-end CI/CD automation pipeline using Jenkins, Ansible, AWS EC2, and Kubernetes.

The pipeline automates:
- Pulling code from GitHub
- Transferring files to the Ansible server
- Running Ansible playbooks
- Provisioning AWS EC2 instances
- Deploying Kubernetes Pods remotely

---

# 🏗️ Architecture

```text
GitHub Repository
        ↓
   Jenkins Server
        ↓
 SCP Files to Ansible
        ↓
   Ansible Playbooks
        ↓
 ┌─────────────────────┐
 │ AWS EC2 Provision   │
 │ Kubernetes Deploy   │
 └─────────────────────┘
```

⚙️ Technologies Used
Jenkins
Ansible
Kubernetes
AWS EC2
GitHub
Linux (Ubuntu)
Python3-boto
SSH / SCP
MobaXterm

📁 Project 1 — AWS EC2 Provisioning
🔹 Objective

Automate AWS EC2 instance creation using Jenkins and Ansible.

🔹 Jenkins Pipeline Flow
Stage 1 — GitHub Connection
Pulls code from GitHub repository
Stage 2 — Transfer Files
SCP transfers playbook files to Ansible server
Stage 3 — Ansible Deployment
Runs ec2.yml playbook from Ansible server

☸️ Project 2 — Kubernetes Pod Deployment
🔹 Objective

Deploy Kubernetes pods using Ansible through Jenkins automation.

🚀 Jenkins Pipeline Example
node {

  stage('Github connect') {
    git branch: 'main',
    url: 'https://github.com/PushpenderKumar7505/Devops-automation-jenkins-ansible.git'
  }

  stage('ansible-server') {
    sshagent(['ansible']) {
      sh 'scp /var/lib/jenkins/workspace/Aws-project/* ubuntu@172.31.35.92:/home/ubuntu'
    }
  }

  stage('ansible deploy') {
    sshagent(['ansible']) {
      sh 'ssh ubuntu@172.31.35.92 ansible-playbook ec2.yml'
    }
  }
}

✅ Key Features
Automated AWS EC2 provisioning
Kubernetes pod deployment automation
Jenkins CI/CD integration
GitHub pipeline integration
Passwordless SSH authentication
Infrastructure automation using Ansible

👨‍💻 Author

Pushpender Kumar

GitHub Repository:
https://github.com/PushpenderKumar7505/Devops-automation-jenkins-ansible
