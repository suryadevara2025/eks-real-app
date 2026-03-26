pipeline {
  agent any

  environment {
    AWS_REGION = 'ap-south-1'
    ECR_REPO = '202264954934.dkr.ecr.ap-south-1.amazonaws.com/eks-real-app'
  }

  stages {

    stage('Checkout') {
      steps {
        git 'https://github.com/suryadevara2025/eks-real-app.git'
      }
    }

    stage('Build Docker Image') {
      steps {
        sh 'docker build -t $ECR_REPO:latest .'
      }
    }

    stage('Login to ECR') {
      steps {
        sh '''
        aws ecr get-login-password --region $AWS_REGION | \
        docker login --username AWS --password-stdin $ECR_REPO
        '''
      }
    }

    stage('Push Image') {
      steps {
        sh 'docker push $ECR_REPO:latest'
      }
    }

    stage('Deploy to EKS') {
      steps {
        sh '''
        sed -i "s|REPLACE_WITH_ECR_URI|$ECR_REPO|g" k8s/deployment.yaml
        kubectl apply -f k8s/
        '''
      }
    }

  }
}
