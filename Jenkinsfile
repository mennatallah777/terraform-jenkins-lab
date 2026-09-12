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
        }
        failure {
            echo "❌ Build failed for ${params.ENVIRONMENT}."
        }
    }
}