pipeline {
  environment {
    DOCKERHUB_CREDENTIALS = credentials('bujaretemi1998-dockerhub')
  }
  stages {
    stage('Build') {
      steps {
        sh 'docker build -t bujaretemi1998/react-test -f ./client/Dockerfile.dev ./client'
      }
    }
    stage('Run') {
      steps {
        sh 'docker run bujaretemi1998/react-test npm test -- --coverage'
      }
    }
    stage('After-Success') {
      steps {
        sh 'docker build -t bujaretemi1998/multi-client ./client'
        sh 'docker build -t bujaretemi1998/multi-nginx ./nginx'
        sh 'docker build -t bujaretemi1998/multi-server ./server'
        sh 'docker build -t bujaretemi1998/multi-worker ./worker'

      }
    }
    stage('Login') {
      steps {
        sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
      }
    }
    stage('Push') {
      steps {
        sh 'docker push bujaretemi1998/multi-client'
        sh 'docker push bujaretemi1998/multi-nginx'
        sh 'docker push bujaretemi1998/multi-server'
        sh 'docker push bujaretemi1998/multi-worker'
      }
    }
  }
  post {
    always {
      sh 'docker logout'
    }
  }
}