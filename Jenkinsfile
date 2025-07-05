pipeline {
    agent any

    environment {
        GIT_REPO = 'https://github.com/AgamSinghh/jenkins_assign.git'
        IMAGE_NAME = 'jenkinsassign'
        CONTAINER_NAME = 'jenkinsassign-container'
        PORT = '3000'
    }

    stages {

        stage('Clone Repo') {
            steps {
                echo 'Cloning repository...'
                git branch: 'main', url: "${GIT_REPO}"
                echo 'Repository cloned'
            }
        }

        

        stage('Build Docker Image for App') {
            steps {
                echo 'Building Docker image...'
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        stage('Run App in Container') {
            steps {
                echo 'Running Docker container...'
                sh '''
                    docker rm -f $CONTAINER_NAME || true
                    docker run -d --name $CONTAINER_NAME -p $PORT:3000 $IMAGE_NAME
                '''
                echo "App is running at http://localhost:$PORT"
            }
        }
    }

    post {
        always{
            echo"cleaning up the images"
            sh 'docker image prune -f'
        }
        success {
            echo 'Pipeline completed successfully.'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}
