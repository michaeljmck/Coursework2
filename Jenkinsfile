node {
    def app

    stage('Clone repository') {

        checkout scm
    }

    stage('Build image') {
        

        app = docker.build("michaeljmckenna/coursework2")
    }

    stage('Push image') {
       
        docker.withRegistry('', 'docker-hub-credentials') {
            app.push("${env.BUILD_NUMBER}")
            app.push("latest")
        }
    }
}
