pipeline {
  agent any
  environment {
    DOCKER_USERNAME = 'fharvellbiz'
    FRONTEND_IMAGE = 'frontend'
    BACKEND_IMAGE = 'backend'
    REGISTRY_CREDENTIALS = 'docker_login'
  }

  stages {
    stage('Docker install') {
      steps {
        sh 'sudo apt update -y && sudo apt upgrade -y'
        sh 'sudo apt install docker-compose -y'
      }
    }

    stage('Clone repo') {
      steps {
	git branch: 'main', url: "https://github.com/fharvell-biz/my-docker-app.git"
      }
    }

    stage('Build images') {
      steps {
        sh 'docker-compose build'
        sh 'docker tag docker-project-pipeline_client:latest ${DOCKER_USERNAME}/${FRONTEND_IMAGE}:latest'
        sh 'docker tag docker-project-pipeline_server:latest ${DOCKER_USERNAME}/${BACKEND_IMAGE}:latest'
      }
    }

    stage('Push images') {
      steps {
	withDockerRegistry([credentialsId: REGISTRY_CREDENTIALS, url: 'https://index.docker.io/v1/']) {
	  sh 'docker push ${DOCKER_USERNAME}/${FRONTEND_IMAGE}:latest'
	  sh 'docker push ${DOCKER_USERNAME}/${BACKEND_IMAGE}:latest'
	}
      }
    }

    stage('Deploy images') {
      steps {
	sh 'docker-compose up -d'
      }
    }
  }

  post {
    always {
      echo "Pipeline execution completed."
    }
  }
}
