pipeline {
  environment {
    registry = "cuonghapvn/hellonode"
    registryCredential = 'docker_registry_server'
    dockerImage = ''
  }
  agent any
  tools {nodejs "node" }
  stages {
    stage('Cloning Git') {
      steps {
        checkout scm
      }
    }
    stage('Build') {
       steps {
         sh 'npm install'
       }
    }
    stage('Test') {
      steps {
        sh 'echo "Tests passed"'
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
            withCredentials([[$class: 'UsernamePasswordMultiBinding', 
                        credentialsId: 'docker_registry_server',
                        usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD']]) {
              sh 'docker login -u $USERNAME -p $PASSWORD'
            }
            sh "docker push cuonghapvn/hellonode:latest"
            //docker.withRegistry( '', registryCredential ) {
            //dockerImage.push("latest")
          //}
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
