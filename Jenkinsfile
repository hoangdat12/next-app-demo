pipeline {
    agent any
    
    tools {
        nodejs "nodejs"
    }

    environment {
        APP_NAME = "demo-project"
        RELEASE = "1.0.0"
        DOCKER_USER = "hoangdat12"
        DOCKER_PASS = "dockerhub"
        IMAGE_NAME = "${DOCKER_USER}" + "/" + "${APP_NAME}"
        IMAGE_TAG = "${RELEASE}-${BUILD_NUMBER}"
    }

    stages {
        stage("Install") {
            steps {
                sh 'npm install'
            }
        }

        stage("Build") {
            steps {
                sh 'npm run build'
            }
        }

        state("Build & Deploy Docker Image") {
            steps {
                script {
                    docker.withRegister("", DOCKER_PASS) {
                        docker_image = docker.build "${IMAGE_NAME}"
                    }

                    docker.withRegister("", DOCKER_PASS) {
                        docker_image.push("${IMAGE_NAME}")
                        docker_image.push("latest")
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
