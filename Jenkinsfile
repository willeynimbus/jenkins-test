pipeline {
    agent {
        dockerfile {
            filename 'Dockerfile'
            dir '.'
        }
    }

    stages {

        stage('Install dependencies') {
            steps {
                bat 'pip install -r requirement.txt'
            }
        }

        stage('Run Tests') {
            steps {
                bat 'python -m unittest discover > result.log'
            }
            post {
                always {
                    archiveArtifacts artifacts: 'result.log', allowEmptyArchive: true
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                bat "docker build -t willeynimbus99/calculator-app:new-latest ."
            }
        }

        stage('Login to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'Docker-hubs-cred', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_TOKEN')]) {
                    bat "echo %DOCKER_TOKEN% | docker login -u %DOCKER_USERNAME% --password-stdin"
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                bat "docker push willeynimbus99/calculator-app:new-latest"
            }
        }
    }
}
