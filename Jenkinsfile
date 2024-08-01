pipeline {
    agent any

    triggers {
        pollSCM('H/5 * * * *')
    }

    environment {
        SLACK_CHANNEL = 'C07EL1L7HAT'
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
        stage('Slack Notification') {
            steps {
                slackSend(
                    channel: "${env.SLACK_CHANNEL}",
                    color: '#FFFF00',
                    tokenCredentialId: "${env.SLACK_CREDENTIAL_ID}",
                    message: 'Build process completed.'
                )
            }
        }
    }

    post {
        success {
            slackSend (
                channel: "${env.SLACK_CHANNEL}", 
                color: 'good', 
                tokenCredentialId: "${env.SLACK_CREDENTIAL_ID}",
                message: "Build succeeded in ${env.JOB_NAME} #${env.BUILD_NUMBER} (<${env.BUILD_URL}|Open>)"
            )
        }
        failure {
            slackSend (
                channel: "${env.SLACK_CHANNEL}", 
                color: 'danger', 
                tokenCredentialId: "${env.SLACK_CREDENTIAL_ID}",
                message: "Build failed in ${env.JOB_NAME} #${env.BUILD_NUMBER} (<${env.BUILD_URL}|Open>)"
            )
        }
        unstable {
            slackSend (
                channel: "${env.SLACK_CHANNEL}", 
                color: 'warning', 
                tokenCredentialId: "${env.SLACK_CREDENTIAL_ID}",
                message: "Build is unstable in ${env.JOB_NAME} #${env.BUILD_NUMBER} (<${env.BUILD_URL}|Open>)"
            )
        }
        always {
            slackSend (
                channel: "${env.SLACK_CHANNEL}", 
                color: '#FFFF00', 
                tokenCredentialId: "${env.SLACK_CREDENTIAL_ID}",
                message: "Build completed in ${env.JOB_NAME} #${env.BUILD_NUMBER} (<${env.BUILD_URL}|Open>)"
            )
        }
    }
}
