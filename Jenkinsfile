pipeline {
  agent {
    docker {
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
    stage('Deliver to dev') {
      steps {
        sh '''node server.js &
echo $! > .pidfile'''
        input 'Please, check Your changes on the web http://35.197.102.142:3000 and if all ok Click "Proceed" to continue'
        sh 'kill $(cat .pidfile)'
      }
    }
    stage('deploy to prod') {
      agent {
        docker {
          image 'rhnylyukh/ansible-playbook'
        }

      }
      when {
        branch 'master'
      }
      steps {
        sh 'ansible-playbook /ansible/playbooks/deploy_calculator.yml'
      }
    }
  }
}