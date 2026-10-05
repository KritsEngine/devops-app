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

        stage('Docker Push') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-credentials',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh '''
                        echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                        docker tag devops-app:v2 $DOCKER_USER/devops-app:v2
                        docker push $DOCKER_USER/devops-app:v2
                    '''
                }
            }
        }

        stage('Kubernetes Deploy') {
            steps {
                sh '''
                    kubectl set image deployment/devops-app \
                    devops-app=kriteshdoker/devops-app:v2
                    kubectl rollout status deployment/devops-app
                '''
            }
        }
    }
}

