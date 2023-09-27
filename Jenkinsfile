pipeline {
  agent any

  tools {
    maven 'M2_HOME'
    
    }
  environment {
        AWS_ACCESS_KEY_ID     = credentials('AWS_ACCESS_KEY_ID')
        AWS_SECRET_ACCESS_KEY = credentials('AWS_SECRET_ACCESS_KEY') 
  } 
  
  stages {
   stage('CheckOut') {
      steps {
        echo 'Checkout the source code from GitHub'
        git branch: 'main', url: 'https://github.com/swethaenukonda/minikube-deployment.git'
            }
    }
    
    stage('Package the Application') {
      steps {
        echo " Packaging the Application"
        sh 'mvn clean package'
            }
    }
     stage('Docker Image Creation') {
      steps {
        sh 'docker build -t swethamba859/minikube-deployment:1.0 .'
            }
    }
    stage('DockerLogin') {
      steps {
          withCredentials([usernamePassword(credentialsId: 'docker1hub', passwordVariable: 'docker_password', usernameVariable: 'docker_user')]) {
         sh 'docker login -u ${docker_user} -p ${docker_password}'
            }
        }
    } 
  
    stage('Push Image to DockerHub') {
      steps {
        sh 'docker push swethamba859/minikube-deployment:1.0'
            }
    } 
    stage('Deploying App to Kubernetes') {
      steps {
        script {
          kubernetesDeploy(configs: "deploymentservice.yml", kubeconfigId: "kubernetes")
        }
      }
    }
  }
}
