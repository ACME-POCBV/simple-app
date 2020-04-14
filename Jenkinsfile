pipeline {
    agent {
        label 'ubuntu-1604'
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/kidh0/dead-inside.git'
            }
        }
        stage('Build') {
            steps {
                sh "docker build -t gcr.io/bv-poc-cd/simple-app:${BUILD_NUMBER} ."
            }
        }
        stage('Push') {
            steps {
                withDockerRegistry(credentialsId: 'gcr:bv-poc-cd', url: 'https://gcr.io/bv-poc-cd/simple-app') {
                   sh "docker push gcr.io/bv-poc-cd/simple-app:${BUILD_NUMBER}"
                }
            }
        }
        stage('Update property file') {
            steps {
                sh "echo 'IMAGE_TAG=release-${BUILD_NUMBER}' > spinnaker.properties'
            }
        }
    }
    post {
      always {
          archiveArtifacts artifacts: 'spinnaker.properties', onlyIfSuccessful: true
      }
  }
}
