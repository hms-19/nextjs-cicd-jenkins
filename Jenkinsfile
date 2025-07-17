pipeline {
  agent any

  environment {
    DEPLOY_DIR = '/var/www/nextjs-cicd-jenkins'
    NODE_ENV = 'production'
  }

  stages {
    stage('Clone') {
      steps {
        dir("${DEPLOY_DIR}") {
          git branch: 'main', credentialsId: '2fa0f6c0-f982-4587-b25b-ae609f6bfe5e', url: 'git@github.com:hms-19/nextjs-cicd-jenkins.git'
        }
      }
    }

    stage('Install Dependencies') {
      steps {
        dir("${DEPLOY_DIR}") {
          sh 'npm install'
        }
      }
    }

    stage('Build') {
      steps {
        dir("${DEPLOY_DIR}") {
          sh 'npm run build'
        }
      }
    }

    stage('Start with PM2') {
      steps {
        dir("${DEPLOY_DIR}") {
          sh '''
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
}
