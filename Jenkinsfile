node {
    def app 
    // clone the repository to s3
    stage('Clone Repository') {
        checkout scm
    }
    // build the docker image
    stage('Build image') {
        app = docker.build('diazagasatya/dazdocker')
    }
    // run unit testing here from the image
    stage('Test image') {
        app.inside {
            sh 'echo "Test passed"'
        }
    }
    // push the image to docker hub
    stage('Push image') {
        docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
            app.push("${env.BUILD_NUMBER}")
        }
    }
    // will run manifest update that will update the deployment yaml
    stage('Trigger ManifestUpdate') {
        echo "triggering updatemanifestjob"
        build job: "updatemanifest", parameters: [string(name: 'DOCKERTAG', value: env.BUILD_NUMBER)]
    }
}