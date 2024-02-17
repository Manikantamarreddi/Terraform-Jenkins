pipeline {
    agent any
    
    tools {
     terraform 'terraform-17'
    }
    
    environment {
        AWS_ACCESS_KEY_ID     = credentials('AWS_ACCESS_KEY_ID')
        AWS_SECRET_ACCESS_KEY = credentials('AWS_SECRET_ACCESS_KEY')
    }
    
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', credentialsId: 'GitHUB', url: 'https://github.com/Manikantamarreddi/Terraform-Jenkins.git'
            }
        }
        
        stage('Terraform init') {
            steps {
                sh 'terraform init'
            }
        }
        
        stage('Terraform plan') {
            steps {
                sh 'terraform plan'
            }
        }
        
        stage('Terraform apply') {
            steps {
                sh 'terraform apply -auto-approve'
            }
        }

        stage('Destroy') {
            steps {
                sh 'terraform destroy -auto-approve'
            }
        }
    }
}