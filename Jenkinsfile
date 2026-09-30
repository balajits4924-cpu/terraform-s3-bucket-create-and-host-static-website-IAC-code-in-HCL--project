pipeline {
    agent any

    stages {

        stage('Pull Code from GitHub') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/balajits4924-cpu/terraform-s3-bucket-create-and-host-static-website-IAC-code-in-HCL--project.git'
            }
        }

        stage('Terraform Init & Apply') {
            steps {
                withAWS(credentials: '71f378d2-ae79-45a3-b1ae-eb2cf895e5f4', region: 'us-east-1') {
                    sh 'terraform init'
                    sh 'terraform validate'
                    sh 'terraform apply -auto-approve'
                }
            }
        }

        stage('Upload Files to S3') {
            steps {
                withAWS(credentials: '71f378d2-ae79-45a3-b1ae-eb2cf895e5f4', region: 'us-east-1') {
                    sh '''
                        BUCKET_NAME="demojnk"

                        echo "Uploading files to S3 bucket: $BUCKET_NAME"

                        aws s3 sync ./ s3://$BUCKET_NAME \
                            --exclude ".git/*" \
                            --exclude ".terraform/*" \
                            --exclude "terraform.lock.hcl" \
                            --exclude "*.tf" \
                            --exclude "*.hcl" \
                            --exclude "Jenkinsfile" \
                            --exclude "*.md"
                    '''
                }
            }
        }
    }

    post {
        success {
            echo 'Static website deployment successful!'
            echo 'Files uploaded to S3 bucket: demojnk'
        }

        failure {
            echo 'Static website deployment failed!'
        }
    }
}
