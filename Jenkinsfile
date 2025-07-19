pipeline {
    agent any

    environment {
        AWS_DEFAULT_REGION = 'ap-south-1'
        S3_BUCKET = 'my-portfolio-bucket-for-project' 
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Hussai-N/PORTFOLIO.git'
            }
        }

        stage('Install AWS CLI (Optional)') {
            steps {
                bat '''
                aws --version || (
                    curl -o awscliv2.zip https://awscli.amazonaws.com/AWSCLIV2.msi
                    msiexec.exe /i awscliv2.zip /quiet
                )
                '''
            }
        }

        stage('Deploy to S3') {
            steps {
                withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', credentialsId: 'aws-s3-creds']]) {
                    bat '''
                    aws configure set aws_access_key_id %AWS_ACCESS_KEY_ID%
                    aws configure set aws_secret_access_key %AWS_SECRET_ACCESS_KEY%
                    aws s3 sync . s3://%S3_BUCKET% --delete
                    '''
                }
            }
        }
    }
}
