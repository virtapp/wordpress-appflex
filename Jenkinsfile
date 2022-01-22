pipeline {
  environment {
    registry = "global-registry.virtapp.io/library/wordpress-appflex"
    registryCredential = 'harbor-registry'
    dockerImage = ''
  }
  agent any
  stages {
    stage('Cloning Git') {
      steps {
        credentialsId: '14e56be23df2168e6107401e458b93a5225a2116',
        git 'https://github.com/virtapp/wordpress-appflex.git'
      }
    }
    stage('Building image') {
      steps{
        script {
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
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
        sh "docker rmi $registry:$BUILD_NUMBER"
      }
    }
  }
}
