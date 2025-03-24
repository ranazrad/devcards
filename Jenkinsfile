pipeline {
    agent any

    triggers {
        githubPush()  
    }

    stages {
        stage('Checkout') {
            steps {
                script {
                    
                    if (env.GIT_BRANCH == 'origin/develop' || env.BRANCH_NAME == 'develop') {
                        checkout scm
                    } else {
                        error('Not on the develop branch, aborting...')
                    }
                }
            }
        }
    }
}