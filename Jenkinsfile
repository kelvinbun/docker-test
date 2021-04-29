pipeline {
  environment {
    imagename = "kelvinbun22/php-test"
    registryCredential = 'dockerHub'
    dockerImage = ''
  }
  agent any
  stages {
    stage('Cloning Git') {
      steps {
        git([url: 'https://github.com/kelvinbun/docker-test.git', branch: 'master', credentialsId: 'Github'])

      }
    }
    stage('Building image') {
      steps{
        script {
          dockerImage = docker.build imagename
        }
      }
    }
    stage('Deploy Image') {
      steps{
        script {
          docker.withRegistry( '', registryCredential ) {
            dockerImage.push("$BUILD_NUMBER")
             dockerImage.push('latest')

          }
        }
      }
    }
    stage('Remove Unused docker image') {
      steps{
        sh "docker rmi $imagename:$BUILD_NUMBER"
         sh "docker rmi $imagename:latest"

      }
    }
    stage('Run Docker'){
    steps{
      sh "docker -H ssh://root@192.168.20.178 run -d -p 8080:80 $imagename"
  }
}
