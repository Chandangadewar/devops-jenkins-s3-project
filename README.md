# 🚀 DevOps CI/CD Project – Jenkins + AWS S3 (Full Stack Application)

## 📌 Project Overview

This project demonstrates a complete **end-to-end DevOps workflow** for a full-stack application.

It includes:

* Frontend (React)
* Backend (Spring Boot)
* Database (AWS RDS)
* CI/CD Pipeline using Jenkins
* Artifact storage in AWS S3

The pipeline automates building, packaging, and uploading application artifacts to the cloud.

---

## 🏗️ Tech Stack

* **Frontend**: React (Vite)
* **Backend**: Spring Boot (Java 17)
* **Database**: MariaDB (AWS RDS)
* **CI/CD Tool**: Jenkins
* **Cloud Services**: AWS EC2, AWS S3
* **Build Tool**: Maven

---

## 📂 Project Structure

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

1. Pull source code from GitHub
2. Clean workspace
3. Build backend using Maven
4. Archive generated artifact (.jar)
5. Upload artifact to AWS S3

---

## ⚙️ Jenkins Pipeline

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

Artifacts are stored in:

```
s3://my-balti-bucket4321/backend/target/
```

---

## 🌐 Application Deployment

### Frontend

* Built using React (Vite)
* Deployed on Apache Web Server
* Accessible via:

```
http://<EC2-PUBLIC-IP>
```

### Backend

* Spring Boot application
* Running on EC2 instance
* Port: `8081`

### Database

* AWS RDS (MariaDB)
* Connected to backend using JDBC

---

## 🔄 Complete DevOps Workflow

```
Developer → GitHub → Jenkins → Build → Artifact (.jar) → AWS S3 → Deployment
```

---

## 🛠️ Setup Instructions

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

## 🔐 Jenkins Configuration

### Required Plugins

* Pipeline: AWS Steps
* AWS Credentials
* Credentials Binding

### AWS Credentials Setup

* Add AWS credentials in Jenkins
* Use credential ID: `creds`

---

## 🗄️ Database Setup

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

Use placeholders:

```
spring.datasource.username=YOUR_DB_USERNAME
spring.datasource.password=YOUR_DB_PASSWORD
```

---

## 📸 Suggested Screenshots (for better impact)

* Jenkins pipeline success
* S3 bucket with uploaded artifact
* Running application UI

---

## 🎯 Key Learnings

* CI/CD pipeline implementation using Jenkins
* Artifact management and storage in S3
* Secure credential handling in Jenkins
* Debugging real-world DevOps issues
* Integration of full-stack application with cloud services

---

## 🚀 Future Improvements

* Deploy backend automatically from S3 to EC2
* Dockerize application
* Kubernetes deployment
* CI/CD pipeline for frontend

---

## 👨‍💻 Author

**Chandan Gadewar**
