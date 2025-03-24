pipeline {
    agent any

    environment {
        IMAGE_NAME = 'devcards'
        CONTAINER_NAME = "devcards-container-${env.BUILD_ID}"
        HOST_PORT = '8000'
        CONTAINER_PORT = '8000'
    }

    stages {
        stage('Validate PR Target') {
            when {
                expression {
                    return env.CHANGE_ID && env.CHANGE_TARGET == 'develop'
                }
            }
            steps {
                echo "This pipeline only runs for PRs targeting 'develop'. Skipping."
                script {
                    currentBuild.result = 'SUCCESS'
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
                    echo "Building image ${IMAGE_NAME}:${BUILD_ID}"
                    docker build -t ${IMAGE_NAME}:${BUILD_ID} . --no-cache
                '''
            }
        }

        stage('Stop Previous Container on Port') {
            steps {
                sh '''
                    docker rm -f $(docker ps -qa)
                '''
            }
        }

        stage('Run Docker Container') {
            steps {
                sh '''
                    echo "Running container ${CONTAINER_NAME} on port ${HOST_PORT}"
                    docker run -d --name ${CONTAINER_NAME} -p ${HOST_PORT}:${CONTAINER_PORT} ${IMAGE_NAME}:${BUILD_ID}
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
