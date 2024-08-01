pipeline {
    agent any

    triggers {
        pollSCM('H/5 * * * *')
    }

    environment {
        SLACK_CHANNEL = 'jenkins'
        SLACK_CREDENTIAL_ID = 'slack-webhook-url'
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
    }
    
    post {
        success {
            slackSend (
                channel: "${env.SLACK_CHANNEL}", 
                color: 'good', 
                message: "Build succeeded in ${env.JOB_NAME} #${env.BUILD_NUMBER} (<${env.BUILD_URL}|Open>)"
            )
        }
        failure {
            slackSend (
                channel: "${env.SLACK_CHANNEL}", 
                color: 'danger', 
                message: "Build failed in ${env.JOB_NAME} #${env.BUILD_NUMBER} (<${env.BUILD_URL}|Open>)"
            )
        }
        unstable {
            slackSend (
                channel: "${env.SLACK_CHANNEL}", 
                color: 'warning', 
                message: "Build is unstable in ${env.JOB_NAME} #${env.BUILD_NUMBER} (<${env.BUILD_URL}|Open>)"
            )
        }
        always {
            slackSend (
                channel: "${env.SLACK_CHANNEL}", 
                color: '#FFFF00', 
                message: "Build completed in ${env.JOB_NAME} #${env.BUILD_NUMBER} (<${env.BUILD_URL}|Open>)"
            )
        }
    }
}