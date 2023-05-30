pipeline {
    agent any  
    stages {
        stage('Build Docker image') {
            steps {
                script {
                    // Clone the Git repository
                    checkout([$class: 'GitSCM',
                              branches: [[name: 'master']],
                              doGenerateSubmoduleConfigurations: false,
                              extensions: [],
                              submoduleCfg: [],
                              userRemoteConfigs: [[url: 'https://github.com/pkpopin/capstone']]])
                    // Build the Docker image
                    sh "chmod +x build.sh"
                    sh "./build.sh"
                }
            }
        }
     stage('deploy') {
      steps {
        // Checkout the code from GitHub
	sh "chmod +x deploy.sh"
        sh "./deploy.sh"        
      }
    }
      stage('Push') {
      steps {
        // Login to Docker Hub
        withCredentials([usernamePassword(credentialsId: 'docker-hub-credential-id', usernameVariable: 'DOCKERHUB_USERNAME', passwordVariable: 'DOCKERHUB_PASSWORD')]) {
          sh "docker login -u $DOCKERHUB_USERNAME -p $DOCKERHUB_PASSWORD"
        }

        // Push the Docker image to Docker Hub
        sh 'docker tag react-app:latest pkpopin/prod:react-app'
        sh 'docker push pkpopin/prod:react-app'
      }
    }
  }

  post {
    success {
      echo 'Build and push completed successfully!'
    }
    failure {
      echo 'Build and push failed.'
    }
  }
}
   
