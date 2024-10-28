pipeline {
  environment {
    dockerimagename = "ianwaswa/nodeapp"
    dockerImage = ""
  }

  agent any

  stages {

    stage('Setup Safe Directory') {
      steps {
        // Mark workspace as safe to avoid "dubious ownership" error
        sh 'git config --global --add safe.directory /var/jenkins_home/workspace/webstack_project'
      }
    }

    stage('Checkout Source') {
      steps {
        git 'https://github.com/Ianwaswa/nodeapp_test.git'
      }
    }

    stage('Build image') {
      steps {
        script {
          dockerImage = docker.build dockerimagename
        }
      }
    }

    stage('Pushing Image') {
      environment {
        registryCredential = 'dockerhublogin'
      }
      steps {
        script {
          docker.withRegistry('https://registry.hub.docker.com', registryCredential) {
            dockerImage.push("latest")
          }
        }
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
