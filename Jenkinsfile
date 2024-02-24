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
        IMAGE_NAME = "${DOCKER_USER}" + "/" + "APP_NAME"
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

        stage("SonarQube analysis") {
            stage("SonarQube analysis") {
                steps {
                    script {
                        def scannerHome = tool 'SonarScanner'
                        withSonarQubeEnv(credentialsId: 'Jenkins-token') {
                            sh "${scannerHome}/bin/sonar-scanner"
                        }
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
