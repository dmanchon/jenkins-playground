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
  - name: busybox
    image: busybox
    command:
    - cat
    tty: true
  - name: gcloud
    image: google/cloud-sdk:latest
    command:
    - cat
    tty: true   
  - name: kaniko
    image: gcr.io/kaniko-project/executor:debug
    command:
    - /busybox/cat
    tty: true
"""
   }
}
    stages {
      
      
       
        stage ('building docker image') {
            when {
                branch 'test1'
            }
            steps {
                dir ('jenkins-app') {
                    container('python-385'){
                        sh 'python --version'
                    }

                    container(name: 'kaniko', shell: '/busybox/sh') {
                        sh 'pwd'
                        sh """
                        #!/busybox/sh 
                        /kaniko/executor --dockerfile Dockerfile --context `pwd`/ --verbosity debug --insecure --skip-tls-verify --destination replacewithregistry:latest
                        """
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
