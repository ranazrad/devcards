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
                    exit 0
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
                sh '''
                    docker build -t ${IMAGE_NAME}:${BUILD_ID} .
                '''
            }
        }

        stage('Run Docker Container') {
            steps {
                sh '''
                    docker rm -f ${CONTAINER_NAME} || true
                    docker run -d --name ${CONTAINER_NAME} -p 8000:8000 ${IMAGE_NAME}:${BUILD_ID}
                '''
            }
        }

    }

    post {
        always {
            echo 'Pipeline execution completed.'
        }
    }
}
