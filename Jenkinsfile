pipeline {
    agent any

    stages {

        stage('Clone Code') {
            steps {
                git 'https://github.com/krishnachaitanya90/spring-petclinic-devops.git'
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
                sh './mvnw sonar:sonar'
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t devops-app .'
            }
        }

        stage('Run Container') {
            steps {
                sh 'docker run -d -p 8085:8080 devops-app'
            }
        }

    }
}
