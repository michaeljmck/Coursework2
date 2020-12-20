pipeline {
    environment {
        app=''
    }
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

        stage("SonarQube analysis") {
            environment {
                scannerHome = tool 'SonarQube'
            }
            steps {
                withSonarQubeEnv('SonarQube') {
                   sh "${scannerHome}/bin/sonar-scanner"
                }
                timeout(time: 5, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    sh "ssh ubuntu@100.25.166.183 \
                        kubectl set image deployments/cw2deployment \
                        cw2deployment=michaeljmckenna/coursework2:${env.BUILD_NUMBER}"
                }
            }
        }
    }
}