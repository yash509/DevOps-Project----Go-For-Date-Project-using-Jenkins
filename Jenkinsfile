pipeline {
    agent any
    
    environment {
        SCANNER_HOME= tool 'sonar-scanner'
    }

    stages {
        stage('Git Checkout') {
            steps {
                git branch: 'main', credentialsId: 'git-token', url: 'https://github.com/yash509/DevOps-Project----Go-For-Date-Project-using-Jenkins.git'
            }
        }
        
        stage('Trivy FileSystem Scanning') {
            steps {
                sh "trivy fs --format table -o trivy-fs-report.html ."
            }
        }
        
        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('sonar') {
                    sh "$SCANNER_HOME/bin/sonar-scanner -Dsonar.projectKey=Want-to-come-for-Date? -Dsonar.projectName=Want-to-come-for-Date?"
                }
            }
        }
        
        stage('Build & Tag Docker Image') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker', toolName: 'docker') {
                        sh "docker build -t yash5090/Want-to-come-for-Date?:v1 ."
                    }
                }
            }
        }
        
        stage('Trivy Image Scanning') {
            steps {
                sh "trivy image --format json -o trivy-image-report.json yash5090/Want-to-come-for-Date?:v1"
            }
        }
        
        stage('Push Image to DockerHub') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker', toolName: 'docker') {
                        sh "docker push yash5090/Want-to-come-for-Date?:v1"
                    }
                }
            }
        }
        
        stage('Deploy to Container') {
            steps {
                sh "docker run -d -p 8081:80 yash5090/Want-to-come-for-Date?:v1"
            }
        }
    }
}
