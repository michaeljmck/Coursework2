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

    stage("build & SonarQube analysis") {
        environment {
        scannerHome = tool 'SonarQube'
    }
          node {
              withSonarQubeEnv('SonarQube') {
                 sh "${scannerHome}/bin/sonar-scanner"
              }
          }
      }

      stage("Quality Gate"){
          timeout(time: 10, unit: 'MINUTES') {
              def qg = waitForQualityGate()
              if (qg.status != 'OK') {
                  error "Pipeline aborted due to quality gate failure: ${qg.status}"
              }
          }
      }
}
