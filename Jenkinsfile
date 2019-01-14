pipeline {
  agent none
  stages {
    stage('Build') {
      steps {
        build 'Hotel'
      }
    }
    stage('script') {
      steps {
        sh 'mvn package'
      }
    }
  }
}