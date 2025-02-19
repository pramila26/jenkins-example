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
