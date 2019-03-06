pipeline {
  agent {
    docker {
      args '-p 3000:3000'
      image 'rhnylyukh/slave4'
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
  -Dsonar.host.url=http:35.197.102.142:9000 \\
  -Dsonar.login=d52870b6c2a71840168f980566a852f8ffdb9e7c'''
          }
        }
      }
    }
    stage('Deploy to dev') {
      steps {
        sh 'node server.js & echo $! > .pidfile'
        input 'Please, check Your changes on the web http://35.197.102.142:3000 and if all ok Click "Proceed" to continue'
        sh 'kill $(cat .pidfile)'
      }
    }
    stage('publish') {
      steps {
        sh 'echo \'//35.197.102.142:8081/repository/npm-repo/:_authToken=NpmToken.9021dc1c-5fd8-3b54-af24-39d2d4b58f5e\' >> $HOME/.npmrc'
        sh 'npm publish'
      }
    }
  }
  environment {
    registryCredential = 'nexus'
  }
}