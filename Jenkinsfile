pipeline {
    agent {
        docker {
            image 'node:18-alpine'
            args '-u root:root'
        }
    }

    environment {
        GIT_REPO = 'https://github.com/AgamSinghh/jenkins_assign.git'
        IMAGE_NAME = 'jenkinsassign'
        CONTAINER_NAME = 'jenkinsassign-container'
        PORT = '3000'
    }

    stages {

        stage('Clone Repo') {
            steps {
                echo '📥 Cloning repository...'
                git branch: 'main', url: "${GIT_REPO}"
                echo 'Cloned'
            }
        }

        stage('Install Dependencies') {
            steps {
                echo 'Installing...'
                sh 'npm install'
                echo 'Installed'
            }
        }

        stage('Build Docker Image for App') {
            steps {
                echo 'Building app Docker image...'
                sh 'docker build -t $IMAGE_NAME .'
                echo 'Image built'
            }
        }

        stage('Run App in Container') {
            steps {
                echo 'Running app container...'
                sh '''
                    docker rm -f $CONTAINER_NAME || true
                    docker run -d --name $CONTAINER_NAME -p $PORT:3000 $IMAGE_NAME
                '''
                echo "App is live on http://localhost:$PORT"
            }
        }
    }

    post {
        success {
            echo 'Assignment completed using Docker agent!'
        }
        failure {
            echo 'Failed'
        }
    }
}
