pipeline {
  agent any
  options {
    buildDiscarder(logRotator(numToKeepStr: '5'))
  }
  environment {
    DOCKERHUB_CREDENTIALS = credentials('amsantana-dockerhub')
  }
  stages {
    stage('Build Application') {
      steps {
        sh 'docker build -t amsantana/ubuntu:latest .'
      }
    }
    stage('Deploy Application') {
      steps {
        sh 'docker run -d -p 80:80 amsantana/ubuntu:latest'
      }
    }
    stage('Connect DockerHub') {
      steps {
        sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
      }
    }
    stage('Push DockerHub') {
      steps {
        sh 'docker push amsantana/ubuntu:latest'
      }
    }
  }
  post {
    always {
      sh 'docker logout'
    }
  }
  }
