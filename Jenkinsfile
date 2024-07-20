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
                sh 'docker build -t my-new-website-prod .'
            }
        }

         // Deploy to Docker Container

        stage('Deploy - Docker') {
            steps {
               
                echo 'Stopping previous container'
                sh "docker stop ${CONTAINER_NAME} || true"
                
                echo 'Removing previous container'
                sh "docker rm ${CONTAINER_NAME} || true"
                
                echo 'Starting new container'
                sh "docker run -d --name ${CONTAINER_NAME} -p 80:80 ${DOCKER_IMAGE}"
            }
        }
    }
}
