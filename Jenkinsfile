pipeline {
  agent any

  environment {
    DEPLOY_DIR = '/var/www/nextjs-cicd-jenkins'
    NODE_ENV = 'production'
  }

  stages {
    stage('Checkout') {
      steps {
        git branch: 'main', url: 'https://github.com/hms-19/nextjs-cicd-jenkins.git'
      }
    }

    stage('Install & Build') {
      steps {
        sh '''
          echo "Checking node..."
          node -v
          npm -v
          npm install
          npm run build
        '''
      }
    }

    stage('Deploy to /var/www') {
      steps {
        sh '''
          sudo rm -rf /var/www/nextjs-cicd-jenkins
          sudo mkdir -p /var/www/nextjs-cicd-jenkins
          sudo cp -r . /var/www/nextjs-cicd-jenkins
          sudo chown -R jenkins:jenkins /var/www/nextjs-cicd-jenkins
        '''
      }
    }

    stage('Start App with PM2') {
      steps {
        sh '''
          cd /var/www/nextjs-cicd-jenkins
          pm2 describe nextjs-cicd-jenkins > /dev/null
          if [ $? -eq 0 ]; then
            pm2 restart nextjs-cicd-jenkins
          else
            pm2 start npm --name "nextjs-cicd-jenkins" -- start
          fi
        '''
      }
    }
  }
}
