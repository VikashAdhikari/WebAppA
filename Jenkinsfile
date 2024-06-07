pipeline {
    agent any

    environment {
        REPO_URL = 'https://github.com/VikashAdhikari/WebAppA.git'
        AWS_REGION = 'us-east-1'
        CODEDEPLOY_APPLICATION = 'EC2-On-premise-'
        CODEDEPLOY_DEPLOYMENT_GROUP = 'deployment'
        AWS_CREDENTIALS_ID = 'aws-credentials-id'  // The ID you provided when adding the credentials
    }

    stages {
        stage('Clone Repository') {
            steps {
                git url: "${REPO_URL}", branch: 'main'
            }
        }

        stage('Deploy with CodeDeploy') {
            steps {
                script {
                    withAWS(region: "${AWS_REGION}", credentials: "${AWS_CREDENTIALS_ID}") {
                        awsCodeDeploy(
                            applicationName: "${CODEDEPLOY_APPLICATION}",
                            deploymentGroupName: "${CODEDEPLOY_DEPLOYMENT_GROUP}",
                            revisionType: 'github',
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
