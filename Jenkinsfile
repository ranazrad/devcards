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
                echo "PR is targeting develop. Proceeding with pipeline."
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
                    docker build -t ${IMAGE_NAME}:${BUILD_ID} .
                '''
            }
        }

        stage('Stop Previous Container on Port') {
            steps {
                sh '''
                    echo "Looking for containers using port ${HOST_PORT}"
                    CONTAINER_ID=$(docker ps -q --filter "publish=${HOST_PORT}")
                    if [ ! -z "$CONTAINER_ID" ]; then
                        echo "Stopping container using port ${HOST_PORT}"
                        docker rm -f $CONTAINER_ID
                    fi
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
