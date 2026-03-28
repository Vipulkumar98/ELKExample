node {
    def application = "springbootapp"
    def dockerhubaccountid = "vipulkumar98"
    def app

    stage('Clone repository') {
        checkout scm
    }

    stage('Build image') {
        app = docker.build("${dockerhubaccountid}/${application}:${BUILD_NUMBER}")
    }

    stage('Push image') {
        withDockerRegistry([credentialsId: "dockerHub", url: ""]) {
            app.push()
            app.push("latest")
        }
    }

    stage('Deploy') {
        sh """
            docker rm -f springbootapp-container || true
            docker run -d --name springbootapp-container -p 8081:8080 -v /var/log/:/var/log/ ${dockerhubaccountid}/${application}:${BUILD_NUMBER}
        """
    }

    stage('Remove old images') {
        sh "docker rmi ${dockerhubaccountid}/${application}:latest -f || true"
    }
}
