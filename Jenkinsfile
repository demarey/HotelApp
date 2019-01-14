pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        build 'Hotel'
      }
    }
    stage('Sleep') {
      steps {
        sleep(time: 5, unit: 'NANOSECONDS')
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
            sh 'mvn package'
          }
        }
      }
    }
    stage('Test') {
      steps {
        publishCoverage()
      }
    }
    stage('Clean') {
      steps {
        sh 'mvn clean'
      }
    }
  }
}