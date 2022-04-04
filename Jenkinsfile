node {
    def commit_id

    stage("Preparation") {
        checkout scm
        sh "git rev-parse --short HEAD > .git/commit-id"
        commit_id = readFile(".git/commit-id").trim()
    }

    stage("Test") {
        nodejs(nodeJSInstallationName: "NodeJS") {
            sh "npm install --only=dev"
            sh "npm test"
        }
    }

    stage("Docker build/push") {
        docker.withRegistry("http://index.docker.io/v1/", "dockerhub") {
            def app = docker.build("qkransrb90/jenkins-express-server:${commit_id}", ".").push()
        }
    }
}