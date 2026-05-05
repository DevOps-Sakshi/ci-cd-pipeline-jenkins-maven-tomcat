# End-to-End CI/CD Pipeline for Cab Booking Application using Jenkins, Maven, and Tomcat on AWS

## 1. Overview

This document provides a complete, real-world setup of a CI/CD pipeline using:

* **Maven** → Build tool
* **Jenkins** → Automation server
* **Tomcat** → Application deployment server
* **AWS EC2** → Infrastructure

This setup is commonly used in DevOps environments for automating build and deployment of Java applications.

---

## 2. Architecture Flow

Developer → GitHub → Jenkins → Maven Build → WAR File → Tomcat Deployment

---

## 3. AWS EC2 Instance Creation (Maven + Jenkins Server)

1. Created an EC2 instance named **Maven Server**
2. OS: Amazon Linux / RHEL-based
3. Connected using:

   * AWS SSH OR
   * MobaXterm

Switch to root:

```bash
sudo su -
```

---

## 4. Apache Maven Installation

### 4.1 Download

```bash
cd /opt
wget https://dlcdn.apache.org/maven/maven-3/3.9.15/binaries/apache-maven-3.9.15-bin.tar.gz
```

### 4.2 Extract & Setup

```bash
tar -xvf apache-maven-3.9.15-bin.tar.gz
mv apache-maven-3.9.15 maven
```

---

## 5. Java Installation (Prerequisite)

### Check available versions:

```bash
yum list java*
```

### Install Java 21:

```bash
yum install java-21-openjdk-headless -y
```

---

## 6. Environment Variables Configuration

Edit:

```bash
vi ~/.bash_profile
```

Add:

```bash
JAVA_HOME=/usr/lib/jvm/java-21
M2_HOME=/opt/maven
M2=/opt/maven/bin
PATH=$PATH:$HOME/bin:$JAVA_HOME:$M2_HOME:$M2
```

Apply changes:

```bash
source ~/.bash_profile
```

### Verify:

```bash
echo $JAVA_HOME
mvn -version
```

---

## 7. Jenkins Installation

### 7.1 Add Repository

```bash
wget -O /etc/yum.repos.d/jenkins.repo \
https://pkg.jenkins.io/rpm-stable/jenkins.repo
```

```bash
rpm --import https://pkg.jenkins.io/rpm-stable/jenkins.io-2023.key
```

### 7.2 Install Jenkins

```bash
yum install jenkins -y
```

### 7.3 Start Service

```bash
systemctl start jenkins
systemctl enable jenkins
systemctl status jenkins
```

---

## 8. Jenkins Web Access

Open:

```
http://<EC2-Public-IP>:8080
```

Get password:

```bash
cat /var/lib/jenkins/secrets/initialAdminPassword
```

### Setup:

* Install Suggested Plugins
* Create Admin User

---

## 9. Jenkins Configuration (Important)

### 9.1 Install Plugins

* Maven Integration Plugin
* Git Plugin
* Pipeline Plugin
* Deploy to Container Plugin

### 9.2 Configure Tools

Manage Jenkins → Global Tool Configuration:

* JDK (Java 21)
* Git
* Maven (/opt/maven)

---

## 10. Jenkins Job Creation

### Step 1: Create Job

* New Item → Freestyle Project

### Step 2: Source Code

* Add GitHub repository URL (Cab Booking App - Forked Repo)
* Branch:

```
*/main
```

### Step 3: Build Triggers (Optional)

* Poll SCM
* GitHub webhook

### Step 4: Build Step (Very Important)

```bash
mvn clean package
```

### Output Generated:

```
target/*.war
```

---

## 11. Deployment to Tomcat (CI/CD Integration)

### Using "Deploy to Container" Plugin

Configure in **Post-Build Actions**:

* WAR/EAR Files:

```
target/*.war
```

* Context Path:

```
/cab-app
```

* Tomcat URL:

```
http://<tomcat-ip>:8080
```

---

## 12. Final Output

After successful build and deployment:

Access the application in browser:

```
http://<tomcat-ip>:8080/cab-app
```

✅ Cab Booking Application is successfully deployed and live

---

## 13. Apache Tomcat Installation (Deployment Server)

Apache Tomcat Installation (Deployment Server)

### 11.1 EC2 Setup (Tomcat Server)

1. Created a separate EC2 instance named **Tomcat Server**
2. Connected using SSH / MobaXterm
3. Switch to root:

```bash
sudo su -
```

---

### 11.2 Install Java (Prerequisite)

```bash
yum install java -y
```

Verify:

```bash
java -version
```

---

### 11.3 Download and Install Tomcat

Go to /opt directory:

```bash
cd /opt
```

Download Tomcat:

```bash
wget https://dlcdn.apache.org/tomcat/tomcat-10/v10.1.54/bin/apache-tomcat-10.1.54.tar.gz
```

Extract:

```bash
tar -xvzf apache-tomcat-10.1.54.tar.gz
```

Rename:

```bash
mv apache-tomcat-10.1.54 tomcat
```

---

### 11.4 Start Tomcat Server

```bash
cd /opt/tomcat/bin
sh startup.sh
```

Access in browser:

```
http://<Public-IP>:8080
```

---

### 11.5 Enable Manager & Host Manager Access

By default, Tomcat restricts access. Modify context.xml files:

```bash
vi /opt/tomcat/webapps/manager/META-INF/context.xml
vi /opt/tomcat/webapps/host-manager/META-INF/context.xml
```

Remove or comment the RemoteAddrValve restriction.

---

### 11.6 Create Tomcat User

Edit:

```bash
vi /opt/tomcat/conf/tomcat-users.xml
```

Add:

```xml
<role rolename="manager-gui"/>
<user username="tomcat" password="tomcat" roles="manager-gui"/>
```

---

### 11.7 Restart Tomcat

```bash
cd /opt/tomcat/bin
sh shutdown.sh
sh startup.sh
```

---

### 11.8 Access Manager Application

Open:

```
http://<Public-IP>:8080/manager/html
```

Login using:

* Username: tomcat
* Password: tomcat

---

## 12. CI/CD Flow Explanation (Interview Ready)

CI/CD Flow Explanation (Interview Ready)

1. Developer pushes code to GitHub
2. Jenkins pulls code automatically
3. Maven builds the project and creates WAR file
4. Jenkins deploys WAR to Tomcat
5. Application becomes live

---

## 13. Real-World Enhancements

* Use **separate EC2 instances** for Jenkins and Tomcat
* Store credentials in Jenkins Credentials Manager
* Use **Pipeline as Code (Jenkinsfile)**
* Integrate with Docker & Kubernetes
* Add SonarQube for code quality

---

## 14. Common Errors & Fixes

### Jenkins not opening

* Check port 8080 in Security Group

### Maven not working

* Verify PATH and JAVA_HOME

---
## Application Source  

This project uses a cab booking application forked from an open-source repository.

Original Repo: https://github.com/CloudNinjaa/cab-booking.git

## 16. Conclusion

This project demonstrates practical DevOps skills including:

* Server setup
* Build automation
* CI/CD pipeline creation
* Deployment automation

---


