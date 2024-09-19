pipeline {
  environment {
    dockerimagename = "migueldkhub/helloworld"
    dockerImage = ""
  }
  agent any
  stages {
    stage('Checkout Source') {
      steps {
        //git 'https://github.com/MalvarezDKHub/jenkins-kubernetes-deployment.git'
        git branch: 'main', url: 'https://github.com/MalvarezDKHub/jenkins-kubernetes-deployment.git'      }
    }
    stage('Build image') {
      steps{
        script {
          dockerImage = docker.build dockerimagename
        }
      }
    }
    stage('Pushing Image') {
      environment {
          registryCredential = 'dockerhub-credentials'
           }
      steps{
        script {
          docker.withRegistry( 'https://registry.hub.docker.com', registryCredential ) {
            dockerImage.push("latest")
          }
        }
      }
    }
    stage('Deploying webapp container to Kubernetes') {
      steps {
        script {
          kubernetesDeploy(configs: "deployment.yaml", 
                                         "service.yaml")
        }
      }
    }
  }
}