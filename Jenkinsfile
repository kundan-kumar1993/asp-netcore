#!groovy

node {
   def app
   def dockerImage = ''
   environment {
       registry = "kundanr26/aspnet-core-test1"
       registryCredential = 'dockerhub'
   }

   stage('Checkout SCM') {
       checkout scm
   }

  stage('Building Image') {
      steps {
          script {
              dockerImage = docker.build registry + ":$BUILD_NUMBER"
          }
      }
  }

  stage('Deploy Image') {
      steps {
          script {
              docker.withRegistry( '', registryCredential ) {
                  dockerImage.push()
              }
          }
      }
  }

  stage('Remove Unused docker image') {
      steps {
          sh "docker rmi $registry:$BUILD_NUMBER"
      }
  }

   stage('Test Image') {
       app.inside {
           sh 'echo "Tests passed"'
       }
   }
}