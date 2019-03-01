pipeline {
  agent {
    docker {
      args '-p 3000:3000'
      image 'node:6-alpine'
    }

  }
  stages {
    stage('Build') {
      steps {
        sh 'npm config set unsafe-perm true'
        sh 'npm install'
        sh 'npm install express --save'
      }
    }
    stage('Test') {
      environment {
        CI = 'true'
      }
      steps {
        sh 'npm test'
      }
    }
    stage('Deliver') {
      parallel {
        stage('Deliver') {
          steps {
            sh '''node server.js &
echo $! > .pidfile'''
            input 'Finished using the web site? (Click "Proceed" to continue)'
            sh 'kill $(cat .pidfile)'
          }
        }
      }
    }
  }
}