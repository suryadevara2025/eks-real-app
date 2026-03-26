pipeline {
  agent any

  environment {
    AWS_REGION = 'ap-south-1'
    ECR_REPO = '202264954934.dkr.ecr.ap-south-1.amazonaws.com/eks-real-app'
  }

  stages {

    stage('Checkout') {
      steps {
        git branch: 'main', url: 'https://github.com/suryadevara2025/eks-real-app.git'
      }
    }

    stage('Build Docker Image') {
      steps {
        sh 'docker build -t 202264954934.dkr.ecr.ap-south-1.amazonaws.com/eks-real-app:latest .'
      }
    }

    stage('Login to ECR') {
      steps {
        sh '''
        aws ecr get-login-password --region ap-south-1 | \
        docker login --username AWS --password-stdin 202264954934.dkr.ecr.ap-south-1.amazonaws.com/eks-real-app
        '''
      }
    }

    stage('Push Image') {
      steps {
        sh 'docker push 202264954934.dkr.ecr.ap-south-1.amazonaws.com/eks-real-app:latest'
      }
    }

    stage('Deploy to EKS') {
      steps {
        sh '''
        sed -i "s202264954934.dkr.ecr.ap-south-1.amazonaws.com/eks-real-appg" k8s/deployment.yaml
        kubectl apply -f k8s/
        '''
      }
    }

  }
}
