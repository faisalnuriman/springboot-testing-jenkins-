pipeline {
    agent any

    environment {
        SLACK_CHANNEL = 'C07EL1L7HAT'
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
    }

post {
    success {
        slackSend(
            channel: '#C07EL1L7HAT',
            message: "Pipeline sukses: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
            tokenCredentialId: 'slack-webhook-url'
        )
    }
    failure {
        slackSend(
            channel: '#C07EL1L7HAT',
            message: "Pipeline gagal: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
            tokenCredentialId: 'slack-webhook-url'
        )
    }
    unstable {
        slackSend(
            channel: '#C07EL1L7HAT',
            message: "Pipeline tidak stabil: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
            tokenCredentialId: 'slack-webhook-url'
        )
    }
}
