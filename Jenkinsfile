pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        echo 'Building'    
        sh 'podman build -f Dockerfile --tag default-route-openshift-image-registry.apps.tz-206240.cecc.ihost.com/python/python:latest'
      }
    }
    stage('Verification'){
        steps{
            echo 'Verifying image was created successfully'
            sh 'podman images'
        }
    }
    stage('Authenticating with Registry'){
        steps{
            echo 'Logging into Openshift Image Registry'
            sh 'podman login -u $(oc whoami) -p $(oc whoami -t)'
        }
    }
    stage('Pushing') {
      steps {
        echo 'Pushing'
        sh 'podman push default-route-openshift-image-registry.apps.tz-206240.cecc.ihost.com/python/python:latest --tls-verify=false --cert $(oc whoami):$(oc whoami -t)'
            }
    }
  }
}