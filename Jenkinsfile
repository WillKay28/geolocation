pipeline {
    agent any
    tools {
        maven 'M2_HOME'
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
    }
}