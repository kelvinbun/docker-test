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
      sshagent (credentials: ['deploy_user']) {
          sh "ssh -vvv -o StrictHostKeyChecking=no -T jenks@192.168.20.178 && docker run --name php -d -p 8080:80 kelvinbun22/php-test"
        }
      }
    }
  }
}
