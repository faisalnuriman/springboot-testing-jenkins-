pipeline {
    agent any

    environment {
        SLACK_CHANNEL = '#jenkins'
        SLACK_CREDENTIAL_ID = 'slack-webhook-url'
    }

    triggers {
        pollSCM('H/5 * * * *')
    }

    stages {
        stage('Checkout') {
            steps {
                checkout([
                    $class: 'GitSCM', 
                    branches: [[name: '*/main']], 
                    doGenerateSubmoduleConfigurations: false, 
                    extensions: [], 
                    submoduleCfg: [], 
                    userRemoteConfigs: [[
                        url: 'https://github.com/faisalnuriman/springboot-testing-jenkins-.git',
                        credentialsId: 'github-faisalnuriman'
                    ]]
                ])
            }
        }
        stage('Build') {
            steps {
                sh './mvnw clean install'
            }
        }
        stage('Build Docker Images') {
            steps {
                script {
                    VERSION = sh(script: 'git rev-parse HEAD', returnStdout: true).trim()
                    sh 'docker build -t faisalnuriman/springboot:' + VERSION + ' -f Dockerfile .'
                }
            }
        }
        stage('Run Docker Containers') {
            steps {
                script {
                    sh 'docker stop my-springboot-container || true'
                    sh 'docker rm my-springboot-container || true'
                    
                    sh 'docker run -d --name my-springboot-container -p 8081:8080 faisalnuriman/springboot:' + VERSION
                }
            }
        }
    }

    post {
        success {
            slackSend(
                channel: env.SLACK_CHANNEL,
                message: "Pipeline sukses: ${env.JOB_NAME} #${env.BUILD_NUMBER}. Semua containers berjalan dengan port: 8081 (latest), 8082 (v1), 8083 (v2)",
                tokenCredentialId: env.SLACK_CREDENTIAL_ID
            )
        }
        failure {
            slackSend(
                channel: env.SLACK_CHANNEL,
                message: "Pipeline gagal: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                tokenCredentialId: env.SLACK_CREDENTIAL_ID
            )
        }
        unstable {
            slackSend(
                channel: env.SLACK_CHANNEL,
                message: "Pipeline tidak stabil: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                tokenCredentialId: env.SLACK_CREDENTIAL_ID
            )
        }
    }
}
