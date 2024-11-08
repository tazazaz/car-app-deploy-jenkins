pipeline {
  environment {
    backendRegistry = "manasaid1994/lbg-car-spring-app-started"
    frontendRegistry = "manasaid1994/lbg-car-react-starter"
    registryCredential = "DOCKER_LOGIN"
    dockerImage = ""
    JAVA_BACKEND_REPO = 'https://github.com/tazazaz/lbg-car-spring-app-starter.git'
    REACT_FRONTEND_REPO = 'https://github.com/tazazaz/lbg-car-react-starter.git'
  }
  agent any
  stages {
    
    stage('Checkout Java Backend') {
        steps {
            script {
                // Clone the Java backend repository
                git branch: 'main', url: "${JAVA_BACKEND_REPO}"
            }
        }
    }
    
    stage('Build Java Backend Docker Image') {
        steps {
            script {
                // Build the Docker image for the Java backend
               dockerImage=docker.build(backendRegistry)
            }
        }
    }
    
    stage ("push backend to docker hub") {
      steps {
        script {
          docker.withRegistry('', registryCredential) {
            dockerImage.push("${env.BUILD_NUMBER}")
            dockerImage.push("latest")
          }
        }
      }
    }
    
    stage('Checkout front Backend') {
        steps {
            script {
                // Clone the Java backend repository
                git branch: 'main', url: "${REACT_FRONTEND_REPO}"
            }
        }
    }
    
    stage('Build reac front Docker Image') {
        steps {
            script {
                // Build the Docker image for the Java backend
               dockerImage=docker.build(frontendRegistry)
            }
        }
    }
    
    stage ("push frontend to docker hub") {
      steps {
        script {
          docker.withRegistry('', registryCredential) {
            dockerImage.push("${env.BUILD_NUMBER}")
            dockerImage.push("latest")
          }
        }
      }
    }

    stage("clean up"){
      steps{
        script{
          sh 'docker image prune --all --force --filter "until=48h"'
        }
      }
    }

    

    stage("run services"){
      steps{
        script{
          sh 'docker compose up -d'
        }
      }
    }
    
  }
}
