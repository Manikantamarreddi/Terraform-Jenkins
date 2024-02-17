pipeline {
    tools {
  terraform 'terraform-17'
}

    parameters {
        booleanParam(name: 'autoApprove', description: 'Do you want to aoto approve the pipeline', defaultValue: false)
    }

    environment {
        AWS_ACCESS_KEY = credentials('AWS_ACCESS_KEY_ID')
        AWS_SECRET_KEY = credentials('AWS_SECRET_ACCESS_KEY')
    }

    agent any

    stages {
        stage('checkout'){
            steps{
                script {
                    dir('terraform') {
                        git branch: 'main', credentialsId: 'GitHUB', url: 'https://github.com/Manikantamarreddi/Terraform-Jenkins.git'
                    }
                }
            }
        }

        stage('initialize') {
            steps {
                sh 'cd terraform/'
                sh 'terraform init'
            }
        }

        stage('plan') {
            steps {
                sh 'pwd;cd terraform/ ; terraform init'
                sh 'pwd;cd terraform/ ; terraform plan -out tfplan'
                sh 'pwd;cd terraform/ ; terraform show -no-color tfplan > tfplan.txt'
            }
        }

        stage('Approval') {
            when{
                not{
                    equals expected: true, actual: param.autoApprove
                }
            }

            steps{
                script {
                    def plan = readFile 'terraform/tfplan.txt'
                    input message: 'Do you want to apply the plan?',
                    parameters: [text(name: 'Plan', defaultValue: plan, description: 'please review the plan')]
                }
            }
        }

        stage('Apply') {
            steps{
                sh 'cd terraform/; terraform apply -input=false tfplan'
            }
        }
    }
}