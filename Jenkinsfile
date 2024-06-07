pipeline {
    agent any

    environment {
        REPO_URL = 'https://github.com/VikashAdhikari/WebAppA.git'
        AWS_REGION = 'us-east-1'
        CODEDEPLOY_APPLICATION = 'EC2-On-premise-'
        CODEDEPLOY_DEPLOYMENT_GROUP = 'deployment'
        AWS_CREDENTIALS_ID = 'aws-credentials-id'  // Use your Jenkins AWS credentials ID
    }

    stages {
        stage('Clone Repository') {
            steps {
                git url: "${REPO_URL}", branch: 'main'
            }
        }

        stage('Prepare Deployment') {
            steps {
                script {
                    // Create a deployment directory and copy index.html
                    sh '''
                    mkdir -p deploy
                    cp index.html deploy/
                    cd deploy
                    zip -r ../deploy.zip .
                    '''
                }
            }
        }

        stage('Deploy with CodeDeploy') {
            steps {
                script {
                    withAWS(region: "${AWS_REGION}", credentials: "${AWS_CREDENTIALS_ID}") {
                        def deploymentId = awsCodeDeploy(
                            applicationName: "${CODEDEPLOY_APPLICATION}",
                            deploymentGroupName: "${CODEDEPLOY_DEPLOYMENT_GROUP}",
                            revisionType: 'github',
                            repositoryName: 'VikashAdhikari/WebAppA',
                            commitId: 'latest'
                        )
                        echo "Deployment started with ID: ${deploymentId}"
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
