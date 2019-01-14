pipeline {
  agent {
    node {
      label 'Hotel'
    }

  }
  stages {
    stage('Build') {
      steps {
        build 'Hotel'
      }
    }
    stage('Sleep') {
      steps {
        sleep 5
      }
    }
    stage('Salutations') {
      parallel {
        stage('Salutations') {
          steps {
            echo 'Bonjour'
          }
        }
        stage('Script Package') {
          steps {
            powershell 'mvn package'
          }
        }
      }
    }
    stage('Clean') {
      steps {
        powershell 'mvn clean'
      }
    }
  }
}