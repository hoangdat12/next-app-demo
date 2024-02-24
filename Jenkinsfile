pipeline {
    agent any
    
    tools {
        nodejs "nodejs"
    }

    stages {
        stage ('Checkout') { 
            steps { 
                git branch:'main', url: 'https://github.com/hoangdat12/next-app-demo' 
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
    
    post {
        success {
            echo "SUCCESSFUL"
        }
        failure {
            echo "FAILED"
        }
    }
}
