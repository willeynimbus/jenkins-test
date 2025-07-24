pipeline {
    agent {
        docker {
            image 'python:3.10'
            args '-u root:root'  // Optional, if you need root privileges
        }
    }

    stages {
        stage('Install dependencies') {
            steps {
                sh 'pip install -r requirements.txt'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'python -m unittest discover > result.log'
            }
            post {
                always {
                    archiveArtifacts artifacts: 'result.log', allowEmptyArchive: true
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t willeynimbus99/calculator-app:new-latest ."
            }
        }

        stage('Login to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'Docker-hubs-cred', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_TOKEN')]) {
                    sh "echo $DOCKER_TOKEN | docker login -u $DOCKER_USERNAME --password-stdin"
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                sh "docker push willeynimbus99/calculator-app:new-latest"
            }
        }
    }
}
