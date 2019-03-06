pipeline {
  agent {
    docker {
      image 'rhnylyukh/ansible-playbook'
    }

  }
  stages {
    stage('Deploy to prod') {
      steps {
        sh 'ansible-playbook /ansible/playbooks/deploy_calculator.yml'
      }
    }
  }
}