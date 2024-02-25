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
        IMAGE_NAME = "${DOCKER_USER}/${APP_NAME}"
        IMAGE_TAG = "${RELEASE}-${BUILD_NUMBER}"
    }

    stages {
        stage('SCM') {
            steps {
                checkout scm
            }
        }
        
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

        stage('SonarQube Analysis') {
            steps {
                script {
                    def scannerHome = tool 'SonarScanner';
                    withSonarQubeEnv(credentialsId: "Jenkins-token") {
                      sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=demo-project"
                    }
                }
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

                    // Authenticate with Docker registry
                    withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'DOCKER_PASS', usernameVariable: 'DOCKER_USER')]) {
                        // Build and push Docker image
                        sh "echo \$DOCKER_PASS | docker login -u \$DOCKER_USER --password-stdin"

                        // Build and push Docker image
                        sh "docker build -t ${IMAGE_NAME}:${IMAGE_TAG} ."
                        sh "docker push ${IMAGE_NAME}:${IMAGE_TAG}"
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
