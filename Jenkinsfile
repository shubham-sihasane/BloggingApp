pipeline {
	
  agent any
  
  tools {
    jdk 'jdk17'
    maven 'maven3'
  }

  parameter {
    string (name: 'BRANCH_NAME', defaultValue: 'main', description: 'Select a branch to build and deploy application')
  }
	
  stages {
    stage('Git Checkout'){
      steps {
        git branch: "${params.BRANCH_NAME}", url: 'https://github.com/shubham-sihasane/BloggingApp.git'
      }
    }
    stage('Compile') {
      steps {
        sh 'mvn compile'
	echo "Application successfully compiled."
      }
    }
    stage('Test'){
      steps {
	sh 'mvn test'
	echo "Application testing sucessfully completed."
      }
    }
    stage('Package'){
      steps {
	sh 'mvn package'
        echo "Application packaged successfully."
      }
    }
  }
}
