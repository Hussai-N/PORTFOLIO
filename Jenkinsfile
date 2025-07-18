pipeline {
    agent any

    environment {
        AWS_DEFAULT_REGION = 'ap-south-1'
        S3_BUCKET = 'my-portfolio-bucket-for-project' // <-- Replace with your actual S3 bucket
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Hussai-N/PORTFOLIO.git'
            }
        }

        stage('Install AWS CLI') {
            steps {
                sh '''
                if ! command -v aws >/dev/null 2>&1; then
                    curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
                    unzip awscliv2.zip
                    sudo ./aws/install
                fi
                '''
            }
        }

        stage('Deploy to S3') {
            steps {
                withCredentials([[
                    $class: 'AmazonWebServicesCredentialsBinding',
                    credentialsId: 'aws-credentials-id' // <-- Replace with your Jenkins AWS credentials ID
                ]]) {
                    sh '''
                    aws s3 sync . s3://$S3_BUCKET \
                        --exclude ".git/*" \
                        --exclude "Jenkinsfile" \
                        --delete
                    '''
                }
            }
        }
    }
}
