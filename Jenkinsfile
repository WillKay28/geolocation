pipeline {
    agent any
    tools {
        maven 'M2_HOME'
    }
    environment {
        registry = '358966077154.dkr.ecr.us-east-1.amazonaws.com/geoloc_ecr_rep'
        registryCredential = 'jenkins-ecr'
        dockerimage = ''
    }
    stages {
        stage('Code Checkout'){
            steps{
                git branch: 'main', url: 'https://github.com/WillKay28/geolocation.git'
            }
        }
        stage('Code Build'){
            steps{
                sh 'mvn clean package'
            }
        }
        stage('Code Test'){
            steps{
                sh 'mvn test'
            }
        }
        stage('Image Build'){
            steps{
                script{
                    dockerImage = docker.build registry + ":$BUILD_NUMBER" .
                }
            }
        }
    }
}