node {
    def app

    stage('Clone repository') {

        checkout scm
    }

    // stage('Build image') {
        

    //     app = docker.build("michaeljmckenna/coursework2")
    // }

    
    // stage('Push image') {
       
    //     docker.withRegistry('', 'docker-hub-credentials') {
    //         app.push("${env.BUILD_NUMBER}")
    //         app.push("latest")
    //     }

    // }

    stage('SonarQube') {
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
