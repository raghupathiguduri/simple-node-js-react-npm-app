pipeline {
    agent {
kubernetes {
      defaultContainer 'jnlp'
      yaml """
apiVersion: v1
kind: Pod
metadata:
labels:
  component: ci
spec:
  # Use service account that can deploy to all namespaces
  serviceAccountName: jenkins-admin
  containers:
  - name: nodejs
    image: node:6-alpine
    command:
    - cat
    tty: true
  - name: gcloud
    image: gcr.io/cloud-builders/gcloud
    command:
    - cat
    tty: true
  - name: kubectl
    image: gcr.io/cloud-builders/kubectl
    command:
    - cat
    tty: true
"""
}
    }
    environment { 
        CI = 'true'
    }
    stages {
        stage('Build') {
            steps {
              container('gcloud') {
	        sh 'gcloud builds submit -t test:v1 '
            }
        }
        stage('Deliver') { 
            steps {
              container('kubectl') {
	        sh 'kubectl apply --image=test:v1 --replicas=2'
            }
        }
    }
}
