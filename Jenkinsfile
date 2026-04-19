pipeline {
    agent { label 'AGENT-1'}
    environment {
        PROJECT = 'expense'
        COMPONENT = 'backend'
        appVersion = ''
        aws_account_id = '444152781209'
        //ACC_ID = ''
    }
    options {
        disableConcurrentBuilds()
        timeout(time: 30, unit:'MINUTES')
    }
    
    stages {
        stage('Read Version') {
            steps {
                script {
                def packageJson = readJSON file: 'package.json'
                appVersion = packageJson.version
                echo "Version is: $appVersion"
                }
            }

        }
        stage('install dependencies') {
            steps {
                script {
                    sh """
                        npm install
                    """
                }
            }

        }

        stage('docker build'){
            steps{
                script{
                withAWS(region: 'us-east-1', credentials: 'aws-creds') {
                    sh """
                    aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin ${aws_account_id}.dkr.ecr.us-east-1.amazonaws.com

                    docker build -t  ${aws_account_id}.dkr.ecr.us-east-1.amazonaws.com/${project}/${component}:${appVersion} .

                    docker push ${aws_account_id}.dkr.ecr.us-east-1.amazonaws.com/${project}/${component}:${appVersion}
                    """
                }
                 
               }
            }
        }
    }
    post {
            always {
                echo 'i will run if it is success or fail'
                deleteDir()
            }
            failure {
                echo 'i will run when pipeline fails'
            }
            success {
                echo 'i will run when pipeline success'
            }
        }
    }


