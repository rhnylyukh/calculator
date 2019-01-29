pipeline {
  agent {
    docker {
      args '-p 3000:3000'
      image 'rhnylyukh/jenkins-agent'
    }

  }
  stages {
    stage('Build') {
      steps {
        sh 'npm install'
        withSonarQubeEnv('tst') {
          sh '''$HOME/sonar-scanner-cli-3.3.0.1492-linux/bin/sonar-scanner \\
  -Dsonar.projectKey=calc \\
  -Dsonar.sources=. \\
  -Dsonar.host.url=http://35.247.94.233:9000 \\
  -Dsonar.login=d52870b6c2a71840168f980566a852f8ffdb9e7c'''
        }

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
        sh '''node server.js &
echo $! > .pidfile'''
        input 'Finished using the web site? (Click "Proceed" to continue)'
        sh 'kill $(cat .pidfile)'
      }
    }
  }
}