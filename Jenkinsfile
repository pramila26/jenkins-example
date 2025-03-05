pipeline {
    agent any

    // Define parameters to make the pipeline customizable
    parameters {
        // Define a string parameter for the PHP server IP address
        string(name: 'PHP_SERVER', defaultValue: 'ubuntu@13.201.21.226', description: 'The PHP Server IP')

        // Define a string parameter for the Git repository URL
        string(name: 'GIT_REPO', defaultValue: 'https://github.com/pramila26/jenkins-example.git', description: 'The Git Repository URL')

        // Define a string parameter for the deployment path
        string(name: 'DEPLOY_PATH', defaultValue: '/var/www/html/jenkins-example/', description: 'The deployment path on the PHP server')
    }

    environment {
        // You can still use the parameters in your environment section
        PHP_SERVER = "${params.PHP_SERVER}"
        GIT_REPO = "${params.GIT_REPO}"
        DEPLOY_PATH = "${params.DEPLOY_PATH}"
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from the repository
                git branch: 'main', url: "${GIT_REPO}"
            }
        }

        stage('Deploy') {
            steps {
                // Example deploy step
                echo "Deploying the project to $PHP_SERVER at $DEPLOY_PATH..."
                script {
                    sshagent(['php-deploy']) {
                        sh """
                        ssh -o StrictHostKeyChecking=no $PHP_SERVER '
                        cd $DEPLOY_PATH &&

                        # Stash changes only if there are any changes
                        git diff-index --quiet HEAD || git stash &&
                        
                        # Pull the latest changes from the 'main' branch
                        git pull origin main || { echo "git pull failed"; exit 1; } &&
                        
                        # Apply the stashed changes if any, otherwise print a message
                        git stash pop || echo "No stashed changes to apply"
                        '
                        """
                    }
                }
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
