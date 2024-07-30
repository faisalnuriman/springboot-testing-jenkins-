pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                script {
                    sh './mvnw clean package -DskipTests'
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    sh './mvnw test'
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t my-springboot .'
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    sh 'docker run -d -p 8081:8080 --name my-springboot-container my-springboot'
                }
            }
        }
    }

    triggers {
        pollSCM('H/5 * * * *')
    }
}

