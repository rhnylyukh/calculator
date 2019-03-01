pipeline {
  agent {
    docker {
      args '-p 3000:3000'
      image 'rhnylyukh/slave5'
    }

  }
  stages {
    stage('Build') {
      steps {
        sh 'npm install'
      }
    }
    stage('Test') {
      parallel {
        stage('Test') {
          environment {
            CI = 'true'
          }
          steps {
            sh 'npm test'
          }
        }
        stage('Static code analysis') {
          steps {
            sh '''sonar-scanner \\
  -Dsonar.projectKey=calc \\
  -Dsonar.sources=. \\
  -Dsonar.host.url=http:35.203.128.99:9000 \\
  -Dsonar.login=d52870b6c2a71840168f980566a852f8ffdb9e7c'''
          }
        }
      }
    }
    stage('Deliver to dev') {
      steps {
        sh '''node server.js &
echo $! > .pidfile'''
        input 'Finished using the web site? (Click "Proceed" to continue)'
        sh 'kill $(cat .pidfile)'
      }
    }
    stage('v') {
      steps {
        sh 'npm publish'
      }
    }
  }
}