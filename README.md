# 🚀 DevOps CI/CD Project – Jenkins + AWS S3 (Full Stack Application)

## 📌 Project Overview

This project demonstrates a complete **CI/CD pipeline using Jenkins** for a full-stack application.

The pipeline:

* Pulls code from GitHub
* Builds the backend (Spring Boot)
* Archives the artifact (.jar)
* Uploads the artifact to AWS S3

---

## 🏗️ Tech Stack

* **Frontend**: React (Vite)
* **Backend**: Spring Boot (Java 17)
* **Database**: MariaDB (AWS RDS)
* **CI/CD Tool**: Jenkins
* **Cloud**: AWS (S3, EC2)
* **Build Tool**: Maven

---

## ⚙️ Project Structure

```
devops-jenkins-s3-project/
│
├── backend/         # Spring Boot application
├── frontend/        # React application
├── Jenkinsfile      # CI/CD pipeline
├── README.md
├── .gitignore
```

---

## 🔄 CI/CD Pipeline Flow

1. **Pull Code** from GitHub
2. **Clean Workspace**
3. **Build Backend** using Maven
4. **Archive Artifact (.jar)**
5. **Upload Artifact to AWS S3**

---

## 📦 Jenkins Pipeline

```groovy
pipeline {
    agent any

    stages {
        stage('pull') {
            steps {
                git branch: 'main', url: 'https://github.com/Chandangadewar/devops-jenkins-s3-project.git'
            }
        }

        stage('cleanup') {
            steps {
                sh 'rm -rf backend/target || true'
            }
        }

        stage('build') {
            steps {
                sh '''
                    export JAVA_HOME=/usr/lib/jvm/java-17-openjdk-amd64
                    export PATH=$JAVA_HOME/bin:$PATH
                    cd backend
                    mvn clean package -DskipTests
                '''
            }
        }

        stage('archive') {
            steps {
                archiveArtifacts artifacts: 'backend/target/*.jar', fingerprint: true
            }
        }

        stage('s3upload') {
            steps {
                withAWS(credentials: 'creds', region: 'ap-south-1') {
                    sh '''
                        aws s3 cp backend/target/student-registration-backend-0.0.1-SNAPSHOT.jar s3://my-balti-bucket4321/backend/target/student-registration-backend-0.0.1-SNAPSHOT.jar
                    '''
                }
            }
        }
    }
}
```

---

## ☁️ AWS S3 Artifact Storage

Artifacts are uploaded to:

```
s3://my-balti-bucket4321/backend/target/
```

---

## 🔐 Jenkins Setup

### Required Plugins

* Pipeline: AWS Steps
* AWS Credentials
* Credentials Binding

### AWS Credentials

* Stored securely in Jenkins
* Used via:

```groovy
withAWS(credentials: 'creds', region: 'ap-south-1')
```

---

## 🛠️ Setup Steps

### 1. Install Dependencies

```bash
sudo apt update
sudo apt install -y openjdk-17-jdk maven git unzip curl
```

### 2. Install Jenkins

```bash
sudo apt install jenkins -y
sudo systemctl start jenkins
```

### 3. Install AWS CLI

```bash
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
```

---

## 🗄️ Database Setup (MariaDB)

```bash
sudo apt install mariadb-server -y
mysql -u root -p
```

```sql
CREATE DATABASE student_db;
```

---

## ⚠️ Important Notes

* Do NOT commit:

  * AWS credentials
  * Database passwords
  * `.env` files

* Use placeholders:

```properties
spring.datasource.username=YOUR_DB_USERNAME
spring.datasource.password=YOUR_DB_PASSWORD
```

---

## 🎯 Key Learnings

* CI/CD pipeline creation using Jenkins
* Artifact management in Jenkins
* Secure credential handling
* AWS S3 integration
* Debugging real-world DevOps issues

---

## 🚀 Future Improvements

* Deploy backend from S3 to EC2
* Dockerize application
* Add Kubernetes deployment
* Add CI/CD for frontend

---

## 👨‍💻 Author

**Chandan Gadewar**

---
