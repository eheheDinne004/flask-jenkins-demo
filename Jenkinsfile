pipeline {
    agent any

    stages {
        stage('Clone Code') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t flask-app:latest .'
                }
            }
        }

        stage('Deploy Container') {
            steps {
                script {
                    try {
                        sh 'docker stop flask-container'
                        sh 'docker rm flask-container'
                    } catch (Exception e) {
                        echo "Khong co container cu dang chay. Tien hanh deploy moi."
                    }
                    sh 'docker run -d -p 5000:5000 --name flask-container flask-app:latest'
                }
            }
        }
    }
}
