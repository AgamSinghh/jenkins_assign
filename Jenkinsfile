pipeline {
    agent {
        docker {
            image 'node:22-alpine'
            args '-u root:root -v /var/run/docker.sock:/var/run/docker.sock'
        }
    }

    environment {
        GIT_REPO = 'https://github.com/AgamSinghh/jenkins_assign.git'
        IMAGE_NAME = 'jenkinsassign'
        TAG = 'latest'
    }

    stages {

        stage('Clone Repository') {
            steps {
                echo 'Cloning repository...'
                sh 'apk add git' 
                sh 'rm -rf jenkins_assign && git clone $GIT_REPO'
                dir('jenkins_assign') {
                    sh 'ls -la'
                }
            }
        }

        stage('Install Dependencies') {
            steps {
                dir('jenkins_assign') {
                    echo 'Installing npm dependencies...'
                    sh 'npm install'
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                dir('jenkins_assign') {
                    echo 'Building Docker image...'
                    sh 'docker build -t $IMAGE_NAME:$TAG .'
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                echo 'Running container...'
                sh 'docker run -d -p 3000:3000 --name running_app $IMAGE_NAME:$TAG'
            }
        }
    }

    post {
        always {
            echo 'Cleaning up...'
            sh 'docker image prune -f || true'
        }
    }
}
