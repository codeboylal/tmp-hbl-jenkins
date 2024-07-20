pipeline {
    agent any

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

        stage('Deploy') {
            steps {
                echo 'Deploy on container'

                // Stop and remove any existing container using the same image name
                script {
                    def containerId = sh(script: "docker ps -aq --filter 'ancestor=my-new-website-prod'", returnStdout: true).trim()
                    if (containerId) {
                        sh "docker stop ${containerId}"
                        sh "docker rm ${containerId}"
                    }
                }

                // Run the new container
                sh 'docker run -d --name my-new-website-prod -p 80:80 my-new-website-prod'
            }
        }
    }
}
