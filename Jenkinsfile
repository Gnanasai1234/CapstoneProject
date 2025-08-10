pipeline {
    agent any

    environment {
        FRONTEND_REPO = 'https://github.com/Gnanasai1234/CapstoneProject.git'
        BACKEND_REPO  = 'https://github.com/Gnanasai1234/BackendApp.git'
    }

    stages {
        stage('Clone Frontend') {
            steps {
                dir('frontend') {
                    git branch: 'main', url: "${FRONTEND_REPO}"
                }
            }
        }

        stage('Build Frontend') {
            steps {
                dir('frontend') {
                    bat 'npm install'
                    bat 'npm run build'
                }
            }
        }

        stage('Clone Backend') {
            steps {
                dir('backend') {
                    git branch: 'main', url: "${BACKEND_REPO}"
                }
            }
        }

        stage('Copy Frontend to Backend') {
            steps {
                // Copy React build output into Spring Boot static folder
                bat 'xcopy frontend\\build backend\\src\\main\\resources\\static /E /I /Y'
            }
        }

        stage('Build Backend') {
            steps {
                dir('backend') {
                    bat './mvnw clean package -DskipTests'
                }
            }
        }

        stage('Docker Build & Run') {
            steps {
                // Assuming docker-compose.yml is in backend folder
                dir('backend') {
                    bat 'docker-compose down'
                    bat 'docker-compose build'
                    bat 'docker-compose up -d'
                }
            }
        }
    }

    post {
        success {
            echo "Pipeline Completed Successfully! 🎉"
        }
        failure {
            echo "Pipeline Failed. Please check the logs."
        }
    }
}
