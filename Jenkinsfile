pipeline {

  environment {
    dockerimagename1 = "kumard31/shopfront"
    dockerImage1 = ""
    dockerimagename2 = "kumard31/stockmanager"
    dockerImage2 = ""
    dockerimagename3 = "kumard31/productcatalogue"
    dockerImage3 = ""
  }

  agent any

  stages {

    stage('Checkout Source') {
      steps {
        git branch: 'main', url: 'https://github.com/kushwaha1980/docker-Java-kubernetes-project.git'
      }
    }

    stage('Build image') {
      steps{
        script {
          sh 'mvn clean install -f $WORKSPACE/shopfront/pom.xml'
          dockerImage1 = docker.build (dockerimagename1, "./shopfront/")
        }
      }
    }

    stage('Pushing Image') {
      environment {
               registryCredential = 'dockerhublogin'
           }
      steps{
        script {
          docker.withRegistry( 'https://registry.hub.docker.com', registryCredential ) {
            dockerImage1.push("latest")
          }
        }
      }
    }

    stage('Deploying App to Kubernetes') {
      steps {
        script {
          sh 'cd kubernetes'
          kubernetesDeploy(configs: "kubernetes/shopfront-service.yaml", kubeconfigId: "kubernetes")
        }
      }
    }

  }

}
