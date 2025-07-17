pipeline {
  agent any

  environment {
    IMAGE_NAME = 'my-app/frontend'
    TAG = "latest"
  }

  stages {
    stage('Checkout') {
      steps {
        git url: 'https://github.com/your-org/frontend-repo.git'
      }
    }

    stage('Build & Push Docker') {
      steps {
        script {
          docker.build("${IMAGE_NAME}:${TAG}").push()
        }
      }
    }

    stage('Deploy to EC2') {
      steps {
        sh '''
        ssh -o StrictHostKeyChecking=no ubuntu@your-ec2-ip <<EOF
          docker pull yourdockerhub/frontend:latest
          docker stop frontend || true && docker rm frontend || true
          docker run -d --name frontend -p 80:80 yourdockerhub/frontend:latest
        EOF
        '''
      }
    }
  }
}
