pipeline {
    agent any

    stages {
        stage('Clone Repository') {
            steps {
                script {
                    // Clone the Git repository
                    git url: 'https://github.com/VikashAdhikari/WebAppA.git', branch: 'main'
                }
            }
        }

        stage('Deploy with Nginx') {
            steps {
                script {
                    // Copy index.html to Nginx web root directory
                    sh 'cp index.html /usr/share/nginx/html/'
                    // Restart Nginx to apply changes
                    sh 'sudo systemctl restart nginx'
                }
            }
        }
    }

    post {
        success {
            echo 'Deployment successful'
        }
        failure {
            echo 'Deployment failed'
        }
    }
}
