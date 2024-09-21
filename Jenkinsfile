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

    //stage('Apply Kubernetes files') {
    //  steps{
    //    withKubeConfig(
    //      caCertificate: '', clusterName: 'minikube', contextName: 'minikube', credentialsId: 'minikube-jenkins-secret',
    //      namespace: 'jenkins', restrictKubeConfigAccess: false, serverUrl: 'https://127.0.0.1:58589') {
    //    //sh 'kubectl apply -f deployment.yaml'
    //    sh 'kubectl version'
    //      } 
    //  }
    //}
    stage('Apply Kubernetes files') {
      steps{
        withKubeConfig(
          caCertificate: '', clusterName: 'dev-proactive', contextName: 'dev-proactive', credentialsId: 'dev-proactive-token',
          namespace: 'jenkins', restrictKubeConfigAccess: false, serverUrl: 'https://56B47FBABCA86EE703CFF27207C63802.gr7.eu-south-2.eks.amazonaws.com') {
        sh 'kubectl apply -f deployment.yaml'
        sh 'kubectl apply -f service.yaml'
        //sh 'kubectl version'
          } 
      }
    }
  } 

}