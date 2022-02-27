pipeline {

  environment {
    dockerimagename = "oyaro/hello"
    dockerImage = ""
    PROJECT_ID = 'upwork-project-001'
    CLUSTER_NAME_TEST = 'upwork-project1-test-cluster'
    CLUSTER_NAME_PROD = 'upwork-project1-prod-cluster'
    LOCATION = 'us-east1-b'
    CREDENTIALS_ID = 'upwork-project-001'
  }

  agent any

  stages {
    
    stage('Checkout') {
      steps{
        checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/Maxwell-Oyaro2/Deploy-to-GCP.git']]])
      }
    }

    stage('Deploy to GKE Test Cluster') {
      steps{
        sh "sed -i 's/prod-app:latest/latest:${env.BUILD_ID}/g' deployment.yaml"
        step([$class: 'KubernetesEngineBuilder', projectId: env.PROJECT_ID, clusterName: env.CLUSTER_NAME_TEST, location: env.LOCATION, manifestPattern: 'deployment.yaml', credentialsId: env.CREDENTIALS_ID, verifyDeployments: false])
      }
    }

    stage('Deploy to GKE production cluster') {
      steps{
        input message:"Proceed with production deployment?"
        step([$class: 'KubernetesEngineBuilder', projectId: env.PROJECT_ID, clusterName: env.CLUSTER_NAME_PROD, location: env.LOCATION, manifestPattern: 'deployment.yaml', credentialsId: env.CREDENTIALS_ID, verifyDeployments: false])
      }
    }
  }
}
