pipeline {
  agent any

  environment {
    DEPLOY_DIR = '/var/www/nextjs-cicd-jenkins'
    NODE_ENV = 'production'
  }

  stages {
    stage('Clone Repo') {
      steps {
        git branch: 'main',  url: 'https://github.com/hms-19/nextjs-cicd-jenkins.git'
      }
    }

    stage('Install Dependencies') {
      steps {
        sh '''
          echo "Checking system environment..."
          echo "Current PATH: $PATH"
          which node
          which npm
          node -v || echo "Node not found"
          npm -v || echo "npm not found"
        '''
        dir("${DEPLOY_DIR}") {
          sh '''
            echo "Running npm install..."
            npm install
            npm install -g pm2
          '''
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

    stage('Start App with PM2') {
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
