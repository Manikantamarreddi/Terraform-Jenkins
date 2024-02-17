pipeline {

    parameters {
  booleanParam defaultValue: flase, description: 'Automatically run apply after generating plan?', name: 'autoApprove'
}

    environment {
        AWS_ACCESS_KEY = credentials('AWS_ACCESS_KEY_ID')
        AWS_SECRET_KEY = credentials('AWS_SECRET_ACCESS_KEY')
    }

    agent any
    
    stages {
        stage('Checkout') {
            steps {
                script{
                    dir('terraform') {
                        git branch: 'main', credentialsId: 'GitHUB', url: 'https://github.com/Manikantamarreddi/Terraform-Jenkins.git'
                    }
                }
            }
        }

        stage('initialize') {
            steps {
                sh 'pwd;cd terraform/;terraform init'
            }
        }

        stage('Plan') {
            steps {
                sh 'pwd;cd terraform/;terraform plan -out tfplan'
                sh 'cd terraform/; terraform show -no-color tfplan > tfplan.txt'
            }
        }

        stage('Approval') {
            when {
                not {
                    equals expected: true, actual: param.autoApprove
                }
            }

            steps {
                script {
                    def plan = readFile 'terraform/tfplan.txt'
                    input message: "Do you want to apply the plan?"
                    parameters {
  text defaultValue: 'plan', description: 'Please review the plan', name: 'Plan'
}
                }
            }
        }

        stage ('Apply') {
            steps {
                sh 'cd terraform/; terraform apply -input=false tfplan'
            }
        }
    }
}