pipeline {
  agent any
  stages {
    stage('Verify Docker Access') {
      steps {
        sh 'docker run --rm hello-world'
      }
    }
  }
}
