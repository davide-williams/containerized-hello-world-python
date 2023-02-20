pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        echo 'Building'    
        sh 'podman build -f Dockerfile --tag python:knick'
      }
    }
    stage('Test') {
      steps {
        echo 'Testing'
        sh 'podman images'
            }
    }
  }
}