pipeline {
  agent none

  environment {
    IMAGE = 'node-todo-test:latest'
  }

  stages {

    stage('Checkout Code') {
      agent { label 'node-agent' }
      steps {
        git url: 'https://github.com/aj-c64/node-todo-cicd.git', branch: 'master'
      }
    }

    stage('Build Docker Image') {
      agent {
        docker {
          image 'docker:27-cli'
          args '--network jenkins-net'
          reuseNode true
        }
      }
      environment {
        DOCKER_HOST = 'tcp://dind:2375'
      }
      steps {
        sh '''
          set -eux
          docker version
          docker build . -t ${IMAGE}
          docker images | head -n 5
        '''
      }
    }
  }

  post {
    success { echo 'Docker image built successfully' }
    failure { echo 'Build failed.' }
  }
}
