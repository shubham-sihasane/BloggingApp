pipeline {
    agent any

    tools {
        jdk 'jdk17'
        maven 'maven3'
    }
    
    environment {
        SCANNER_HOME = tool 'sonarscanner'
        APP_NAME = "BloggingApp"
    }
    
    stages {
        stage('Git Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/shubham-sihasane/BloggingApp.git'
            }
        }
        stage('Trivy Scan') {
            steps {
                sh "trivy fs --format table -o trivy-scan-report.txt ."
            }
        }
        stage('Compile') {
            steps {
                sh 'mvn compile'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }
        stage('Package') {
            steps {
                sh 'mvn clean package'
            }
        }
        // stage('Sonarqube Analysis') {
        //     steps {
        //       withSonarQubeEnv('sonarserver') {
        //           sh '''
        //                 $SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=$APP_NAME -Dsonar.projectKey=$APP_NAME -Dsonar.java.binaries=.
        //                 echo $SCANNER_HOME
        //           '''
        //         }
        //     }
        // }
        // stage("Quality Gate") {
        //     steps {
        //       timeout(time: 1, unit: 'HOURS') {
        //         waitForQualityGate abortPipeline: false
        //       }
        //     }
        // }
        // stage('Publish Artifact') {
        //     steps {
        //         withMaven(globalMavenSettingsConfig: '', jdk: '', maven: '', mavenSettingsConfig: 'MavenSettings', traceability: true) {
        //             sh 'mvn deploy'    
        //         }
        //     }
        // }
        stage('Build Image') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'DockerRegistry') {
                        sh 'docker build -t sihasaneshubham/blogapp:v1 -f Dockerfile .'
                        sh 'docker image tag sihasaneshubham/blogapp:v1 sihasaneshubham/blogapp:latest'
                    }
                }
            }
        }
        stage('Docker Image Scan') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'DockerRegistry') {
                        sh 'trivy image --format table -o image-scan.txt sihasaneshubham/blogapp:v1'
                        sh 'trivy image --format table -o image-scan.txt sihasaneshubham/blogapp:latest'
                    }
                }
            }
        }
        stage('Push Image') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'DockerRegistry') {
                        sh 'docker push sihasaneshubham/blogapp:v1'
                        sh 'docker push sihasaneshubham/blogapp:latest'
                    }
                }
            }
        }
    }
}
