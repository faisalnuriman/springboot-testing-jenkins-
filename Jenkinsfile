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
    }
    
    stages {
        stage('Slack Notification') {
            steps {
                slackSend(
                    channel: "${env.SLACK_CHANNEL}",
                    color: '#FFFF00',
                    tokenCredentialId: "${env.SLACK_CREDENTIAL_ID}",
                    message: 'Test notification'
                )
            }
        }
    }
}