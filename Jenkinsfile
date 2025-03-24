pipeline {
    agent any

    environment {
        IMAGE_NAME = 'devcards'
        CONTAINER_NAME = "devcards-container-${env.BUILD_ID}"
    }

    stages {

        stage('Validate PR Target') {
            when {
                expression {
                    return !(env.CHANGE_ID && env.CHANGE_TARGET == 'develop')
                }
            }
            steps {
                echo "This pipeline only runs for PRs targeting 'develop'. Skipping."
                script {
                    currentBuild.result = 'SUCCESS'
                    return
                }
            }
        }

        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                sh echo 'jessica'
            }
        }

        stage('Run Docker Container') {
            steps {
                sh echo 'jessica'
            }
        }

    }

    post {
        always {
            echo 'Pipeline execution completed.'
        }
    }
}
