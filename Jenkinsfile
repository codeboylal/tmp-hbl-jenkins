pipeline {
    agent any

    environment{
        DOCKER_IMAGE = 'my-new-website-prod'
        CONTAINER_NAME = 'container-my-new-website-prod'
    }

    stages {
        stage('Code Fetch') {
            steps {
                echo 'Code fetch from GitHub'
                git url: 'https://github.com/codeboylal/tmp-hbl-jenkins.git', branch: 'main'
            }
        }

        stage('Build') {
            steps {
                // Stopping existing containers
                echo 'Stopping previous container'
                sh "docker stop ${CONTAINER_NAME} || true"

                // Removing existing containers
                echo 'Removing previous container'
                sh "docker rm ${CONTAINER_NAME} || true"

                // Building new fresh image
                sh 'docker build -t my-new-website-prod .'
            }
        }

         // Deploy to Docker Container

        stage('Deploy - Docker') {
            steps {                
                echo 'Starting new container'
                sh "docker run -d --name ${CONTAINER_NAME} -p 80:80 ${DOCKER_IMAGE}"
                
                // Remove unused containers and images after successful running desired containers
                sh "docker system prune -a -f" 
            }
        }
    }
}
