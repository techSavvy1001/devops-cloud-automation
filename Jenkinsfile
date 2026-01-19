pipeline {
    agent any

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main',
                url: 'https://github.com/techSavvy1001/devops-cloud-automation.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t devops-cloud-app .'
            }
        }

        stage('Deploy Application') {
            steps {
                sh 'echo "Deploying application"'
            }
        }
    }
}
