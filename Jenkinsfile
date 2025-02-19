pipeline {
    agent any

    environment {
        // Define your environment variables here, if any
        GIT_REPO = 'https://github.com/pramila26/jenkins-example.git'
        PHP_SERVER = 'ubuntu@13.201.21.226' // Update with actual PHP server IP
        DEPLOY_PATH = '/var/www/html/jenkins-example/' // Update with your project deployment path
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from the repository
                git branch: 'main', url: "${GIT_REPO}"
            }
        }

        stage('Build') {
            steps {
                // Example build step: here we assume you might want to run a PHP script or install dependencies
                echo 'Building the project...'
                // Add actual build steps here (e.g., running a PHP build, composer install, etc.)
            }
        }

        stage('Deploy') {
            steps {
                // Example deploy step
                echo 'Deploying the project...'
                // Add your deploy steps here (e.g., copying files to the server, etc.)
                script {
                    sshagent(['php-deploy']) {
                        sh """
                        ssh -o StrictHostKeyChecking=no $PHP_SERVER '
                        cd $DEPLOY_PATH &&
                        git stash &&
                        git pull origin main &&
                        git stash pop
                        '
                        """
                    }
                }
            }
        }

        stage('Post-Build Actions') {
            steps {
                echo 'Running post-build actions...'
                // Example post-build action like cleaning up or notifications
            }
        }
    }

    post {
        always {
            echo 'Cleaning up...'
            // Optional: Clean up if required
        }

        success {
            echo 'Pipeline succeeded!'
        }

        failure {
            echo 'Pipeline failed.'
        }
    }
}
