pipeline {
agent any
tools { jdk 'JDK17'; maven 'Maven3' }
stages {
stage('Checkout'){steps{checkout scm}}
stage('Build'){steps{sh 'mvn clean package'}}
stage('Compose Down'){steps{sh 'docker compose down || true'}}
stage('Compose Build'){steps{sh 'docker compose build'}}
stage('Deploy'){steps{sh 'docker compose up -d'}}
}
}
