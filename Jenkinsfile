pipeline {
  agent any
  tools{
    jdk 'jdk17'
    nodejs 'node16'
  }
  environment {
    SCANNR_HOME = tool 'sonar-scanner'
    APP_NAME = "reddit-clone-app"
    RELEASE = "1.0.0"
    DOCKER_USER = "ashwinmreddy"
    DOCKER_PASS = "dockerub"
    IMAGE-NAME = "${DOCKER_USER}" + "/" + "${APP_NAME}"
    IMAGE_TAG = "${RELEASE}-${BUILD_NUMBER}"
    JENKINS_API_TOKEN = credentials("JENKINS_API_TOKEN")	  
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
          sh '''$SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=Reddit-Clone-CI \
          -Dsonar.projectKey=Reddit-Clone-CI'''
        }
      }
    }
    stage ('Quality Gate'){
      steps{
        waitForQualityGate abortPipeline false. credentialsId: 'SonarQube-Token' 	      
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
