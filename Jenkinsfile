pipeline {
    agent any

    environment {
        IMAGE_NAME = 'agam/jenkinsassignment'
        CONTAINER_NAME = 'jenkinsassignment'
        HOST_PORT = '3000'
        CONTAINER_PORT = '3000'
    }

    stages {
        stage('Clone repository') {
            steps {
                git url: 'https://github.com/AgamSinghh/jenkins_assign.git', branch: 'main'
            }
        }

        stage('Install dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Build Docker image') {
            steps {
                sh 'docker build -t $IMAGE_NAME:latest .'
            }
        }

        stage('Deploy container') {
            steps {
                sh '''
                    docker rm -f $CONTAINER_NAME || true
                    docker run -d -p $HOST_PORT:$CONTAINER_PORT --name $CONTAINER_NAME $IMAGE_NAME:latest
                '''
            }
        }
    }

    post {
        always {
            script {
                sh 'docker image prune -f'
            }
        }
        success {
            echo "Build & deployment successful"
        }
        failure {
            echo "Build or deployment failed"
        }
    }
}
