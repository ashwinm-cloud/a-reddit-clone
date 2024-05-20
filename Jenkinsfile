pipeline {
  agent any
  tools{
    jdk 'jdk17'
    nodejs 'node16'
  }
  environment {
    SCANNER_HOME = tool 'sonar-scanner'
    APP_NAME = "reddit-clone-app"
    RELEASE = "1.0.0"
    DOCKER_USER = "ashwinmreddy"
    DOCKER_PASS = "dockerub"
    IMAGE_NAME = "${DOCKER_USER}" + "/" + "${APP_NAME}"
  }
  stages {
    stage ('clean workspace'){
      steps{
        cleanWs()
      }	      
    }
    stage ('Checkout from Git'){
      steps{	      
          git branch: 'main', url: 'https://github.com/ashwinm-cloud/a-reddit-clone'
      }	      
    }
    stage ('Sonarqube Analysis'){	  
      steps {
        withSonarQubeEnv('SonarQube-Server') {
          sh "${SCANNER_HOME}/bin/sonar-scanner -Dsonar.projectName=reddit-clone-ci -Dsonar.projectKey=reddit-clone-ci"
        }
      }
    }
    stage ('Quality Gate'){
      steps{
        script { 	      
          waitForQualityGate abortPipeline: false, credentialsId: 'SonarQube-Token' 	      
	}
      }
    }
    stage ('Install Dependencies'){
      steps{
        sh "npm install" 	      
      } 	    
    }
    stage ('Trivy FS Scan'){
      steps{
	sh "trivy fs . > trevyfs.txt"      
      }
    }
    

	  
  }	  



}	
