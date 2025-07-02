pipeline {
    agent {
        docker {
            image 'node:22-alpine'
        }
    }

    environment {
        IMAGE_NAME = 'jenkins_assignment'
        CONTAINER_NAME = 'jenkins_assignment'
        HOST_PORT = '3000'
        CONTAINER_PORT = '3000'
    }

    stages {
        stage('Clone Repository') {
            steps {
                echo "Cloning the repository..."
                git url: 'https://github.com/AgamSinghh/jenkins_assign.git', branch: 'main'
            }
        }

        stage('Install Dependencies') {
            steps {
                echo "Installing npm dependencies..."
                sh 'npm install'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo "Building Docker image..."
                sh 'docker build -t $IMAGE_NAME:latest .'
            }
        }

        stage('Deploy Container') {
            steps {
                echo "Deploying the Docker container..."
                sh '''
                    docker rm -f $CONTAINER_NAME || true
                    docker run -d -p $HOST_PORT:$CONTAINER_PORT --name $CONTAINER_NAME $IMAGE_NAME:latest
                '''
            }
        }
    }

    post {
        always {
            echo "Cleaning up untagged Docker images..."
            sh 'docker image prune -f'
        }
        success {
            echo "deployment completed successfully."
        }
        failure {
            echo " dployment failed."
        }
    }
}
