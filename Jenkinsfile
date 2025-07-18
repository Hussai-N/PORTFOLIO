pipeline {
    agent any

    environment {
        AWS_DEFAULT_REGION = 'ap-south-1'
        S3_BUCKET = 'my-portfolio-bucket-for-project' // update to your actual bucket name
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Hussai-N/PORTFOLIO.git'
            }
        }

        stage('Install AWS CLI (Optional)') {
            steps {
                sh '''
                if ! aws --version; then
                    curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
                    unzip awscliv2.zip
                    sudo ./aws/install
                fi
                '''
            }
        }

        stage('Deploy to S3') {
            steps {
                sh '''
                aws s3 sync . s3://$S3_BUCKET --delete
                '''
            }
        }
    }
}
