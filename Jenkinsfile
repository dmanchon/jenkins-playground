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
  - name: buildctl
    image: dmanchon/buildctl
    command:
    - cat
    env:
    - name: BUILDKIT_HOST
      value: "tcp://buildkitd.default.svc.cluster.local:1234"
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
          container('buildctl') {
            sh 'pwd'
            sh 'buildctl --version'
          }

          container('python-385'){
            sh 'python --version'
          }

          container('buildctl') {
            sh "buildctl build --frontend dockerfile.v0 --local context=. --local dockerfile=. --output type=image,name=test:one"
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
