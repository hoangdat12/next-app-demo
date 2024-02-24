pipeline {
    agent any
    
    tools {
        nodejs "nodejs"
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
