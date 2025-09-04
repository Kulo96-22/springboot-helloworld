pipeline {
    agent any
    
 
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
                bat 'mvn clean package -DskipTests'
            }
        }
        stage('Build Docker Image') {
            steps {
                bat "docker build -t ${DOCKER_IMAGE}:latest ."
            }
        }
        stage('Push Docker Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds',
                    usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    bat 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                    bat "docker push ${DOCKER_IMAGE}:latest"
                }
            }
        }
        stage('Deploy Container') {
            steps {
                bat "docker run -d -p 8080:8080 ${DOCKER_IMAGE}:latest"
            }
        }
    }
}






