pipeline {
    agent any

    parameters {
        string(name: 'GIT_BRANCH', defaultValue: 'main', description: 'Branch to build')
        string(name: 'IMAGE_NAME', defaultValue: 'jenkinsassign', description: 'Docker image name')
        string(name: 'CONTAINER_NAME', defaultValue: 'jenkinsassign-container', description: 'Docker container name')
        string(name: 'PORT', defaultValue: '3000', description: 'Port to expose the app on')
    }

    environment {
        GIT_REPO = 'https://github.com/AgamSinghh/jenkins_assign.git'
    }

    stages {
        stage('Clone Repo') {
            steps {
                echo "Cloning branch: ${params.GIT_BRANCH}"
                git branch: "${params.GIT_BRANCH}", url: "${env.GIT_REPO}"
                echo 'Repository cloned'
            }
        }

        stage('Build Docker Image for App') {
            steps {
                echo "Building Docker image: ${params.IMAGE_NAME}"
                sh "docker build -t ${params.IMAGE_NAME} ."
            }
        }

        stage('Run App in Container') {
            steps {
                echo "Running container: ${params.CONTAINER_NAME} on port ${params.PORT}"
                sh """
                    docker rm -f ${params.CONTAINER_NAME} || true
                    docker run -d --name ${params.CONTAINER_NAME} -p ${params.PORT}:3000 ${params.IMAGE_NAME}
                """
                echo "App is running at http://localhost:${params.PORT}"
            }
        }
    }

    post {
        always {
            echo "Cleaning up dangling Docker images..."
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
