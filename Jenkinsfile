pipeline {
    def app
    agent any
    stages {
        stage('Clone repository') {
            steps { 
                checkout scm
            }
        }

        stage('Build image') {
            steps {
                script {
                    app = docker.build("michaeljmckenna/coursework2")
                }
            }
        }

        stage('Push image') {
           steps {
               script {
                   docker.withRegistry('', 'docker-hub-credentials') {
                        app.push("${env.BUILD_NUMBER}")
                        app.push("latest")
                    }    
                } 
            }
        }

        stage("build & SonarQube analysis") {
            environment {
                scannerHome = tool 'SonarQube'
            }
            steps {
                withSonarQubeEnv('SonarQube') {
                   sh "${scannerHome}/bin/sonar-scanner"
                }
                timeout(time: 10, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }
}