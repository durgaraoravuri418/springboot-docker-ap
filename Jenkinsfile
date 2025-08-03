pipeline {
    agent any

    tools {
        maven 'Maven 3.8.1' // Make sure this matches what you configured in Jenkins
    }

    environment {
        IMAGE_NAME = 'springboot-docker-app'
        CONTAINER_NAME = 'springboot-app'
        HOST_PORT = '8081'
        CONTAINER_PORT = '8080'
    }

  stage('Checkout Code') {
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
                bat "docker build -t %IMAGE_NAME% ."
            }
        }

        stage('Remove Old Container if Exists') {
            steps {
                // Force remove container if it exists (ignore error if not)
                bat "docker rm -f %CONTAINER_NAME% || echo No old container to remove"
            }
        }

        stage('Run New Container') {
            steps {
                bat "docker run -d -p %HOST_PORT%:%CONTAINER_PORT% --name %CONTAINER_NAME% %IMAGE_NAME%"
            }
        }
    }

    post {
        success {
            echo '✅ Deployment successful!'
        }
        failure {
            echo '❌ Build or Deployment failed.'
        }
    }
}
