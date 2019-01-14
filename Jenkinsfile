pipeline {
  agent none
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
            publishCoverage()
          }
        }
      }
    }
    stage('Clean') {
      steps {
        sh 'echo Clean'
      }
    }
  }
}