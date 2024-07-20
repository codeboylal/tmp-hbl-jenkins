pipeline {
    agent any

    stages {
        stage('Code Fetch') {
            steps {
                echo 'Code fetch form GitHub'
                git url:'https://github.com/codeboylal/tmp-hbl-jenkins.git', branch: 'main'
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

                // Stop and remove existing container if it exists
                script {
                    def containerId = sh(script: "docker ps -q --filter 'name=my-new-website-prod'", returnStdout: true).trim()
                    if (containerId) {
                        sh "docker stop ${containerId}"
                        sh "docker rm ${containerId}"
                    }
                }
                
                // Deploy fresh application to docker container
                sh 'docker run -d -p 80:80 my-new-website-prod'

            }
        }
    }
}
