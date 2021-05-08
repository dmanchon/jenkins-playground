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
    image: agisoft/buildctl
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
            sh 'buildctl build --frontend dockerfile.v0 --local context=. --local dockerfile=.'
          }

          container('python-385'){
            sh 'python --version'
          }

        }
      }
    }

  }
  post {
    success {
      slackSend (
        color: '#00FF00',
        message: "Hurray! CI/CD is *Success* \n*Trigger: * `${env.JOB_NAME}` #${env.BUILD_NUMBER}\n<${env.BUILD_URL}|OPEN JENKINS BUILD>\n*GitHub: * ${GIT_BRANCH} >> <${GIT_URL}|Open Github>"
      )
    }
    failure {
      slackSend (
        color: '#FF0000',
        message: "Oops, something's wrong; CI/CD *Failed* \n*Trigger: * `${env.JOB_NAME}` #${env.BUILD_NUMBER}\n<${env.BUILD_URL}|OPEN JENKINS BUILD>\n*GitHub: * ${GIT_BRANCH} >> <${GIT_URL}|Open Github>"
      )
    }
  }
}
