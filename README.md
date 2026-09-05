# MLOps Production-Ready Machine Learning Project

A production-ready machine learning project demonstrating an end-to-end MLOps workflow, including data management, model development, MongoDB integration, AWS deployment, Docker, ECR, EC2, and GitHub Actions.

---

## 🛠️ Tools & Resources

| Tool       | Link                                                    |
| ---------- | ------------------------------------------------------- |
| Anaconda   | https://www.anaconda.com/                               |
| VS Code    | https://code.visualstudio.com/download                  |
| Git        | https://git-scm.com/                                    |
| Flowchart  | https://whimsical.com/                                  |
| MLOps Tool | https://www.evidentlyai.com/                            |
| MongoDB    | https://account.mongodb.com/account/login               |
| Dataset    | https://www.kaggle.com/datasets/moro23/easyvisa-dataset |

---

## 📂 Project Workflow

The project follows the following workflow:

1. **Constants**
2. **Entity**
3. **Components**
4. **Pipeline**
5. **Main File**

---

## 🔀 Git Commands

Use the following commands to add, commit, and push changes to the repository:

```bash
git add .

git commit -m "Updated"

git push origin main
```

---

# 🚀 How to Run?

## 1. Create Conda Environment

```bash
conda create -n visa python=3.8 -y
```

## 2. Activate Conda Environment

```bash
conda activate visa
```

## 3. Install Dependencies

```bash
pip install -r requirements.txt
```

---

# 🔐 Export Environment Variables

Set the required environment variables before running the project.

```bash
export MONGODB_URL="mongodb+srv://<username>:<password>...."

export AWS_ACCESS_KEY_ID=<AWS_ACCESS_KEY_ID>

export AWS_SECRET_ACCESS_KEY=<AWS_SECRET_ACCESS_KEY>
```

---

# ☁️ AWS CI/CD Deployment with GitHub Actions

This section describes the deployment process using AWS, Docker, Amazon ECR, Amazon EC2, and GitHub Actions.

---

## 1. Login to AWS Console

Login to the AWS Console.

---

## 2. Create IAM User for Deployment

Create an IAM user with the required permissions for deployment.

### Required Access

```bash
1. EC2 access : It is virtual machine

2. ECR: Elastic Container registry to save your docker image in aws
```

### Deployment Process

```bash
1. Build docker image of the source code

2. Push your docker image to ECR

3. Launch Your EC2

4. Pull Your image from ECR in EC2

5. Lauch your docker image in EC2
```

### Required Policies

```bash
1. AmazonEC2ContainerRegistryFullAccess

2. AmazonEC2FullAccess
```

---

## 3. Create ECR Repository

Create an ECR repository to store/save the Docker image.

### ECR Repository URI

Save the following URI:

```bash
891376917319.dkr.ecr.ap-south-1.amazonaws.com/visa
```

---

## 4. Create EC2 Machine

Create an **EC2 machine using Ubuntu**.

---

## 5. Open EC2 and Install Docker

### Optional

```bash
sudo apt-get update -y

sudo apt-get upgrade
```

### Required

```bash
curl -fsSL https://get.docker.com -o get-docker.sh

sudo sh get-docker.sh

sudo usermod -aG docker ubuntu

newgrp docker
```

---

## 6. Configure EC2 as Self-Hosted Runner

Go to:

```text
Settings > Actions > Runner > New self hosted runner
```

Choose the appropriate operating system and then run the commands one by one on the EC2 machine.

---

## 7. Setup GitHub Secrets

Add the following secrets to your GitHub repository:

```bash
AWS_ACCESS_KEY_ID
AWS_SECRET_ACCESS_KEY
AWS_DEFAULT_REGION
ECR_REPO
```

---

# 🔄 Deployment Workflow

```text
Source Code
     |
     v
GitHub Repository
     |
     v
GitHub Actions
     |
     v
Build Docker Image
     |
     v
Push Docker Image to ECR
     |
     v
Amazon EC2
     |
     v
Pull Image from ECR
     |
     v
Run Docker Container
```

---

# 📌 Project Summary

This project demonstrates a complete production-oriented machine learning workflow, including:

* Machine Learning project structure
* MongoDB integration
* MLOps workflow
* Docker containerization
* Amazon ECR for Docker image storage
* Amazon EC2 for deployment
* GitHub Actions for CI/CD
* GitHub Actions self-hosted runner
* AWS IAM permissions
* Environment variable configuration
