pipeline {
  agent none
  stages {
    stage('Build') {
      steps {
        build 'Hotel'
      }
    }
    stage('test') {
      steps {
        cobertura()
      }
    }
  }
}