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
                docker build -t trend-react .
                '''
            }
        }

        stage('Push Image') {
            steps {
                sh '''
                # Login to DockerHub
                echo $DOCKERHUB_CRED_PSW | docker login -u $DOCKERHUB_CRED_USR --password-stdin
                # Tag and push image
                docker tag trend-react $DOCKERHUB_CRED_USR/trend-react:latest
                docker push $DOCKERHUB_CRED_USR/trend-react:latest
                '''
            }
        }

       stage('Deploy to EKS') {
    steps {
        withCredentials([[
            $class: 'AmazonWebServicesCredentialsBinding',
            accessKeyVariable: 'AWS_ACCESS_KEY_ID',
            secretKeyVariable: 'AWS_SECRET_ACCESS_KEY',
            credentialsId: 'aws-tamililan'
        ]]) {
            sh '''
            aws eks update-kubeconfig --region ap-south-1 --name Trend-eks-cluster
            kubectl apply -f deployment.yaml
            kubectl apply -f service.yaml
            '''
        }
    }
}
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
