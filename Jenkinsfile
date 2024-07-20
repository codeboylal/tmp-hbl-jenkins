pipeline {
    agent any

    stages {
        stage('Code Fetch') {
            steps {
                echo 'Code fetch from GitHub'
                git url: 'https://github.com/codeboylal/tmp-hbl-jenkins.git', branch: 'main'
            }
        }

        stage('Clean Up') {
            steps {
                script {
                    // Stop and remove the existing container
                    def containerId = sh(script: "docker ps -q --filter 'name=my-new-website-prod'", returnStdout: true).trim()
                    if (containerId) {
                        sh "docker stop ${containerId}"
                        sh "docker rm ${containerId}"
                    }

                    // Remove the existing image
                    def imageId = sh(script: "docker images -q my-new-website-prod", returnStdout: true).trim()
                    if (imageId) {
                        sh "docker rmi -f ${imageId}"
                    }
                }
            }
        }

        stage('Build') {
            steps {
                sh 'docker build -t my-new-website-prod .'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploy on container'

                // Run the new container
                sh 'docker run -d --name my-new-website-prod -p 80:80 my-new-website-prod'
            }
        }
    }
}
