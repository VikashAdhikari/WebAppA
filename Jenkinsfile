pipeline {
    agent any

    environment {
        REPO_URL = 'https://github.com/VikashAdhikari/WebAppA.git'
        AWS_REGION = 'us-east-1'
        CODEDEPLOY_APPLICATION = 'EC2-On-premise-'
        CODEDEPLOY_DEPLOYMENT_GROUP = 'deployment'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git url: "${REPO_URL}", branch: 'main'
            }
        }

        stage('Prepare Deployment') {
            steps {
                sh 'cp index.html /usr/share/nginx/html/'
                sh 'sudo systemctl restart nginx'
            }
        }

        stage('Deploy with CodeDeploy') {
            steps {
                script {
                    withAWS(region: "${AWS_REGION}", credentials: 'aws-credentials-id') {
                        awsCodeDeploy(
                            applicationName: "${CODEDEPLOY_APPLICATION}",
                            deploymentGroupName: "${CODEDEPLOY_DEPLOYMENT_GROUP}",
                            revisionType: 'GitHub',  // Use 'GitHub' if revision is from GitHub
                            repositoryName: 'WebAppA',
                            commitId: 'latest'
                        )
                    }
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
