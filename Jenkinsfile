pipeline {
    agent {
        docker {
            image 'node: 22'
        }
    }

    environment {
        APP_NAME = 'jenkins-assignment'
        HOST_PORT= '3000'
        CONTAINER_PORT = '3000'
    }

    stages {
        stage('Clone repository') {
            steps {
                echo "Cloning the repoo"
                git url: 'https://github.com/AgamSinghh/jenkins_assign.git' , branch: 'main'
            }
        }

        stage('Installing depened..') {
            steps {
                echo "Installing dependencies"
                sh 'npm install'
            }
        }

        stage('Docker Build') {
            steps {
                echo "Biulding the Docker image"
                sh'docker build -t $APP_NAME:latest .'
            }
        }
        stage('Deploying the containers') {
            steps{
                echo"Deploying the containers"
                sh '''
                docker rm -f $APP_NAME || true
                docker run -d -p $HOST_PORT:$CONTAINER_PORT --name $APP_NAME $APP_NAME:latest
                '''
            }
        }
    }
    post {
        success {
            echo "Build and deployment successful"
        }
        failure {
            echo "Build or deployment failed"
        }

    }
}