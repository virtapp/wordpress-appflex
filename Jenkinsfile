pipeline {
  environment {
    registry = "registry.hub.docker.com/virtapp/wordpress-appflex"
    registryCredential = 'docker-hub-credentials'
    dockerImage = ''
  }
  agent any
  stages {
    stage('Cloning Git') {
      steps {
        git 'https://github.com/virtapp/wordpress-appflex.git'
      }
    }
    stage('Building image') {
      steps{
        script {
          dockerImage = docker.build registry + ":0.$BUILD_NUMBER"
        }
      }
    }
    stage('Deploy Image') {
      steps{
        script {
            docker.withRegistry( 'https://' + registry, registryCredential ) {
            dockerImage.push()
          }
        }
      }
    }
    stage('Remove Unused docker image') {
      steps{
        sh "docker rmi $registry:0.$BUILD_NUMBER"
      }
    }
  }
}
