# AWS + Kubernetes CI/CD Automation Project

> End-to-end CI/CD pipeline using **Jenkins · Ansible · Kubernetes · AWS EC2** — fully automated, zero manual steps

<br>

<div align="center">

| | Project 1 | Project 2 |
|:---:|:---:|:---:|
| **Goal** | Auto-provision AWS EC2 instances | Deploy Kubernetes pods remotely |
| **Trigger** | GitHub Webhook | GitHub Webhook |
| **Key Tools** | Jenkins + Bash Scripts | Jenkins + Ansible + Kubernetes |
| **Result** | ✅ EC2 running in under 3 min | ✅ Kubernetes pod Running state |

</div>

---

## 🏗️ Overall Architecture

```mermaid
flowchart TD
    A[("🐙 GitHub Repository")] -->|"Webhook on code push"| B
    B["⚙️ Jenkins Server\nt3.micro · ap-south-1"] -->|"SCP Transfer"| D
    D["📦 Ansible Server\nt3.micro · SSH auth"] -->|"ec2.yml playbook"| E
    D -->|"pod1.yml playbook"| F
    E["☁️ AWS EC2 Instance\nProvisioned automatically ✅"]
    F["☸️ Kubernetes Cluster\nPod in Running state ✅"]

    style A fill:#2d333b,color:#adbac7,stroke:#444c56
    style B fill:#1158c7,color:#ffffff,stroke:#1158c7
    style D fill:#e3a008,color:#000000,stroke:#e3a008
    style E fill:#0e8a16,color:#ffffff,stroke:#0e8a16
    style F fill:#6f42c1,color:#ffffff,stroke:#6f42c1
```

---

## 📁 Project 1 — Automated AWS EC2 Provisioning via Jenkins CI/CD

**Automate AWS EC2 instance provisioning using Jenkins and parameterised Bash scripts — eliminating 100% of manual AWS Console steps.**

<br>

### 🔧 Tools Used

![Jenkins](https://img.shields.io/badge/Jenkins-D24939?style=flat&logo=jenkins&logoColor=white)
![GitHub Actions](https://img.shields.io/badge/GitHub_Webhooks-181717?style=flat&logo=github&logoColor=white)
![AWS EC2](https://img.shields.io/badge/AWS_EC2-FF9900?style=flat&logo=amazonaws&logoColor=white)
![AWS IAM](https://img.shields.io/badge/AWS_IAM-FF9900?style=flat&logo=amazonaws&logoColor=white)
![Bash](https://img.shields.io/badge/Bash_Scripting-4EAA25?style=flat&logo=gnubash&logoColor=white)
![Linux](https://img.shields.io/badge/Linux_Ubuntu-E95420?style=flat&logo=ubuntu&logoColor=white)

<br>

### Pipeline Flow

```mermaid
flowchart LR
    A["🐙 GitHub\nCode Push"] -->|"Webhook trigger"| B
    B["⚙️ Jenkins\nPipeline"] --> C
    C["Stage 1\n📥 Code Checkout"] --> D
    D["Stage 2\n📜 Bash Script\nExecution"] --> E
    E["Stage 3\n☁️ AWS EC2\nProvisioned ✅"]

    style A fill:#2d333b,color:#adbac7,stroke:#444c56
    style B fill:#1158c7,color:#ffffff,stroke:#1158c7
    style C fill:#0075ca,color:#ffffff,stroke:#0075ca
    style D fill:#e3a008,color:#000000,stroke:#e3a008
    style E fill:#0e8a16,color:#ffffff,stroke:#0e8a16
```

<br>

### Step-by-Step Breakdown

**① GitHub Webhook Trigger**
Developer code push karta hai → GitHub Webhook automatically Jenkins pipeline trigger karta hai. Koi manual build start nahi karna padta — fully event-driven execution.

**② Jenkins Pipeline — 3 Automated Stages**
Stage 1: GitHub se code checkout.
Stage 2: Parameterised Bash shell scripts execute.
Stage 3: AWS CLI commands se EC2 provision.

**③ AWS IAM Least-Privilege Access**
Jenkins server ko EC2 provision karne ke liye IAM role assign ki — minimum permissions enforce ki. Cloud security best practices follow ki gayi.

**④ EC2 Instance Running — Verified**
2 GitHub repositories pe version-controlled auditability. Provisioning time 15+ minutes se ghatkar under 3 minutes.

<br>

### Jenkins Pipeline Code

```groovy
node {
  stage('GitHub connect') {
    git branch: 'main',
    url: 'https://github.com/PushpenderKumar7505/Devops-automation-jenkins-ansible.git'
  }
  stage('Provision EC2') {
    sh 'bash provision_ec2.sh'   // parameterised Bash script
  }
}
```

<br>

> [!TIP]
> ✅ **Result** — AWS EC2 instance provisioned automatically in under 3 minutes · 100% manual steps eliminated · IAM least-privilege enforced · Fully event-driven via GitHub Webhooks

---

## ☸️ Project 2 — 3-Tier CI/CD: Jenkins + Ansible + Kubernetes

**Deploy Kubernetes pods remotely using a 3-tier CI/CD pipeline across 3 AWS EC2 instances.**

<br>

### 🔧 Tools Used

![Jenkins](https://img.shields.io/badge/Jenkins-D24939?style=flat&logo=jenkins&logoColor=white)
![Ansible](https://img.shields.io/badge/Ansible-EE0000?style=flat&logo=ansible&logoColor=white)
![Kubernetes](https://img.shields.io/badge/Kubernetes-326CE5?style=flat&logo=kubernetes&logoColor=white)
![AWS EC2](https://img.shields.io/badge/AWS_EC2-FF9900?style=flat&logo=amazonaws&logoColor=white)
![GitHub](https://img.shields.io/badge/GitHub-181717?style=flat&logo=github&logoColor=white)
![Linux](https://img.shields.io/badge/Linux_Ubuntu-E95420?style=flat&logo=ubuntu&logoColor=white)
![Python](https://img.shields.io/badge/python3--boto-3776AB?style=flat&logo=python&logoColor=white)

<br>

### Infrastructure — 3 AWS EC2 Instances

```mermaid
flowchart LR
    A["⚙️ Jenkins Server\nt3.micro · ap-south-1"] -->|"SSH + SCP"| B
    B["📦 Ansible Server\nt3.micro · ap-south-1"] -->|"SSH + ansible-playbook"| C
    C["☸️ Kubernetes Server\nc7i.flex.large · ap-south-1"]

    style A fill:#1158c7,color:#ffffff,stroke:#1158c7
    style B fill:#e3a008,color:#000000,stroke:#e3a008
    style C fill:#6f42c1,color:#ffffff,stroke:#6f42c1
```

<br>

### Pipeline Flow

```mermaid
flowchart TD
    A["🐙 GitHub Repository"] -->|"Webhook trigger"| B
    B["⚙️ Jenkins Server"] --> C
    C["Stage 1 — GitHub Checkout\nPull ec2.yml + pod1.yml"] --> D
    D["Stage 2 — SCP Transfer\nPlaybooks → Ansible server"] --> E
    E["Stage 3 — Ansible Deploy\nRemote playbook execution via SSH"] --> F & G
    F["ec2.yml ☁️\nAWS EC2 Provisioned\nSecurity group · AMI · Key-pair ✅"]
    G["pod1.yml ☸️\nKubernetes Pod Deployed\nubuntu:latest · Running ✅"]

    style A fill:#2d333b,color:#adbac7,stroke:#444c56
    style B fill:#1158c7,color:#ffffff,stroke:#1158c7
    style C fill:#0075ca,color:#ffffff,stroke:#0075ca
    style D fill:#0075ca,color:#ffffff,stroke:#0075ca
    style E fill:#e3a008,color:#000000,stroke:#e3a008
    style F fill:#0e8a16,color:#ffffff,stroke:#0e8a16
    style G fill:#6f42c1,color:#ffffff,stroke:#6f42c1
```

<br>

### Step-by-Step Breakdown

**① Jenkins — GitHub Code Checkout (Stage 1)**
GitHub se latest code pull karta hai. Ansible playbook files `ec2.yml` aur `pod1.yml` repository mein stored hain.

**② SCP File Transfer to Ansible (Stage 2)**
Jenkins workspace se playbook files SCP ke through Ansible server pe transfer hoti hain. SSH Agent plugin se passwordless authentication.

**③ Ansible Playbook Execution (Stage 3)**
Jenkins SSH se Ansible server pe connect karta hai aur remotely playbook run karta hai.
- `ec2.yml` — AWS EC2 provision karta hai (security group creation, AMI selection, key-pair configuration)
- `pod1.yml` — Kubernetes server pe pod remotely deploy karta hai

**④ Passwordless SSH — Ansible → Kubernetes**
SSH key-pair generate karke `authorized_keys` mein add kiya. Verification:
```bash
ansible -m ping   # → 0 failures confirmed
```

**⑤ Kubernetes Pod — Running State Verified**
`ubuntu:latest` image se multi-container pod deploy hua. `kubectl get pods` se Running state confirm kiya. MobaXterm se 3 nodes simultaneously manage kiye.

<br>

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

### Ansible Playbook — ec2.yml

```yaml
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

### Ansible Playbook — pod1.yml

```yaml
- hosts: kubernetes
  tasks:
    - name: Deploy Kubernetes pod
      command: kubectl apply -f pod-config.yml
```

<br>

> [!TIP]
> ✅ **Result** — EC2 instances provisioned + Kubernetes pod Running state confirmed · Passwordless SSH between all 3 nodes · `ansible -m ping` → 0 failures · All manual steps eliminated · Multi-node management via MobaXterm

---

## ⚙️ Complete Tech Stack

| Category | Tools |
|---|---|
| CI/CD & Automation | ![Jenkins](https://img.shields.io/badge/Jenkins-D24939?style=flat&logo=jenkins&logoColor=white) ![Webhooks](https://img.shields.io/badge/GitHub_Webhooks-181717?style=flat&logo=github&logoColor=white) |
| Configuration Management | ![Ansible](https://img.shields.io/badge/Ansible-EE0000?style=flat&logo=ansible&logoColor=white) SSH Key Auth · Ansible Playbooks |
| Container Orchestration | ![Kubernetes](https://img.shields.io/badge/Kubernetes-326CE5?style=flat&logo=kubernetes&logoColor=white) kubectl · Pods · Deployments |
| Cloud Platform | ![AWS](https://img.shields.io/badge/AWS-FF9900?style=flat&logo=amazonaws&logoColor=white) EC2 · IAM · S3 · VPC · CloudWatch |
| Scripting & OS | ![Bash](https://img.shields.io/badge/Bash-4EAA25?style=flat&logo=gnubash&logoColor=white) ![Linux](https://img.shields.io/badge/Linux_Ubuntu-E95420?style=flat&logo=ubuntu&logoColor=white) |
| Version Control | ![Git](https://img.shields.io/badge/Git-F05032?style=flat&logo=git&logoColor=white) ![GitHub](https://img.shields.io/badge/GitHub-181717?style=flat&logo=github&logoColor=white) |
| Networking | SSH · SCP · Passwordless Authentication |
| Tools | MobaXterm · python3-boto |

---

## 📸 Project Screenshots

| Screenshot | Description |
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

**Pushpender Kumar** — B.Tech CSE, GLA University 2024

[![LinkedIn](https://img.shields.io/badge/LinkedIn-0077B5?style=flat&logo=linkedin&logoColor=white)](https://linkedin.com/in/pushpender-kumar-5280b7226)
[![GitHub](https://img.shields.io/badge/GitHub-100000?style=flat&logo=github&logoColor=white)](https://github.com/PushpenderKumar7505)
[![Email](https://img.shields.io/badge/Email-D14836?style=flat&logo=gmail&logoColor=white)](mailto:pushpender7505@gmail.com)
