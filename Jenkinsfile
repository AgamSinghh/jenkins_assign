pipeline {
    agent {
        docker {
            image 'node:18-alpine'
            args '-u root:root'
        }
    }

    environment {
        APP_NAME = "jenkins-assign"
        GIT_REPO = 'https://github.com/AgamSinghh/jenkins_assign.git'
        IMAGE_NAME = 'jenkinsassign'
        TAG = 'latest'
        CONTAINER_NAME = 'jenkinsassign-container'
    }

    stages {

        stage('Clone Repository') {
            steps {
                echo 'Cloning repository...'
                git branch: 'main', url: "$GIT_REPO"
                echo "Repository clones successfully"

            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
                echo "Dependencies done"
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME:$TAG .'
                echo "Build done"
            }
        }

        stage('Run Docker Container') {
            steps {
                sh '''

                docker rm -f $CONTAINER_NAME || true
                docker run -d --name $CONTAINER_NAME -p 3000:3000 $IMAGE_NAME
                '''
                echo "Container is running"
            }
        }
    }

    post {
        success {
            echo "Pipelines completed successfully"
        }
        failure {
            echo "Pipeline failed"
        }
    }
}
