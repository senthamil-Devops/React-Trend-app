pipeline {
  agent any

  environment {
    DOCKERHUB_CRED = credentials('dockerhub')
  }

  stages {

    stage('Checkout') {
      steps {
        git branch: 'main', url: 'https://github.com/senthamil-Devops/React-Trend-app.git'
      }
    }

    stage('Build React App') {
      steps {
        sh '''
        npm install
        npm run build
        '''
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
        echo $DOCKERHUB_CRED_PSW | docker login -u $DOCKERHUB_CRED_USR --password-stdin
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
