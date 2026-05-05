# CI/CD Pipeline Implementation using Jenkins, Docker, Ansible and AWS

---

## 1. Project Overview

This project implements an end-to-end CI/CD pipeline to automate the process of building, testing, analyzing, containerizing, and deploying a web application.

The pipeline is automatically triggered when code is pushed to the GitHub repository and completes deployment on an AWS EC2 instance.

---

## 2. Pipeline Workflow

GitHub → Jenkins → Maven Build + JUnit Test → SonarQube → Docker → Trivy → DockerHub → Ansible → AWS EC2

---

## 3. System Setup

### 3.1 Control Node (Jenkins Server)

The following tools are installed on the Jenkins server:

- Jenkins (Pipeline execution)
- Docker (Image creation)
- Maven (Build tool)
- Trivy (Security scanning)
- Ansible (Deployment automation)

---

### 3.2 Target Node (AWS EC2)

- Docker installed
- Application deployed as container

---

## 4. Jenkins Pipeline Configuration

- A Pipeline job is created in Jenkins  
- Connected to GitHub using **Pipeline Script from SCM**  
- Trigger configured using **Poll SCM**  
- Pipeline logic defined in `Jenkinsfile`  

---

## 5. Pipeline Execution Stages

All the following stages are executed automatically by Jenkins.

---

### 5.1 Source Code Integration

- Developer pushes code to GitHub  
- Jenkins detects changes and triggers pipeline  

---

### 5.2 Build and Unit Testing

**Command executed:**
  mvn clean package

**Explanation:**
- Compiles the application  
- Resolves dependencies  
- Executes JUnit test cases  
- Stops pipeline if tests fail  

---

### 5.3 Code Quality Analysis

**Command executed:**
mvn sonar:sonar

**Explanation:**
- Analyzes code using SonarQube  
- Detects bugs and vulnerabilities  
- Ensures code quality  

---

### 5.4 Docker Image Creation (Automated)

**Command executed:**
docker build -t <dockerhub-username>/app .

**Explanation:**
- Builds Docker image using Dockerfile  
- Packages application with dependencies  

---

### 5.5 Security Scanning (Trivy)

**Command executed:**
trivy image <dockerhub-username>/app

**Explanation:**
- Scans image for vulnerabilities  
- Ensures secure deployment  

---

### 5.6 Push Image to DockerHub

**Commands executed:**
docker login
docker push <dockerhub-username>/app

**Explanation:**
- Uploads image to DockerHub  
- Makes image available for deployment  

---

## 6. Deployment using Ansible

Deployment is performed from the Jenkins server to AWS EC2.

---

### 6.1 Inventory Configuration

File:ansible/inventory

Add:<EC2-IP> ansible_user=ubuntu ansible_ssh_private_key_file=key.pem

---

### 6.2 Run Playbook

ansible-playbook -i ansible/inventory ansible/playbook.yml

---

### 6.3 Deployment Actions

- Connects to EC2 instance  
- Pulls Docker image from DockerHub  
- Runs container  

---

## 7. Application Access

After deployment:
http://<EC2-PUBLIC-IP>


---

## 8. Screenshots (Proof of Implementation)

- Jenkins pipeline execution  
- SonarQube analysis dashboard  
- Trivy vulnerability scan  
- DockerHub repository  
- Docker container running on EC2 (`docker ps`)  
- Application running in browser  

---

## 9. Result

- CI/CD pipeline executed successfully  
- Build, testing, and deployment automated  
- Application deployed on AWS EC2  
- Application accessible via browser  

---

## 10. Conclusion

This project successfully demonstrates a complete CI/CD pipeline that automates the software delivery process. It reduces manual effort, improves reliability, and ensures consistent deployment.

---
