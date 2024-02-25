pipeline {
    agent any
    
    // Define the tools required for the pipeline
    tools {
        nodejs "nodejs" // Assuming "nodejs" is the name of your Node.js installation in Jenkins
    }

    // Define environment variables
    environment {
        APP_NAME = "demo-project"
        RELEASE = "1.0.0"
        DOCKER_USER = "hoangdat12"
        DOCKER_PASS = "dockerhub"
        IMAGE_NAME = "${DOCKER_USER}/${APP_NAME}"
        IMAGE_TAG = "${RELEASE}-${BUILD_NUMBER}"
    }

    stages {
        stage("Install") {
            steps {
                // Install Node.js dependencies
                sh 'npm install'
            }
        }

        stage("Build") {
            steps {
                // Build the project
                sh 'npm run build'
            }
        }

        stage("Build & Deploy Docker Image") {
            steps {
                script {
                    // Print environment variables for debugging
                    echo "DOCKER_USER: ${DOCKER_USER}"
                    echo "DOCKER_PASS: ${DOCKER_PASS}"
                    echo "IMAGE_NAME: ${IMAGE_NAME}"
                    echo "IMAGE_TAG: ${IMAGE_TAG}"

                    // Build and push Docker image
                    docker.withRegistry("", DOCKER_USER, DOCKER_PASS) {
                        docker_image = docker.build("${IMAGE_NAME}:${IMAGE_TAG}")
                        docker_image.push()
                    }
                }
            }
        }
    } 
    
    post {
        success {
            echo "SUCCESSFUL"
        }
        failure {
            echo "FAILED"
        }
    }
}
