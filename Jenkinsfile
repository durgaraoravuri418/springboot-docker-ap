pipeline {
    agent any

    tools {
        maven 'Maven 3.8.1'
    }

    environment {
        IMAGE_NAME = 'springboot-docker-app'
        IMAGE_TAG = 'latest'
        CONTAINER_NAME = 'springboot-app'
        HOST_PORT = '8081'
        CONTAINER_PORT = '8080'
        JAR_NAME = 'springboot-docker-ap-0.0.1-SNAPSHOT.jar' // Update if your jar name changes
    }

    stages {
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
                bat "docker build -t %IMAGE_NAME%:%IMAGE_TAG% ."
            }
        }

        stage('Start or Reuse Container') {
            steps {
                script {
                    def exists = bat(script: "docker ps -a -q -f name=%CONTAINER_NAME%", returnStdout: true).trim()
                    if (exists == "") {
                        // Container doesn't exist — create and run
                        bat "docker run -d -p %HOST_PORT%:%CONTAINER_PORT% --name %CONTAINER_NAME% %IMAGE_NAME%:%IMAGE_TAG%"
                    } else {
                        // Container exists — restart the app inside (using custom logic)
                        echo "Container already exists. Copying new JAR and restarting the app..."
                        bat "docker cp target\\%JAR_NAME% %CONTAINER_NAME%:/app.jar"
                        bat "docker exec %CONTAINER_NAME% bash -c \"pkill -f 'java' || true && nohup java -jar /app.jar > /dev/null 2>&1 &\""
                    }
                }
            }
        }
    }

    post {
        success {
            echo 'Deployment completed with reused container!'
        }
        failure {
            echo 'Build or deployment failed.'
        }
    }
}
