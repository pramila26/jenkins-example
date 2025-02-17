pipeline {
    agent any

    environment {
        PHP_SERVER_IP = '13.201.21.226'
        PHP_SERVER_PATH = '/var/www/html'
    }

    stages {
        stage('Checkout') {
            steps {
                git credentialsId: 'github_token', url: 'https://github.com/pramila26/jenkins-example.git'
            }
        }

        stage('Build') {
            steps {
                script {
                    echo 'Running build steps...'
                    // Add build steps if needed
                }
            }
        }

        stage('Deploy') {
            steps {
                sshagent(credentials: ['php-deploy']) {
                    sh """
                        scp -r * ubuntu@$PHP_SERVER_IP:$PHP_SERVER_PATH
                    """
                }
            }
        }
    }

    post {
        success {
            echo 'Deployment Successful!'
        }
        failure {
            echo 'Deployment Failed!'
        }
    }
}
