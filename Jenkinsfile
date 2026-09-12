pipeline {
    agent any

    parameters {
        choice(
            name: 'ENVIRONMENT',
            choices: ['dev', 'stg', 'prod'],
            description: 'Select which environment to deploy'
        )
    }

    stages {
        stage('Terraform Init') {
            steps {
                sh 'terraform init'
            }
        }

        stage('Select Workspace') {
            steps {
                sh """
                    terraform workspace select ${params.ENVIRONMENT} || terraform workspace new ${params.ENVIRONMENT}
                """
            }
        }

        stage('Terraform Plan') {
            steps {
                sh "terraform plan -var-file=${params.ENVIRONMENT}.tfvars -out=tfplan"
            }
        }

        stage('Approval') {
            steps {
                input message: "Apply this plan to ${params.ENVIRONMENT}?", ok: 'Approve'
            }
        }

        stage('Terraform Apply') {
            steps {
                sh 'terraform apply -auto-approve tfplan'
            }
        }
    }

    post {
        success {
            echo "✅ Deployment to ${params.ENVIRONMENT} succeeded!"
            mail to: 'your-email@example.com',
                 subject: "✅ Terraform Apply Succeeded - ${params.ENVIRONMENT}",
                 body: "The pipeline for ${params.ENVIRONMENT} completed successfully. Build #${env.BUILD_NUMBER}"
        }
        failure {
            echo "❌ Deployment to ${params.ENVIRONMENT} failed."
            mail to: 'your-email@example.com',
                 subject: "❌ Terraform Apply Failed - ${params.ENVIRONMENT}",
                 body: "The pipeline for ${params.ENVIRONMENT} failed. Check Jenkins logs. Build #${env.BUILD_NUMBER}"
        }
    }
}
