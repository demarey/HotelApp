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
    stage('') {
      steps {
        mail(subject: 'Bonjour', body: 'Comment va?', from: 'yanngarbe@gmail.com', replyTo: 'yanngarbe@gmail.com', to: 'yanngarbe@gmail.com')
      }
    }
  }
}