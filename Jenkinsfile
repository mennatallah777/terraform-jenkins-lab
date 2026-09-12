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
        stage('Hello World') {
            steps {
                sh '''
                echo 'Hello World'
                '''
            }
        }
        stage('run another hello') {
            steps {
                sh """
                echo 'Deploying to environment: ${params.ENVIRONMENT}'
                """
            }
        }
        stage('Approval') {
            steps {
                input message: "Proceed with ${params.ENVIRONMENT}?", ok: 'Approve'
            }
        }
        stage('Final Step') {
            steps {
                echo "Running final step for ${params.ENVIRONMENT}"
            }
        }
    }

    post {
        always {
            echo 'Pipeline finished running.'
        }
        success {
            echo "✅ Build succeeded for ${params.ENVIRONMENT}!"
            mail to: 'your-email@example.com',
                 subject: "✅ Build Succeeded - ${params.ENVIRONMENT}",
                 body: "Pipeline for ${params.ENVIRONMENT} completed successfully. Build #${env.BUILD_NUMBER}"
        }
        failure {
            echo "❌ Build failed for ${params.ENVIRONMENT}."
            mail to: 'your-email@example.com',
                 subject: "❌ Build Failed - ${params.ENVIRONMENT}",
                 body: "Pipeline for ${params.ENVIRONMENT} failed. Build #${env.BUILD_NUMBER}"
        }
        aborted {
            echo "⚠️ Build aborted for ${params.ENVIRONMENT}."
            mail to: 'your-email@example.com',
                 subject: "⚠️ Build Aborted - ${params.ENVIRONMENT}",
                 body: "Pipeline for ${params.ENVIRONMENT} was manually aborted by a user. Build #${env.BUILD_NUMBER}"
        }
    }
}