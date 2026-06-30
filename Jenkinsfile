pipeline {
    agent any

    tools {
        jdk 'JDK21'
        maven 'Maven3'
    }

    environment {
        APP_NAME = "springboot-app"
    }

    stages {

        stage('Checkout Source') {
            steps {
                echo "Checking out source code..."
                checkout scm
            }
        }

        stage('Verify Environment') {
            steps {
                sh '''
                echo "===== JAVA VERSION ====="
                java -version

                echo "===== MAVEN VERSION ====="
                mvn -version

                echo "===== DOCKER VERSION ====="
                docker --version

                echo "===== DOCKER COMPOSE VERSION ====="
                docker compose version
                '''
            }
        }

        stage('Build Application') {
            steps {
                echo "Building Spring Boot Application..."
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo "Building Docker Image..."
                sh 'docker compose build'
            }
        }

        stage('Stop Existing Containers') {
            steps {
                sh 'docker compose down || true'
            }
        }

        stage('Deploy Application') {
            steps {
                sh 'docker compose up -d'
            }
        }

        stage('Verify Deployment') {
            steps {
                sh '''
                docker compose ps
                docker ps
                '''
            }
        }
    }

    post {

        success {
            echo "Application deployed successfully."
        }

        failure {
            echo "Pipeline failed."
        }

        always {
            script {
                if (fileExists('docker-compose.yml')) {
                    echo "Workspace contains docker-compose.yml"
                }
            }
        }
    }
}
