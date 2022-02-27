pipeline {

  environment {
    dockerimagename = "oyaro/hello"
    dockerImage = ""
    PROJECT_ID = 'upwork-project-001'
    CLUSTER_NAME_TEST = 'upwork-project1-test-cluster'
    CLUSTER_NAME_PROD = 'upwork-project1-prod-cluster'
    LOCATION = 'us-central1-c'
    CREDENTIALS_ID = 'upwork-project-001'
  }

  agent any

  stages {
    
    stage('Checkout') {
      steps{
        
      }
    }

    stage('Deploy to GKE Test Cluster') {
      steps{
        sh "sed -i 's/prod-app:latest/hello:${env.BUILD_ID}/g' deployment.yaml"
        step([$class: 'KubernetesEngineBuilder', projectId: env.PROJECT_ID, clusterName: env.CLUSTER_NAME_TEST, location: env.LOCATION, manifestPattern: 'deployment.yaml', credentialsId: env.CREDENTIALS_ID, verifyDeployments: true])
      }
    }

    stage('Deploy to GKE production cluster') {
      steps{
        input message:"Proceed with production deployment?"
        step([$class: 'KubernetesEngineBuilder', projectId: env.PROJECT_ID, clusterName: env.CLUSTER_NAME_PROD, location: env.LOCATION, manifestPattern: 'deployment.yaml', credentialsId: env.CREDENTIALS_ID, verifyDeployments: true])
      }
    }
  }
}
