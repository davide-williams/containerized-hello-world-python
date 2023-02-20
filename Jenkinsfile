pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        echo 'Building'    
        sh 'podman build -f Dockerfile --tag image-registry.openshift-image-registry.svc:5000/python/python:latest'
      }
    }
    stage('Authentication to Registry'){
        steps{
            echo 'Authenticating to Openshift Registry'
            sh 'podman login -u jenkins -p $(oc whoami -t) image-registry.openshift-image-registry.svc:5000 --tls-verify=false'
        }
    }
    stage('Pushing') {
      steps {
        echo 'Pushing'
        sh 'podman push image-registry.openshift-image-registry.svc:5000/python/python:latest --tls-verify=false'
            }
    }
    stage('Creating application'){
        steps{
            echo 'Creating application in namespace Python'
            sh 'oc new-app image-registry.openshift-image-registry.svc:5000/python/python:latest -n python'
        }
    }
  }
}