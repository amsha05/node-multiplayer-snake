node ('ubuntu') {
    def app
    // Stage to clone the Git repository
    stage('Cloning Git') {
        /* Let's make sure we have the repository cloned to our workspace */
        checkout scm
    }
 
    // Stage to build and tag the Docker image
    stage('Build-and-Tag') {
        /* This builds the actual image; synonymous to
         * docker build on the command line */
        app = docker.build("amshashree/snake")
    }
 
    // Stage to log in to Docker Hub and push the image
    stage('Post-to-dockerhub') {
        withCredentials([usernamePassword(credentialsId: 'training_creds', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
            script {
                // Log in to Docker Hub using --password-stdin
                sh """
                    echo ${DOCKER_PASSWORD} | docker login --username ${DOCKER_USERNAME} --password-stdin https://registry.hub.docker.com
                """
                // Push the image to Docker Hub
                app.push("latest")
            }
        }
    }
 
    // Stage to bring up the image using Docker Compose
    stage('Pull-image-server') {
        sh "docker-compose down"
        sh "docker-compose up -d"
    }
}
