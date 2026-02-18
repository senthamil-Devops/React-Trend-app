pipeline {
  agent any

  environment {
    DOCKERHUB_CRED = credentials('dockerhub')
  }

  stages {
    stage('Checkout') {
      steps {
        git 'https://github.com/senthamil-Devops/React-Trend-app.git'
      }
    }

    stage('Build Docker Image') {
      steps {
        sh 'docker build -t trend-app .'
      }
    }

    stage('Push Image') {
      steps {
        sh '''
        docker tag trend-app $DOCKERHUB_CRED_USR/trend-app:latest
        docker push $DOCKERHUB_CRED_USR/trend-app:latest
        '''
      }
    }

    stage('Deploy to EKS') {
      steps {
        sh '''
        kubectl apply -f deployment.yaml
        kubectl apply -f service.yaml
        '''
      }
    }
  }
}
