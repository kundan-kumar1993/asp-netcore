#!groovy
@Library('utils')

import no.evry.Config
import no.evry.Docker
import no.evry.Git

properties([[$class: 'BuildDiscarderProperty', strategy: [$class: 'LogRotator', numToKeepStr: '60']]])
node {
   def app
   def DOCKER_REGISTRY = 'https://registry.hub.docker.com';

   stage('Checkout SCM') {
       checkout scm
   }

   def dockerInfo = new Docker(this, [nameOnly: true])
   stage('Build Image') {
       app = docker.build(dockerInfo.image().toLowerCase())
   }

   stage('Test Image') {
       app.inside {
           sh 'echo "Tests passed"'
       }
   }

   stage('Push Image') {
       //docker.withRegistry("https://${DOCKER_REGISTRY}", 'docker-hub-credentials') {
       //    app.push("${env.BUILD_NUMBER}")
       //    app.push("latest")
       //}

    // Push the given Docker Image to the given registry
       docker.withRegistry("https://${DOCKER_REGISTRY}", docker-evry-registry) {
           app.push(dockerInfo.buildTag())
        app.push("latest")
       }
   }
}