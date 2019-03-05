pipeline {
  agent none
  stages {
    stage('Build') {
      agent {
        docker {
          image 'rhnylyukh/slave4'
        }

      }
      steps {
        sh 'npm install'
      }
    }
    stage('Test') {
      parallel {
        stage('Test') {
          agent {
            docker {
              image 'rhnylyukh/slave4'
            }

          }
          environment {
            CI = 'true'
          }
          steps {
            sh 'npm install'
            sh 'npm test'
          }
        }
        stage('Static code analysis') {
          agent {
            docker {
              image 'rhnylyukh/slave4'
            }

          }
          steps {
            sh '''sonar-scanner \\
  -Dsonar.projectKey=calc \\
  -Dsonar.sources=. \\
  -Dsonar.host.url=http:35.199.166.119:9000 \\
  -Dsonar.login=d52870b6c2a71840168f980566a852f8ffdb9e7c'''
          }
        }
      }
    }
    stage('Deliver to dev') {
      agent {
        docker {
          image 'rhnylyukh/slave4'
          args '-p 3000:3000'
        }

      }
      steps {
        sh 'npm install'
        sh '''node server.js &
echo $! > .pidfile'''
        input 'Finished using the web site? (Click "Proceed" to continue)'
        sh 'kill $(cat .pidfile)'
      }
    }
    stage('deploy to prod') {
      agent {
        docker {
          image 'rhnylyukh/ansible-playbook'
        }

      }
      steps {
        sh 'deploy_calculator.yml'
      }
    }
  }
}