pipeline {
  agent any

  stages {
    stage('Build') {
      steps {
        sh 'sudo docker build -t my-flask-app .'
        sh 'sudo docker tag my-flask-app $DOCKER_BFLASK_IMAGE'
      }
    }
    stage('Test') {
      steps {
        sh 'sudo docker run my-flask-app python -m pytest app/tests/'
      }
    }
    stage('Deploy') {
      steps {
        withCredentials([usernamePassword(credentialsId: "${DOCKER_REGISTRY_CREDS}", passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USERNAME')]) {
          sh "sudo echo \$DOCKER_PASSWORD | docker login -u \$DOCKER_USERNAME --password-stdin docker.io"
          sh 'sudo docker push $DOCKER_BFLASK_IMAGE'
        }
      }
    }
  }
  post {
    always {
      sh 'sudo docker logout'
    }
  }
}