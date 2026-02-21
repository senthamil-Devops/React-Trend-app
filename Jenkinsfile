pipeline {
    agent any

    environment {
        DOCKERHUB_CRED = credentials('dockerhub') // your DockerHub credentials
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/senthamil-Devops/React-Trend-app.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                # Build Docker image from Dockerfile
                docker build -t trend-frontend .
                '''
            }
        }

        stage('Push Image') {
            steps {
                sh '''
                # Login to DockerHub
                echo $DOCKERHUB_CRED_PSW | docker login -u $DOCKERHUB_CRED_USR --password-stdin
                # Tag and push image
                docker tag trend-application $DOCKERHUB_CRED_USR/tamililan/trend-react
                docker push $DOCKERHUB_CRED_USR/tamililan/trend-react
                '''
            }
        }

        stage('Deploy to EKS') {
            steps {
                sh '''
                # Apply Kubernetes manifests
                kubectl apply -f deployment.yaml
                kubectl apply -f service.yaml
                '''
            }
        }
    }

    post {
        always {
            echo 'Pipeline finished.'
        }
        success {
            echo 'Deployment succeeded!'
        }
        failure {
            echo 'Deployment failed!'
        }
    }
}
