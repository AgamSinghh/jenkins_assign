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
                echo '✅ Cloned'
            }
        }

        stage('Install Dependencies') {
            steps {
                echo '📦 Installing dependencies...'
                sh 'npm install'
                echo '✅ Dependencies installed'
            }
        }

        stage('Build Docker Image for App') {
            agent any 
            steps {
                echo '🐳 Building Docker image outside container agent...'
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        stage('Run App in Container') {
            agent any
            steps {
                echo '🚀 Running Docker container outside agent container...'
                sh '''
                    docker rm -f $CONTAINER_NAME || true
                    docker run -d --name $CONTAINER_NAME -p $PORT:3000 $IMAGE_NAME
                '''
                echo "✅ App is running at http://localhost:$PORT"
            }
        }
    }

    post {
        success {
            echo '🎉 Pipeline completed with Docker agent + external container run!'
        }
        failure {
            echo '❌ Pipeline failed'
        }
    }
}
