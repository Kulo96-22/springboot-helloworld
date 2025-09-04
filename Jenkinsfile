pipeline {
    agent any
    tools {
        maven 'Maven_3.9'
        jdk 'JDK_21'
    }
    environment {
        DOCKER_IMAGE = "kulochana/springboot-helloworld"
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Kulo96-22/springboot-helloworld.git'
            }
        }
        stage('Build with Maven') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${DOCKER_IMAGE}:latest ."
            }
        }
        stage('Push Docker Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds',
                    usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                    sh "docker push ${DOCKER_IMAGE}:latest"
                }
            }
        }
        stage('Deploy Container') {
            steps {
                sh "docker run -d -p 8080:8080 ${DOCKER_IMAGE}:latest"
            }
        }
    }
}

