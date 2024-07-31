pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/faisalnuriman/springboot-testing-jenkins-.git'
            }
        }
        stage('Build') {
            steps {
                sh './mvnw clean install'
            }
        }
    }
}
