pipeline {
    agent any

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
}

