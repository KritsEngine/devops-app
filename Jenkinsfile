pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Test') {
            steps {
                echo 'GitHub checkout successful'
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t devops-app:v2 .'
            }
        }
    }
}

