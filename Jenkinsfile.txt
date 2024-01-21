pipeline {
    agent any

    environment {
        GIT_REPO_URL = 'https://github.com/keyboard-warrior77/d1.git'
        NGINX_PATH = 'C:\\Users\\Dev\\Desktop\\d1\\nginx-1.24.0\\htmldocs'
    }

    stages {
        stage('Checkout') {
            steps {
                script {
                    // Use the checkout step to clone the Git repository
                    checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: '9b53f564-a97d-42fb-ae13-b47a58e82ef2', url: 'https://github.com/keyboard-warrior77/d1.git']]])
                }
            }
        }

        stage('Deploy to Nginx') {
            steps {
                script {
                    // Using the Jenkins workspace variable to reference files
                    bat 'xcopy /y "C:\\Users\\Dev\\Desktop\\d2" "%NGINX_PATH%"'
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline succeeded! You can add additional steps here.'
        }
        failure {
            echo 'Pipeline failed! You may want to take some actions.'
        }
    }
}
