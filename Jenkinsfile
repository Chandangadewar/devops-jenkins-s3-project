pipeline {
    agent any

    stages {
        stage('pull') {
            steps {
                git branch: 'main', url: 'https://github.com/Chandangadewar/EasyCRUD-Updated.git'
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
