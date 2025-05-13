pipeline {
    agent any

    tools {
        maven 'maven3'
        jdk 'JDK17' 
    }
    
    environment {
        IMAGE = "bromaaascripts/banking:${BUILD_NUMBER}"
    }
  
    stages {
        stage('git clone'){
            steps {
                git 'https://github.com/Cloud-savvy/FinPro.git'
            }
        }
        stage('Maven build'){
            steps {
                sh 'mvn clean package'
            }
        }
        stage('Build Docker image'){
            steps {
                sh "docker build -t $IMAGE ."
            }
        }
        stage('Docker login'){
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-cred', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                sh "echo \"\$PASS\" | docker login -u \"\$USER\" --password-stdin"
                }
            }
        }
        stage('Push the image to DockerHub'){
            steps {
                script {
                    sh "docker push $IMAGE"
                    sh "docker tag $IMAGE bromaaascripts/banking:latest"
                    sh "docker push bromaaascripts/banking:latest"
                }
            }
        }
    }
}
