pipeline {
    agent any

    stages {

        stage('Clone Code') {
            steps {
                git branch: 'main', url: 'https://github.com/krishnachaitanya90/spring-petclinic-devops.git'
            }
        }

        stage('Build') {
            steps {
                sh 'chmod +x mvnw'
                sh './mvnw clean package'
            }
        }

        stage('SonarQube Scan') {
            steps {
                withSonarQubeEnv('sonarqube') {
                    sh './mvnw sonar:sonar'
                }
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t chaitanya1234567/devops-app:latest .'
            }
        }

        stage('Push Docker Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-hub', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    sh 'echo $PASS | docker login -u $USER --password-stdin'
                    sh 'docker push chaitanya1234567/devops-app:latest'
                }
            }
        }

        stage('Run Container') {
            steps {
                sh 'docker rm -f petclinic || true'
                sh 'docker run -d -p 8085:8080 --name petclinic chaitanya1234567/devops-app:latest'
            }
        }

    }
}
