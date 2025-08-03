
pipeline {
    agent any

    tools {
        maven 'Maven 3.8.1' // Replace with your installed Maven version in Jenkins
    }

   stages {
        stage('Clone') {
            steps {
                git branch: 'main', url: 'https://github.com/durgaraoravuri418/springboot-docker-ap.git'
            }
        }

        stage('Build with Maven') {
            steps {
                bat 'mvn clean package -DskipTests'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat 'docker build -t springboot-docker-app .'
            }
        }

        stage('Run Docker Container') {
            steps {
                bat 'docker run -d -p 8080:8080 --name springboot-app springboot-docker-app'
            }
        }
    }
}
