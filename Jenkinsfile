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

        stage('Run Container') {
            steps {
                sh '''
                docker stop devops-container || true
                docker rm devops-container || true
                docker run -d -p 5000:5000 --name devops-container devops-cloud-app
                '''
            }
        }
    }

    post {
        success {
            echo "CI/CD pipeline executed successfully"
        }
        failure {
            echo "Pipeline failed"
        }
    }
}
