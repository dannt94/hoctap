pipeline {

  environment {
    registry = "dannt94/hoctap"
    registryCredential = 'dockerhub'
    dockerImage = ""
  }

  agent any
  tools {
    git 'git'
    }

  stages {

    stage('Checkout Source') {
      steps {
        sh 'git clone https://github.com/dannt94/hoctap.git'
      }
    }

    stage('Build image') {
      steps{
        script {
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }
      }
    }

    stage('Push Image') {
      steps{
        script {
          docker.withRegistry( "", registryCredential ) {
            dockerImage.push()
          }
        }
      }
    }

    stage('Deploy App') {
      steps {
        script {
          kubernetesDeploy(configs: "myweb.yaml", kubeconfigId: "mykubeconfig")
        }
      }
    }

  }

}
