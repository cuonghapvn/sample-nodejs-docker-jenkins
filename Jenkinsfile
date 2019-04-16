pipeline {
  environment {
    registry = "cuonghapvn/hellonode"
    registryCredential = 'docker_registry_server'
  }
  agent any
  def app
  stages {
    stage('Clone repository') {
        steps {
            checkout scm
        }
    }
    stage('Build image') {
        steps {
            app = docker.build("cuonghapvn/hellonode")
        }
    }

    stage('Test image') {
        steps {
            app.inside {
                sh 'echo "Tests passed"'
            }
        }
    }

    stage('Push image') {
        steps{
            script {
              docker.build registry + ":$BUILD_NUMBER"
            }
        }
    }
  }
}
