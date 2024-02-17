pipeline {

     environment {
        AWS_ACCESS_KEY = credentials('AWS_ACCESS_KEY_ID')
        AWS_SECRET_KEY = credentials('AWS_SECRET_ACCESS_KEY')
    }

   agent any

    stages {
        stage('checkout') {
            steps {
                 script{
                        dir("terraform")
                        {
                            git branch: 'main', credentialsId: 'GitHUB', url: 'https://github.com/Manikantamarreddi/Terraform-Jenkins.git'
                        }
                    }
                }
            }

        stage('Plan') {
            steps {
                sh 'pwd;cd terraform/ ; terraform init'
            }
        }

        stage('Destroy') {
            steps {
                sh "pwd;cd terraform/ ; terraform destroy -auto-approve"
            }
        }
    }

  }