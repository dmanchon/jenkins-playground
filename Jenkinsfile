pipeline {
  agent {
    kubernetes {
      defaultContainer 'jnlp'
      label 'dani'
      yaml """
apiVersion: v1
kind: Pod
metadata:
  labels:
      run: jnlp
spec:
  containers:
  - name: git
    image: gcr.io/cloud-builders/git
    command:
    - cat
    tty: true
  - name: python-385
    image: registry.hub.docker.com/library/python:3.8.5
    command:
    - cat
    tty: true
"""
    }
  }
  stages {
    stage ('building docker image') {
      when {
        branch 'test2'
      }
      steps {
        dir ('app') {


          container('python-385'){
            sh 'python --version'
          }




        }
      }
    }

  }
  post {
    success {
      echo 'Hurray!'
    }
    failure {
      echo 'Wowwowwow!'
    }
  }
}
