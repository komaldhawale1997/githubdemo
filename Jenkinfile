@Library('company-shared-library') _

pipeline {

    agent {
        label 'docker-k8s-agent'
    }

    parameters {
        choice(
            name: 'ENVIRONMENT',
            choices: ['dev', 'qa', 'prod'],
            description: 'Select deployment environment'
        )
    }

    environment {
        APP_NAME   = 'payment-api'
        IMAGE_NAME = '123456789012.dkr.ecr.ap-south-1.amazonaws.com/payment-api'
        IMAGE_TAG  = "${BUILD_NUMBER}"
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Application') {
            steps {
                build(
                    appName: env.APP_NAME,
                    imageName: env.IMAGE_NAME,
                    imageTag: env.IMAGE_TAG
                )
            }
        }

        stage('Deploy') {
            steps {
                deploy(
                    appName: env.APP_NAME,
                    environment: params.ENVIRONMENT,
                    imageTag: env.IMAGE_TAG
                )
            }
        }
    }

    post {
        success {
            echo "Pipeline completed successfully."
        }

        failure {
            echo "Pipeline failed. Check Jenkins logs."
        }

        always {
            echo "Pipeline finished."
        }
    }
}
