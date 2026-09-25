
pipeline {
    agent any

    stages {

        stage('Clone Code') {
            steps {
                echo 'Cloning code from GitHub...'
                checkout scm
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying website to Ubuntu...'

                sh '''
                    sudo cp index.html /var/www/html/
                    sudo cp style.css /var/www/html/
                '''
            }
        }

        stage('Restart Nginx') {
            steps {
                echo 'Restarting Nginx...'
                sh 'sudo systemctl restart nginx'
            }
        }
    }

    post {
        success {
            echo '✅ Website deployed successfully!'
        }

        failure {
            echo '❌ Deployment failed!'
        }
    }
}
