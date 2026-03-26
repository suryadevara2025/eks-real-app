pipeline {
  agent any

  environment {
    AWS_REGION = 'ap-south-1'
    ECR_REPO = 'YOUR_ECR_URI'
  }

  stages {

    stage('Checkout') {
      steps {
        git 'https://github.com/YOUR_USERNAME/eks-real-app.git'
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
