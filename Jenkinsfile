pipeline {
    agent any
    tools {
        maven 'M2_HOME'
    }
    environment {
        registry = '358966077154.dkr.ecr.us-east-1.amazonaws.com/geoloc_ecr_rep'
        //registryCredential = 'ecr:us-east-1:awscreds'
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
                    dockerImage = docker.build registry
                }
            }
        }
        stage('Push Image'){
            steps{
                script{
                    sh 'aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 358966077154.dkr.ecr.us-east-1.amazonaws.com'
                    sh 'docker push 358966077154.dkr.ecr.us-east-1.amazonaws.com/geoloc_ecr_rep:latest'
                }
            }
        }
        stage('K8s Deploy'){
            steps{
                script{
                    withKubeConfig(caCertificate: 'LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSURCVENDQWUyZ0F3SUJBZ0lJZlhsUVMwaG00dG93RFFZSktvWklodmNOQVFFTEJRQXdGVEVUTUJFR0ExVUUKQXhNS2EzVmlaWEp1WlhSbGN6QWVGdzB5TXpFeU1URXlNVEEwTXpoYUZ3MHpNekV5TURneU1UQTVNemhhTUJVeApFekFSQmdOVkJBTVRDbXQxWW1WeWJtVjBaWE13Z2dFaU1BMEdDU3FHU0liM0RRRUJBUVVBQTRJQkR3QXdnZ0VLCkFvSUJBUURkR3VMYzhDa21qcC9UdG5CalgwRU1XTlZpT3IxZjFOWlRyRmkrek0rNWNZdXlxYkozRXFoU1lCUXYKenc0NktXRVkrUk52a011OFVrL2MyeXRJYXhUenIrUEErRjZHVlg0M0l5M0NQZWlVd241SVkzdVFwTU9JZHNYVgo1NlVSY0ZVRENWTXN5QS9wSXM5RThWckNOWVcyd1dEczRTRjVGZE56aWVCVCtlUXI1VWVyaDNkSUZWeVNiekZFCmdtTG9vKzVoa1dIT0pFR0QvYlBGSGV6aG8rTlR3aU1LcFJvU0cyazNtbjBDZmkxZUhhaGloM1MwZWNwaGc4VDUKQlA2NTdKOWVWZEJkRG9iaGlCYlNIN29JSEhaT3E5cnFscG54UTNaZ0RSQ283N0I4dDlQNXFRUmt6TlY3MUtYZgpyVWJXZkpzYmxjRHE1eTFRbUNrZTlReUM4YW9mQWdNQkFBR2pXVEJYTUE0R0ExVWREd0VCL3dRRUF3SUNwREFQCkJnTlZIUk1CQWY4RUJUQURBUUgvTUIwR0ExVWREZ1FXQkJScnA4ME53aG9KQ1hhOWQvbkM3cFhZTUhNUm16QVYKQmdOVkhSRUVEakFNZ2dwcmRXSmxjbTVsZEdWek1BMEdDU3FHU0liM0RRRUJDd1VBQTRJQkFRRE9xeENYS2hSdgpPamxOZVcyTE40dTBWT1Z1UnpvcmZXVWxPNnlFQzExbWhoNlR3bWRYQi9aL3pSM3BMekF2MCtiQXQvSm90dG1mCmNYN05idThpOGZ4dlpBY0VoS3BVRVg1L1plVEE5c0JDSkhjTlFIY2JqTUVkZzY3WC9aUTlKZFo4VFlNUFVWNEUKQXMycFQ2OVkvS0poM0ZBWHZLOERyK0ZJbE5jY1Zwd0JEc1gxb21mMUV6TTlKOE1DaVFBNmtldmtLOWprdkN1ZwpFMUg1TmdFVEYrL21JYTZpTHJSTG4vZEF0V0dZWHNqKzN0cEZNUUdES3ZhTlg2Q2xNby9admhUTkE1ZlRXZzJICnZTSllBWVdlV3dtR1BYcUQvWjkxd2tUZmtVUHBQK2diREhzbVJHVGlab1lYYU91OEVWUWFodnVXUC9Ncmk3Z3QKZ2JSWWl2NitmakRvCi0tLS0tRU5EIENFUlRJRklDQVRFLS0tLS0K', clusterName: 'geoloc-eks', contextName: 'arn:aws:eks:us-east-1:358966077154:cluster/geoloc-eks', credentialsId: 'eks_credential', namespace: '', serverUrl: ''){
                    sh 'kubectl apply -f eks-deploy-from-ecr.yaml'
                    }
                }
            }
        }
    }
}