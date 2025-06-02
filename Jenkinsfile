pipeline {
  agent any
  
  tools {
    java 'jdk17'
  }
  stages {
    stage('Git Checkout'){
      steps {
        git branch: 'main', url: 'https://github.com/shubham-sihasane/BloggingApp.git'
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
