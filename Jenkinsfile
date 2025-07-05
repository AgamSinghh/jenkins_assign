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
                echo '✅ Repository cloned successfully'
            }
        }

        stage('Install Dependencies') {
            steps {
                echo 'Installing...'
                sh 'npm install'
                echo 'Installed'
            }
        }

        stage('Run the App') {
            steps {
                echo '🚀 Starting the app...'
                sh ' npm start &'
                echo '✅ App started on http://localhost:3000'
            }
        }
    }

    post {
        success {
            echo '🎉 Pipeline completed successfully'
        }
        failure {
            echo 'Failed'
        }
    }
}
