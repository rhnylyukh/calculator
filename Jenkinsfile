pipeline {
  agent {
    docker {
      image 'node:6-alpine'
      args '-p 3000:3000'
    }

  }
  stages {
    stage('Build') {
      steps {
        sh 'npm install'
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
      steps {
        sh 'node server.js &'
        input 'Finished using the web site? (Click "Proceed" to continue)'
        sh 'kill $(cat .pidfile)'
      }
    }
  }
}